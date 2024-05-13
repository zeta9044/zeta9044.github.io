---
layout: post
title: "ANTLR4 to JavaCC Conversion"
date: 2023-07-04
tags: [Conversion, JavaCC, to]
categories: blog
---

## 사용자 입력:
> lexer grammar XMLLexer;

// Default "mode": Everything OUTSIDE of a tag
COMMENT     :   '<!--' .*? '-->' ;
CDATA       :   '<![CDATA[' .*? ']]>' ;
/** Scarf all DTD stuff, Entity Declarations like <!ENTITY ...>,
 *  and Notation Declarations <!NOTATION ...>
 */
DTD         :   '<!' .*? '>'            -> skip ;
EntityRef   :   '&' Name ';' ;
CharRef     :   '&#' DIGIT+ ';'
            |   '&#x' HEXDIGIT+ ';'
            ;
SEA_WS      :   (' '|'\t'|'\r'? '\n')+ ;

OPEN        :   '<'                     -> pushMode(INSIDE) ;
XMLDeclOpen :   '<?xml' S               -> pushMode(INSIDE) ;
SPECIAL_OPEN:   '<?' Name               -> more, pushMode(PROC_INSTR) ;

TEXT        :   ~[<&]+ ;        // match any 16 bit char other than < and &

// ----------------- Everything INSIDE of a tag ---------------------
mode INSIDE;

CLOSE       :   '>'                     -> popMode ;
SPECIAL_CLOSE:  '?>'                    -> popMode ; // close <?xml...?>
SLASH_CLOSE :   '/>'                    -> popMode ;
SLASH       :   '/' ;
EQUALS      :   '=' ;
STRING      :   '"' ~[<"]* '"'
            |   '\'' ~[<']* '\''
            ;
Name        :   NameStartChar NameChar* ;
S           :   [ \t\r\n]               -> skip ;

fragment
HEXDIGIT    :   [a-fA-F0-9] ;

fragment
DIGIT       :   [0-9] ;

fragment
NameChar    :   NameStartChar
            |   '-' | '_' | '.' | DIGIT
            |   '\u00B7'
            |   '\u0300'..'\u036F'
            |   '\u203F'..'\u2040'
            ;

fragment
NameStartChar
            :   [:a-zA-Z]
            |   '\u2070'..'\u218F'
            |   '\u2C00'..'\u2FEF'
            |   '\u3001'..'\uD7FF'
            |   '\uF900'..'\uFDCF'
            |   '\uFDF0'..'\uFFFD'
            ;

// ----------------- Handle <? ... ?> ---------------------
mode PROC_INSTR;

PI          :   '?>'                    -> popMode ; // close <?...?>
IGNORE      :   .                       -> more ;
위 코드는 antlr4의 lexer정의이다. 위에 정의된 skip과 token만 javacc의 jj구문으로 바꿔어 줄 수 있나?

## ChatGPT 응답:
> ANTRL4과 JavaCC는 각각 다른 파서 생성기로, 둘 사이의 직접적인 변환은 쉽지 않습니다. 둘 다 각기 다른 문법을 사용하기 때문에 이를 수행하려면 각 토큰과 규칙을 개별적으로 변환해야 합니다. ANTRL4는 PEG(Parsing Expression Grammar)를 기반으로 한 LL(*) 파서를 생성하고, JavaCC는 LALR 파서를 생성합니다. 이 둘은 구문 분석 전략이 다릅니다.

그러나, 제시된 ANTRL4의 Lexer 규칙을 JavaCC의 JJ 문법으로 어느 정도 변환할 수 있습니다. 이는 정확한 변환에는 제한이 있을 수 있지만 아래에 간단한 예를 제시합니다. 

```java
TOKEN: {
    // 문자열들이 ANTRL4의 스킵을 대체. 해당 토큰은 무시됩니다.
    <DTD: "<!" (~["<"])* ">"> 
|   <S: (" " | "\t" | "\r" | "\n")+>

    // ANTRL4의 토큰 정의를 JavaCC로 대체합니다.
|   <COMMENT: "<!--" (~["<"])* "-->" >
|   <CDATA: "<![CDATA[" (~["<"])* "]]>" >
|   <EntityRef: "&" (~["<"])* ";" >
|   <CharRef: "&#" (~["<"])* ";" | "&#x" (~["<"])* ";" >
|   <OPEN: "<" >
|   <XMLDeclOpen: "<?xml" >
|   <SPECIAL_OPEN: "<?" (~["<"])* >
|   <TEXT: (~["&", "<"])+ >

    // mode INSIDE
|   <CLOSE: ">" >
|   <SPECIAL_CLOSE: "?>" >
|   <SLASH_CLOSE: "/>" >
|   <SLASH: "/" >
|   <EQUALS: "=" >
|   <STRING: "\"" (~["<"])* "\"" | "\'" (~["<"])* "\'" >
|   <Name: (["a"-"z", "A"-"Z"]) (["a"-"z", "A"-"Z", "-", "_", ".", "0"-"9", "\u00B7", "\u0300"-"u036F", "\u203F"-"u2040"])* >

    // mode PROC_INSTR
|   <PI: "?>" >
|   <IGNORE: ~[] >
}
```

위 코드는 대략적인 변환입니다. 특히, XML 토큰의 정규 표현식이나 ANTLR4의 `pushMode`나 `popMode`와 같은 상태 전환을 직접 지원하지 않는 JavaCC에서는 정확하게 처리하기 어렵습니다. 이런 변환은 수동으로 코드를 작성하고, 테스트하며, 필요한 경우 개별적으로 조정해야 합니다. 따라서, 이 작업은 시간과 노력이 상당히 소요될 수 있습니다.

## 사용자 입력:
> <PI: "?>" > 와  <SPECIAL_CLOSE: "?>" > 이게 중복이 되네.

## ChatGPT 응답:
> 네, 그렇습니다. JavaCC에서는 토큰 정의가 겹치면 먼저 정의된 토큰이 우선시 됩니다. 이 경우, `<PI: "?>" >`와 `<SPECIAL_CLOSE: "?>" >`는 동일한 문자열 패턴에 대해 정의되어 있으므로 문제가 될 수 있습니다.

ANTLR4에서의 모드 변경 기능이 JavaCC에는 없으므로 이 부분을 잘 처리하기 위해서는 다른 방식을 찾아야 합니다. 한 가지 방법은 상위 레벨의 파서 규칙에서 `PI`와 `SPECIAL_CLOSE`를 구별하는 것입니다. 예를 들어, 만약 `SPECIAL_CLOSE`가 항상 `XMLDeclOpen` 다음에 온다면, 파서에서 이 패턴을 확인하고 `PI` 대신 `SPECIAL_CLOSE`를 반환할 수 있습니다.

그러나 이런 종류의 해결책은 상당히 복잡하며 경우에 따라 불가능할 수도 있습니다. 결국 ANTLR4와 JavaCC는 매우 다른 도구로, 완전한 호환성은 보장되지 않습니다. 그래서 경우에 따라 두 도구 사이의 완전한 변환은 어려울 수 있습니다.

## 사용자 입력:
> package com.ids.parser.pentaho.parser;

public class PentahoParser {
      public static void main(String args[]) throws Exception {
      }
}

PARSER_END(PentahoParser)

TOKEN: {
    // 문자열들이 ANTRL4의 스킵을 대체. 해당 토큰은 무시됩니다.
    <DTD: "<!" (~["<"])* ">">
|   <S: (" " | "\t" | "\r" | "\n")+>

    // ANTRL4의 토큰 정의를 JavaCC로 대체합니다.
|   <COMMENT: "<!--" (~["<"])* "-->" >
|   <CDATA: "<![CDATA[" (~["<"])* "]]>" >
|   <EntityRef: "&" (~["<"])* ";" >
|   <CharRef: "&#" (~["<"])* ";" | "&#x" (~["<"])* ";" >
|   <OPEN: "<" >
|   <XMLDeclOpen: "<?xml" >
|   <SPECIAL_OPEN: "<?" (~["<"])* >
|   <TEXT: (~["&", "<"])+ >

    // mode INSIDE
|   <CLOSE: ">" >
|   <SPECIAL_CLOSE: "?>" >
|   <SLASH_CLOSE: "/>" >
|   <SLASH: "/" >
|   <EQUALS: "=" >
|   <STRING: "\"" (~["<"])* "\"" | "\'" (~["<"])* "\'" >
|   <Name: (["a"-"z", "A"-"Z"]) (["a"-"z", "A"-"Z", "-", "_", ".", "0"-"9", "\u00B7", "\u0300"-"\u036F", "\u203F"-"\u2040"])* >

    // mode PROC_INSTR
|   <IGNORE: ~[] >
}  위 코드는 구문정의를 제외한  XML용 javacc 토큰정의야.   다음은 anltr4로 만들어진 xml parser인데,  parser grammar XMLParser;

options { tokenVocab=XMLLexer; }

document    :   prolog? misc* element misc* EOF ;

prolog      :   XMLDeclOpen attribute* SPECIAL_CLOSE ;

content     :   chardata?
                ((element | reference | CDATA | PI | COMMENT) chardata?)* ;

element     :   '<' Name attribute* '>' content '<' '/' Name '>'
            |   '<' Name attribute* '/>'
            ;

reference   :   EntityRef | CharRef ;

attribute   :   Name '=' STRING ; // Our STRING is AttValue in spec

/** ``All text that is not markup constitutes the character data of
 *  the document.''
 */
chardata    :   TEXT | SEA_WS ;

misc        :   COMMENT | PI | SEA_WS ;
위 코드로 javacc용 구문을 완성해줘.

## ChatGPT 응답:
> ANTLR4에서 JavaCC로의 직접적인 변환은 불가능하다고 언급했듯이, 그러나 각 규칙을 적절히 변환하면 JavaCC에서 사용할 수 있는 JJ 파일을 만들 수 있습니다.

이 변환의 주요 어려움 중 하나는 ANTLR4가 우선적으로 사용하는 LL(*) 파싱 전략을 JavaCC의 LALR 전략으로 바꾸는 것입니다. 아래는 위의 ANTLR4 파서 규칙들을 가능한 한 잘 JavaCC 스타일로 바꾼 것입니다:

```java
PARSER_BEGIN(PentahoParser)

package com.ids.parser.pentaho.parser;

public class PentahoParser {
      public static void main(String args[]) throws Exception {
      }
}

PARSER_END(PentahoParser)

TOKEN: {
    // ANTRL4 토큰 정의가 위에서 주어짐
}

void document() : {} {
    [prolog()] (misc())* element() (misc())* <EOF>
}

void prolog() : {} {
    <XMLDeclOpen> (attribute())* <SPECIAL_CLOSE>
}

void content() : {} {
    [chardata()]
    (
        (
            element() | reference() | <CDATA> | <PI> | <COMMENT>
        )
        [chardata()]
    )*
}

void element() : {} {
    <OPEN> <Name> (attribute())* <CLOSE> content() <OPEN> <SLASH> <Name> <CLOSE>
    |
    <OPEN> <Name> (attribute())* <SLASH_CLOSE>
}

void reference() : {} {
    <EntityRef> | <CharRef>
}

void attribute() : {} {
    <Name> <EQUALS> <STRING>
}

void chardata() : {} {
    <TEXT> | <SEA_WS>
}

void misc() : {} {
    <COMMENT> | <PI> | <SEA_WS>
}
```

이 JavaCC 코드는 완벽하지 않을 수 있으며, 실제로 파서가 원하는 대로 동작하도록 제대로 조정하고 테스트하는 것은 개발자의 책임입니다. 또한, JavaCC는 ANTLR4가 제공하는 몇 가지 고급 기능을 지원하지 않을 수 있으므로, 그에 대한 적절한 해결책을 찾는 것이 필요할 수 있습니다.

