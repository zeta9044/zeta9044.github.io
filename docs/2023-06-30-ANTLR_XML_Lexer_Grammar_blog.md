---
layout: post
title: "ANTLR XML Lexer Grammar"
date: 2023-06-30
tags: [XML, Lexer, Grammar, ANTLR]
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
IGNORE      :   .                       -> more ;   이거 javacc의 jj로 바꿔줘.

## ChatGPT 응답:
> I see that you have provided a lexer grammar in the format of ANTLR (ANTLR Parser Generator). However, I noticed that the last line contains a comment that seems to be related to another parser generator called Java Compiler Compiler (JavaCC).

If you intend to use ANTLR for generating a lexer/parser for XML, the grammar you provided seems to be incomplete. Typically, an ANTLR grammar consists of lexer rules and parser rules. The lexer rules define how to tokenize the input, while the parser rules define the structure and meaning of the language.

Based on the lexer rules you provided, it seems that you are defining a lexer grammar for XML. Lexer rules in ANTLR start with a capital letter and define how to recognize and tokenize specific patterns in the input.

Here's an example of a complete XML lexer grammar in ANTLR:

```antlr
lexer grammar XMLLexer;

COMMENT     :   '<!--' .*? '-->' ;
CDATA       :   '<![CDATA[' .*? ']]>' ;
DTD         :   '<!' .*? '>'            -> skip ;
EntityRef   :   '&' Name ';' ;
CharRef     :   '&#' DIGIT+ ';'
            |   '&#x' HEXDIGIT+ ';'
            ;
SEA_WS      :   (' ' | '\t' | '\r'? '\n')+ ;

OPEN        :   '<'                     -> pushMode(INSIDE) ;
XMLDeclOpen :   '<?xml' S               -> pushMode(INSIDE) ;
SPECIAL_OPEN:   '<?' Name               -> more, pushMode(PROC_INSTR) ;

TEXT        :   ~[<&]+ ;

mode INSIDE;

CLOSE       :   '>'                     -> popMode ;
SPECIAL_CLOSE:  '?>'                    -> popMode ;
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

mode PROC_INSTR;

PI          :   '?>'                    -> popMode ;
IGNORE      :   .                       -> more ;
```

Please note that this grammar only defines the lexer rules for XML and does not include parser rules. If you need to define parser rules as well, you would create a separate parser grammar file.

