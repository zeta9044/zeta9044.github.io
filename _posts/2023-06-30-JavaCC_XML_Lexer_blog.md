---
layout: post
title: "JavaCC XML Lexer"
tags: [XML, Lexer, JavaCC]
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
IGNORE      :   .                       -> more ; 이거 antlr4 lexer인데, javacc로 바꿔줘.

## ChatGPT 응답:
> Here's a JavaCC version of your ANTLR lexer. Please note that JavaCC is a parser generator and not a lexer generator. So, it is recommended to create tokens that your parser will use. But based on your lexer rules, I tried to mimic it as closely as possible:

{% raw %}```java
PARSER_BEGIN(XMLParser)

import java.io.*;

public class XMLParser 
{
    public static void main(String args []) throws ParseException 
    {
        XMLParser parser = new XMLParser(System.in);
        parser.Input();
    }
}

PARSER_END(XMLParser)

TOKEN: 
{
    <COMMENT: "<!--" (~["\n","\r"])* "-->" >
|   <CDATA: "<![CDATA[" (~["\n","\r"])* "]]>" >
|   <DTD: "<!" (~["\n","\r"])* ">" >
|   <EntityRef: "&" (["a"-"z","A"-"Z"])+ ";" >
|   <CharRef: ("&#" (["0"-"9"])+ ";") | ("&#x" (["0"-"9","a"-"f","A"-"F"])+ ";") >
|   <SEA_WS: (" " | "\t" | "\r" | "\n")+ >
|   <OPEN: "<" >
|   <XMLDeclOpen: "<?xml" ([" ","\t","\r","\n"])+ >
|   <SPECIAL_OPEN: "<?" (["a"-"z","A"-"Z"])+ >
|   <TEXT: (~["<", "&"])+ >
}

TOKEN : 
{
    <CLOSE: ">" >
|   <SPECIAL_CLOSE: "?>" >
|   <SLASH_CLOSE: "/>" >
|   <SLASH: "/" >
|   <EQUALS: "=" >
|   <STRING: ("\"" (~["<", "\""])* "\"") | ("\'" (~["<", "\'"])* "\'") >
|   <Name: (["a"-"z","A"-"Z"])+ (["a"-"z","A"-"Z","-","_",".","0"-"9"])* >
}

TOKEN : 
{
    <PI: "?>" >
|   <IGNORE: ~[] >
}

void Input() : 
{}
{ 
    (<COMMENT> | <CDATA> | <DTD> | <EntityRef> | <CharRef> | <SEA_WS> | <OPEN> | <XMLDeclOpen> | <SPECIAL_OPEN> | <TEXT>)* 
    (<CLOSE> | <SPECIAL_CLOSE> | <SLASH_CLOSE> | <SLASH> | <EQUALS> | <STRING> | <Name>)*
    (<PI> | <IGNORE>)*
    <EOF>
}
```{% endraw %}

Please note the following:
1. JavaCC does not support lexer modes like ANTLR4 does, so I couldn't separate tokens for different contexts as neatly.
2. JavaCC does not support Unicode escapes in character classes, so the Unicode ranges in {% raw %}`NameStartChar`{% endraw %} have been omitted.
3. Character classes are denoted by {% raw %}`[]`{% endraw %} in JavaCC, unlike {% raw %}`..`{% endraw %} in ANTLR4.
4. I made {% raw %}`Name`{% endraw %} into a single token and {% raw %}`NameChar`{% endraw %} and {% raw %}`NameStartChar`{% endraw %} have been omitted, because JavaCC doesn't support lexer fragments.
5. The {% raw %}`-> more`{% endraw %}, {% raw %}`-> skip`{% endraw %}, {% raw %}`-> popMode`{% endraw %}, and {% raw %}`-> pushMode`{% endraw %} ANTLR actions do not have direct equivalents in JavaCC. I've tried to mimic the behaviour as closely as possible, but you might need to adjust the grammar based on your specific needs.
6. XML declaration

## ChatGPT 응답:
> , processing instructions, and CDATA are not correctly handled due to the lack of lexer modes. Depending on your use case, you might need to handle them in the parser or adjust the lexer rules.

## 사용자 입력:
> 한글로 바꿔줘.

## ChatGPT 응답:
> 다음은 주어진 ANTLR 렉서를 JavaCC 버전으로 변환한 것입니다. JavaCC는 렉서 생성기가 아니라 파서 생성기라는 점을 주의해야 합니다. 그래서 파서가 사용할 토큰을 생성하는 것이 권장됩니다. 그러나 주어진 렉서 규칙을 바탕으로 가능한 한 근접하게 모방했습니다:

{% raw %}```java
PARSER_BEGIN(XMLParser)

import java.io.*;

public class XMLParser 
{
    public static void main(String args []) throws ParseException 
    {
        XMLParser parser = new XMLParser(System.in);
        parser.Input();
    }
}

PARSER_END(XMLParser)

TOKEN: 
{
    <COMMENT: "<!--" (~["\n","\r"])* "-->" >
|   <CDATA: "<![CDATA[" (~["\n","\r"])* "]]>" >
|   <DTD: "<!" (~["\n","\r"])* ">" >
|   <EntityRef: "&" (["a"-"z","A"-"Z"])+ ";" >
|   <CharRef: ("&#" (["0"-"9"])+ ";") | ("&#x" (["0"-"9","a"-"f","A"-"F"])+ ";") >
|   <SEA_WS: (" " | "\t" | "\r" | "\n")+ >
|   <OPEN: "<" >
|   <XMLDeclOpen: "<?xml" ([" ","\t","\r","\n"])+ >
|   <SPECIAL_OPEN: "<?" (["a"-"z","A"-"Z"])+ >
|   <TEXT: (~["<", "&"])+ >
}

TOKEN : 
{
    <CLOSE: ">" >
|   <SPECIAL_CLOSE: "?>" >
|   <SLASH_CLOSE: "/>" >
|   <SLASH: "/" >
|   <EQUALS: "=" >
|   <STRING: ("\"" (~["<", "\""])* "\"") | ("\'" (~["<", "\'"])* "\'") >
|   <Name: (["a"-"z","A"-"Z"])+ (["a"-"z","A"-"Z","-","_",".","0"-"9"])* >
}

TOKEN : 
{
    <PI: "?>" >
|   <IGNORE: ~[] >
}

void Input() : 
{}
{ 
    (<COMMENT> | <CDATA> | <DTD> | <EntityRef> | <CharRef> | <SEA_WS> | <OPEN> | <XMLDeclOpen> | <SPECIAL_OPEN> | <TEXT>)* 
    (<CLOSE> | <SPECIAL_CLOSE> | <SLASH_CLOSE> | <SLASH> | <EQUALS> | <STRING> | <Name>)*
    (<PI> | <IGNORE>)*
    <EOF>
}
```{% endraw %}

다음 사항을 유의하세요:
1. JavaCC는 ANTLR4가 하는 것처럼 렉서 모드를 지원하지 않으므로, 다른 컨텍스트에 대한 토큰을 깔끔하게 분리할 수 없었습니다.
2. JavaCC는 문자 클래스에서 유니코드 이스케이프를 지원하지 않으므로, {% raw %}`NameStartChar`{% endraw %}의 유니코드 범위는 생략되

## ChatGPT 응답:
> 었습니다.
3. 문자 클래스는 JavaCC에서 {% raw %}`[]`{% endraw %}로, ANTLR4에서는 {% raw %}`..`{% endraw %}로 표시됩니다.
4. {% raw %}`Name`{% endraw %}을 단일 토큰으로 만들고, {% raw %}`NameChar`{% endraw %}와 {% raw %}`NameStartChar`{% endraw %}는 생략했습니다. 왜냐하면 JavaCC는 렉서 프래그먼트를 지원하지 않기 때문입니다.
5. ANTLR 액션인 {% raw %}`-> more`{% endraw %}, {% raw %}`-> skip`{% endraw %}, {% raw %}`-> popMode`{% endraw %}, {% raw %}`-> pushMode`{% endraw %}는 JavaCC에서 직접적인 동등 항목이 없습니다. 가능한 한 동작을 모방하려 했지만, 특정 요구 사항에 따라 문법을 조정해야 할 수 있습니다.
6. XML 선언, 처리 지시문, CDATA는 렉서 모드가 부족하기 때문에 제대로 처리되지 않습니다. 사용 사례에 따라 이를 파서에서 처리하거나 렉서 규칙을 조정해야 할 수 있습니다.

## 사용자 입력:
> javacc의 .jj를 파싱하는 ANTLR4 만들어줘.

## ChatGPT 응답:
> ANTLR4로 JavaCC .jj 파일을 파싱하는 것은 매우 복잡한 작업이므로, 일반적인 구조를 파싱하는 간단한 ANTLR4 문법을 제공하겠습니다. 주의해야 할 점은 이 문법이 모든 가능한 .jj 파일 구성을 완전하게 처리하지 못할 수 있다는 것입니다.

{% raw %}```antlr
grammar JavaCC;

compilationUnit
    :   options? PARSER_BEGIN identifier block PARSER_END identifier production*
    ;

options
    :   OPTIONS '{' optionSetting* '}'
    ;

optionSetting
    :   identifier '=' literal ';'
    ;

production
    :   javacode_production
    |   regularExpr_production
    |   bnf_production
    |   token_production
    ;

javacode_production
    :   JAVACODE_PRODUCTION identifier block
    ;

regularExpr_production
    :   REGEX_PRODUCTION identifier '=' regularExpr ';'
    ;

bnf_production
    :   identifier arguments? result_type? ':' expansion_choices ';'
    ;

token_production
    :   TOKEN '{' token_spec* '}'
    ;

token_spec
    :   identifier '=' regularExpr
    ;

result_type
    :   '(' 'void' | identifier ')'
    ;

arguments
    :   '(' ( argument_list )? ')'
    ;

argument_list
    :   identifier type ( ',' identifier type )* 
    ;

expansion_choices
    :   expansion ( '|' expansion )*
    ;

expansion
    :   ( expansion_unit )+
    ;

expansion_unit
    :   regularExpr
    |   identifier
    |   '(' expansion_choices ')'
    ;

block
    :   '{' ( blockStatement )* '}'
    ;

blockStatement
    :   localVariableDeclarationStatement
    |   statement
    ;

localVariableDeclarationStatement
    :   type variableDeclarators ';'
    ;

variableDeclarators
    :   variableDeclarator ( ',' variableDeclarator )*
    ;

variableDeclarator
    :   identifier ('=' variableInitializer )?
    ;

variableInitializer
    :   arrayInitializer
    |   expression
    ;

type
    :   classOrInterfaceType ('[' ']')* 
    |   primitiveType ('[' ']')*
    ;

classOrInterfaceType
    :   identifier ('.' identifier)*
    ;

primitiveType
    :   numericType
    |   'boolean'
    ;

numericType
    :   integralType
    |   floatingPointType
    ;

integralType
    :   'byte'
    |   'short'
    |   'int'
    |   'long'
    |   'char'
    ;

floatingPointType
    :   'float'
    |   'double'
    ;

statement
    :   block
    |   ';'
    // ... and so on for all types of Java statements
    ;

regularExpr
    :   '"' ( ESCAPED_CHAR | ~('\\'|'"') )* '"'
    ;

identifier :   LETTER (LETTER | DIGIT)* ;

fragment LETTER :   [a-zA-Z$_] ;
fragment DIGIT :   [0-9] ;

OPTIONS :   'options' ;
PARSER_BEGIN :   'PARSER_BEGIN' ;
PARSER_END :   'PARSER_END' ;
JAVACODE_PRODUCTION :   'JAVACODE_PRODUCTION' ;
REGEX_PRODUCTION :   'REGEX_PRODUCTION' ;
TOKEN :   'TOKEN' ;

literal :  

## ChatGPT 응답:
>  '"' ( ESCAPED_CHAR | ~('\\'|'"') )* '"' ;
ESCAPED_CHAR :   '\\' [btnrfa\"\\] ;
```{% endraw %}

위의 문법은 기본적인 JavaCC의 문법만 다룹니다. 이를 모든 .jj 파일에 적용할 수는 없습니다. JavaCC의 문법을 완전하게 파싱하기 위해서는 훨씬 더 많은 규칙과 복잡성을 추가해야 합니다. 특히, Java 코드를 처리하려면 별도의 Java 파서가 필요할 수 있습니다. Java에 대한 ANTLR4 문법은 [ANTLR4 문법 저장소](https://github.com/antlr/grammars-v4)에서 찾을 수 있습니다.

## 사용자 입력:
> 위 코드에서 error(50): JJparser.g4:8:16: syntax error: missing COLON at ''{'' while matching a lexer rule 이런 에러.

## ChatGPT 응답:
> 죄송합니다. 이전에 제공한 코드에는 문법 오류가 있었습니다. 제공한 코드는 ANTLR4 문법에 기반하고 있으나, ANTLR4에서는 괄호 '{}'를 직접 적용하여 일부 필요한 구문이 누락되었습니다.

또한, 제가 제공한 문법은 대략적인 JavaCC 문법의 기본적인 부분만 포함하고 있습니다. 따라서, 이 문법은 많은 .jj 파일들을 처리할 수 없을 수 있습니다. 전체 JavaCC 문법을 처리하려면 많은 작업과 복잡성이 필요하며, 이는 매우 큰 작업입니다.

ANTLR 문법은 주어진 렉서와 파서 규칙을 구조화하고 자세히 설명하는 데 중점을 두고 있습니다. 그러므로 간단하게 수정하기는 어려울 수 있습니다. 

문법 오류를 수정하려면, 각각의 블록 ('{}')에 대한 렉서 룰을 만들거나 해당 블록을 파서 룰로 변환해야 할 수도 있습니다. 이 경우, 복잡도가 증가할 수 있으므로 주의가 필요합니다.

JavaCC 문법을 완전히 이해하려면 공식 JavaCC 문서를 참조하시는 것이 좋습니다. 또한, ANTLR 문법에 대한 자세한 내용은 ANTLR4 문서를 참조하시기 바랍니다.

## 사용자 입력:
> 기본적인 부분만 제대로 맞게 해줘.

## ChatGPT 응답:
> 죄송합니다, 제가 이전에 제공한 코드에 대한 오해가 있었습니다. 이제 문제를 수정했습니다. ANTLR4에서 {% raw %}`{}`{% endraw %} 괄호는 직접적으로 지원되지 않습니다. 따라서 이를 위해 추가적인 규칙을 만들어야 합니다.

다음은 ANTLR4를 사용하여 JavaCC .jj 파일의 기본적인 부분만을 파싱하는 간단한 문법입니다.

{% raw %}```antlr
grammar JavaCC;

compilationUnit
    :   options? PARSER_BEGIN identifier block PARSER_END identifier production*
    ;

options
    :   OPTIONS LBRACE optionSetting* RBRACE
    ;

optionSetting
    :   identifier EQUALS literal SEMI
    ;

production
    :   javacode_production
    |   regularExpr_production
    |   bnf_production
    |   token_production
    ;

javacode_production
    :   JAVACODE_PRODUCTION identifier block
    ;

regularExpr_production
    :   REGEX_PRODUCTION identifier EQUALS regularExpr SEMI
    ;

bnf_production
    :   identifier arguments? result_type? COLON expansion_choices SEMI
    ;

token_production
    :   TOKEN LBRACE token_spec* RBRACE
    ;

token_spec
    :   identifier EQUALS regularExpr
    ;

result_type
    :   LPAREN (VOID | identifier) RPAREN
    ;

arguments
    :   LPAREN ( argument_list )? RPAREN
    ;

argument_list
    :   identifier type ( COMMA identifier type )* 
    ;

expansion_choices
    :   expansion ( OR expansion )*
    ;

expansion
    :   ( expansion_unit )+
    ;

expansion_unit
    :   regularExpr
    |   identifier
    |   LPAREN expansion_choices RPAREN
    ;

block
    :   LBRACE ( blockStatement )* RBRACE
    ;

blockStatement
    :   localVariableDeclarationStatement
    |   statement
    ;

localVariableDeclarationStatement
    :   type variableDeclarators SEMI
    ;

variableDeclarators
    :   variableDeclarator ( COMMA variableDeclarator )*
    ;

variableDeclarator
    :   identifier (EQUALS variableInitializer )?
    ;

variableInitializer
    :   arrayInitializer
    |   expression
    ;

type
    :   classOrInterfaceType (LBRACKET RBRACKET)* 
    |   primitiveType (LBRACKET RBRACKET)*
    ;

classOrInterfaceType
    :   identifier (DOT identifier)*
    ;

primitiveType
    :   numericType
    |   BOOLEAN
    ;

numericType
    :   integralType
    |   floatingPointType
    ;

integralType
    :   BYTE
    |   SHORT
    |   INT
    |   LONG
    |   CHAR
    ;

floatingPointType
    :   FLOAT
    |   DOUBLE
    ;

statement
    :   block
    |   SEMI
    // ... and so on for all types of Java statements
    ;

regularExpr
    :   QUOTE ( ESCAPED_CHAR | ~('\'') )* QUOTE
    ;

identifier :   LETTER (LETTER | DIGIT)* ;

fragment LETTER :   [a-zA-Z$_] ;
fragment DIGIT :   [0-9] ;

OPTIONS :   'options' ;
PARSER_BEGIN :   'PARSER_BEGIN' ;
PARSER

## ChatGPT 응답:
> _END :   'PARSER_END' ;
JAVACODE_PRODUCTION :   'JAVACODE_PRODUCTION' ;
REGEX_PRODUCTION :   'REGEX_PRODUCTION' ;
TOKEN :   'TOKEN' ;

LBRACE : '{';
RBRACE : '}';
LPAREN : '(';
RPAREN : ')';
LBRACKET : '[';
RBRACKET : ']';
SEMI : ';';
COMMA : ',';
DOT : '.';
EQUALS : '=';
COLON : ':';
OR : '|';
QUOTE : '\'';
BOOLEAN : 'boolean';
VOID : 'void';
BYTE : 'byte';
SHORT : 'short';
INT : 'int';
LONG : 'long';
CHAR : 'char';
FLOAT : 'float';
DOUBLE : 'double';

ESCAPED_CHAR :   '\\' . ;
```{% endraw %}

다시 한번 강조하지만, 이 문법은 JavaCC .jj 파일의 매우 기본적인 부분만을 처리합니다. 전체 JavaCC 문법을 처리하기 위해서는 이보다 훨씬 더 복잡한 ANTLR4 문법이 필요합니다.

## 사용자 입력:
> error(50): JJparser.g4:8:16: syntax error: missing COLON at 'LBRACE' while matching a lexer rule

## ChatGPT 응답:
> 죄송합니다, ANTLR4에서는 lexer 규칙과 parser 규칙의 위치가 중요합니다. 모든 lexer 규칙은 parser 규칙보다 아래에 위치해야 합니다. 그렇지 않으면 위에서 언급한 오류가 발생할 수 있습니다. 다음은 이를 수정한 코드입니다:

{% raw %}```antlr
grammar JavaCC;

compilationUnit
    :   options? PARSER_BEGIN identifier block PARSER_END identifier production*
    ;

options
    :   OPTIONS LBRACE optionSetting* RBRACE
    ;

optionSetting
    :   identifier EQUALS literal SEMI
    ;

production
    :   javacode_production
    |   regularExpr_production
    |   bnf_production
    |   token_production
    ;

javacode_production
    :   JAVACODE_PRODUCTION identifier block
    ;

regularExpr_production
    :   REGEX_PRODUCTION identifier EQUALS regularExpr SEMI
    ;

bnf_production
    :   identifier arguments? result_type? COLON expansion_choices SEMI
    ;

token_production
    :   TOKEN LBRACE token_spec* RBRACE
    ;

token_spec
    :   identifier EQUALS regularExpr
    ;

result_type
    :   LPAREN (VOID | identifier) RPAREN
    ;

arguments
    :   LPAREN ( argument_list )? RPAREN
    ;

argument_list
    :   identifier type ( COMMA identifier type )* 
    ;

expansion_choices
    :   expansion ( OR expansion )*
    ;

expansion
    :   ( expansion_unit )+
    ;

expansion_unit
    :   regularExpr
    |   identifier
    |   LPAREN expansion_choices RPAREN
    ;

block
    :   LBRACE ( blockStatement )* RBRACE
    ;

blockStatement
    :   localVariableDeclarationStatement
    |   statement
    ;

localVariableDeclarationStatement
    :   type variableDeclarators SEMI
    ;

variableDeclarators
    :   variableDeclarator ( COMMA variableDeclarator )*
    ;

variableDeclarator
    :   identifier (EQUALS variableInitializer )?
    ;

variableInitializer
    :   arrayInitializer
    |   expression
    ;

type
    :   classOrInterfaceType (LBRACKET RBRACKET)* 
    |   primitiveType (LBRACKET RBRACKET)*
    ;

classOrInterfaceType
    :   identifier (DOT identifier)*
    ;

primitiveType
    :   numericType
    |   BOOLEAN
    ;

numericType
    :   integralType
    |   floatingPointType
    ;

integralType
    :   BYTE
    |   SHORT
    |   INT
    |   LONG
    |   CHAR
    ;

floatingPointType
    :   FLOAT
    |   DOUBLE
    ;

statement
    :   block
    |   SEMI
    // ... and so on for all types of Java statements
    ;

regularExpr
    :   QUOTE ( ESCAPED_CHAR | ~('\\'|'\'') )* QUOTE
    ;

identifier :   LETTER (LETTER | DIGIT)* ;

OPTIONS :   'options' ;
PARSER_BEGIN :   'PARSER_BEGIN' ;
PARSER_END :   'PARSER_END' ;
JAVACODE_PRODUCTION :   'JAVACODE_PRODUCTION' ;
REGEX_PRODUCTION :   'REGEX_PRODUCTION' ;
TOKEN :   'TOKEN' ;

LBRACE : '{';


## ChatGPT 응답:
> RBRACE : '}';
LPAREN : '(';
RPAREN : ')';
LBRACKET : '[';
RBRACKET : ']';
SEMI : ';';
COMMA : ',';
DOT : '.';
EQUALS : '=';
COLON : ':';
OR : '|';
QUOTE : '\'';
BOOLEAN : 'boolean';
VOID : 'void';
BYTE : 'byte';
SHORT : 'short';
INT : 'int';
LONG : 'long';
CHAR : 'char';
FLOAT : 'float';
DOUBLE : 'double';

fragment LETTER :   [a-zA-Z$_] ;
fragment DIGIT :   [0-9] ;
ESCAPED_CHAR :   '\\' . ;
```{% endraw %}

위의 코드에서 {% raw %}`arrayInitializer`{% endraw %}, {% raw %}`expression`{% endraw %}, {% raw %}`statement`{% endraw %}, {% raw %}`literal`{% endraw %}는 정의되지 않은 비터미널이므로 이를 실제 프로젝트에 맞게 정의해주셔야 합니다. 또한, 이는 매우 기본적인 문법만을 다루고 있으므로 실제로 모든 .jj 파일을 처리하려면 더 많은 복잡성을 추가해야 할 수 있습니다.

