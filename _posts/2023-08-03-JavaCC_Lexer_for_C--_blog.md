---
layout: post
title: "JavaCC Lexer for C++"
tags: [C, Lexer, for, JavaCC]
---

## 사용자 입력:
> IntegerLiteral:
	DecimalLiteral Integersuffix?
	| OctalLiteral Integersuffix?
	| HexadecimalLiteral Integersuffix?
	| BinaryLiteral Integersuffix?;

CharacterLiteral:
	('u' | 'U' | 'L')? '\'' Cchar+ '\'';

FloatingLiteral:
	Fractionalconstant Exponentpart? Floatingsuffix?
	| Digitsequence Exponentpart Floatingsuffix?;

StringLiteral:
	Encodingprefix?
    (Rawstring
	|'"' Schar* '"');

BooleanLiteral: False_ | True_;

PointerLiteral: Nullptr;

UserDefinedLiteral:
	UserDefinedIntegerLiteral
	| UserDefinedFloatingLiteral
	| UserDefinedStringLiteral
	| UserDefinedCharacterLiteral;


/*Keywords*/

Alignas: 'alignas';

Alignof: 'alignof';

Asm: 'asm';

Auto: 'auto';

Bool: 'bool';

Break: 'break';

Case: 'case';

Catch: 'catch';

Char: 'char';

Char16: 'char16_t';

Char32: 'char32_t';

Class: 'class';

Const: 'const';

Constexpr: 'constexpr';

Const_cast: 'const_cast';

Continue: 'continue';

Decltype: 'decltype';

Default: 'default';

Delete: 'delete';

Do: 'do';

Double: 'double';

Dynamic_cast: 'dynamic_cast';

Else: 'else';

Enum: 'enum';

Explicit: 'explicit';

Export: 'export';

Extern: 'extern';

False_: 'false';

Final: 'final';

Float: 'float';

For: 'for';

Friend: 'friend';

Goto: 'goto';

If: 'if';

Inline: 'inline';

Int: 'int';

Long: 'long';

Mutable: 'mutable';

Namespace: 'namespace';

New: 'new';

Noexcept: 'noexcept';

Nullptr: 'nullptr';

Operator: 'operator';

Override: 'override';

Private: 'private';

Protected: 'protected';

Public: 'public';

Register: 'register';

Reinterpret_cast: 'reinterpret_cast';

Return: 'return';

Short: 'short';

Signed: 'signed';

Sizeof: 'sizeof';

Static: 'static';

Static_assert: 'static_assert';

Static_cast: 'static_cast';

Struct: 'struct';

Switch: 'switch';

Template: 'template';

This: 'this';

Thread_local: 'thread_local';

Throw: 'throw';

True_: 'true';

Try: 'try';

Typedef: 'typedef';

Typeid_: 'typeid';

Typename_: 'typename';

Union: 'union';

Unsigned: 'unsigned';

Using: 'using';

Virtual: 'virtual';

Void: 'void';

Volatile: 'volatile';

Wchar: 'wchar_t';

While: 'while';

/*Operators*/

LeftParen: '(';

RightParen: ')';

LeftBracket: '[';

RightBracket: ']';

LeftBrace: '{';

RightBrace: '}';

Plus: '+';

Minus: '-';

Star: '*';

Div: '/';

Mod: '%';

Caret: '^';

And: '&';

Or: '|';

Tilde: '~';

Not: '!' | 'not';

Assign: '=';

Less: '<';

Greater: '>';

PlusAssign: '+=';

MinusAssign: '-=';

StarAssign: '*=';

DivAssign: '/=';

ModAssign: '%=';

XorAssign: '^=';

AndAssign: '&=';

OrAssign: '|=';

LeftShiftAssign: '<<=';

RightShiftAssign: '>>=';

Equal: '==';

NotEqual: '!=';

LessEqual: '<=';

GreaterEqual: '>=';

AndAnd: '&&' | 'and';

OrOr: '||' | 'or';

PlusPlus: '++';

MinusMinus: '--';

Comma: ',';

ArrowStar: '->*';

Arrow: '->';

Question: '?';

Colon: ':';

Doublecolon: '::';

Semi: ';';

Dot: '.';

DotStar: '.*';

Ellipsis: '...';

fragment Hexquad:
	HEXADECIMALDIGIT HEXADECIMALDIGIT HEXADECIMALDIGIT HEXADECIMALDIGIT;

fragment Universalcharactername:
	'\\u' Hexquad
	| '\\U' Hexquad Hexquad;

Identifier:
	/*
	 Identifiernondigit | Identifier Identifiernondigit | Identifier DIGIT
	 */
	Identifiernondigit (Identifiernondigit | DIGIT)*;

fragment Identifiernondigit: NONDIGIT | Universalcharactername;

fragment NONDIGIT: [a-zA-Z_];

fragment DIGIT: [0-9];

DecimalLiteral: NONZERODIGIT ('\''? DIGIT)*;

OctalLiteral: '0' ('\''? OCTALDIGIT)*;

HexadecimalLiteral: ('0x' | '0X') HEXADECIMALDIGIT (
		'\''? HEXADECIMALDIGIT
	)*;

BinaryLiteral: ('0b' | '0B') BINARYDIGIT ('\''? BINARYDIGIT)*;

fragment NONZERODIGIT: [1-9];

fragment OCTALDIGIT: [0-7];

fragment HEXADECIMALDIGIT: [0-9a-fA-F];

fragment BINARYDIGIT: [01];

Integersuffix:
	Unsignedsuffix Longsuffix?
	| Unsignedsuffix Longlongsuffix?
	| Longsuffix Unsignedsuffix?
	| Longlongsuffix Unsignedsuffix?;

fragment Unsignedsuffix: [uU];

fragment Longsuffix: [lL];

fragment Longlongsuffix: 'll' | 'LL';

fragment Cchar:
	~ ['\\\r\n]
	| Escapesequence
	| Universalcharactername;

fragment Escapesequence:
	Simpleescapesequence
	| Octalescapesequence
	| Hexadecimalescapesequence;

fragment Simpleescapesequence:
	'\\\''
	| '\\"'
	| '\\?'
	| '\\\\'
	| '\\a'
	| '\\b'
	| '\\f'
	| '\\n'
	| '\\r'
	| '\\' ('\r' '\n'? | '\n')
	| '\\t'
	| '\\v';

fragment Octalescapesequence:
	'\\' OCTALDIGIT
	| '\\' OCTALDIGIT OCTALDIGIT
	| '\\' OCTALDIGIT OCTALDIGIT OCTALDIGIT;

fragment Hexadecimalescapesequence: '\\x' HEXADECIMALDIGIT+;

fragment Fractionalconstant:
	Digitsequence? '.' Digitsequence
	| Digitsequence '.';

fragment Exponentpart:
	'e' SIGN? Digitsequence
	| 'E' SIGN? Digitsequence;

fragment SIGN: [+-];

fragment Digitsequence: DIGIT ('\''? DIGIT)*;

fragment Floatingsuffix: [flFL];

fragment Encodingprefix: 'u8' | 'u' | 'U' | 'L';

fragment Schar:
	~ ["\\\r\n]
	| Escapesequence
	| Universalcharactername;

fragment Rawstring: 'R"' ( '\\' ["()] |~[\r\n (])*? '(' ~[)]*? ')'  ( '\\' ["()] | ~[\r\n "])*? '"';

UserDefinedIntegerLiteral:
	DecimalLiteral Udsuffix
	| OctalLiteral Udsuffix
	| HexadecimalLiteral Udsuffix
	| BinaryLiteral Udsuffix;

UserDefinedFloatingLiteral:
	Fractionalconstant Exponentpart? Udsuffix
	| Digitsequence Exponentpart Udsuffix;

UserDefinedStringLiteral: StringLiteral Udsuffix;

UserDefinedCharacterLiteral: CharacterLiteral Udsuffix;

fragment Udsuffix: Identifier;  이것은 antlr4의 렉서구문인데, 명칭그대로 javaCC의 토큰 구문으로 변환해줘.

## ChatGPT 응답:
> Sure! Here's the equivalent token syntax in JavaCC for the given lexer grammar:

{% raw %}```java
options {
  STATIC = false; // Set to true if the parser methods are static
}

PARSER_BEGIN(MyParser)
public class MyParser {
}
PARSER_END(MyParser)

TOKEN:
{
  <Alignas: "alignas">
| <Alignof: "alignof">
| <Asm: "asm">
| <Auto: "auto">
| <Bool: "bool">
| <Break: "break">
| <Case: "case">
| <Catch: "catch">
| <Char: "char">
| <Char16: "char16_t">
| <Char32: "char32_t">
| <Class: "class">
| <Const: "const">
| <Constexpr: "constexpr">
| <Const_cast: "const_cast">
| <Continue: "continue">
| <Decltype: "decltype">
| <Default: "default">
| <Delete: "delete">
| <Do: "do">
| <Double: "double">
| <Dynamic_cast: "dynamic_cast">
| <Else: "else">
| <Enum: "enum">
| <Explicit: "explicit">
| <Export: "export">
| <Extern: "extern">
| <False_: "false">
| <Final: "final">
| <Float: "float">
| <For: "for">
| <Friend: "friend">
| <Goto: "goto">
| <If: "if">
| <Inline: "inline">
| <Int: "int">
| <Long: "long">
| <Mutable: "mutable">
| <Namespace: "namespace">
| <New: "new">
| <Noexcept: "noexcept">
| <Nullptr: "nullptr">
| <Operator: "operator">
| <Override: "override">
| <Private: "private">
| <Protected: "protected">
| <Public: "public">
| <Register: "register">
| <Reinterpret_cast: "reinterpret_cast">
| <Return: "return">
| <Short: "short">
| <Signed: "signed">
| <Sizeof: "sizeof">
| <Static: "static">
| <Static_assert: "static_assert">
| <Static_cast: "static_cast">
| <Struct: "struct">
| <Switch: "switch">
| <Template: "template">
| <This: "this">
| <Thread_local: "thread_local">
| <Throw: "throw">
| <True_: "true">
| <Try: "try">
| <Typedef: "typedef">
| <Typeid_: "typeid">
| <Typename_: "typename">
| <Union: "union">
| <Unsigned: "unsigned">
| <Using: "using">
| <Virtual: "virtual">
| <Void: "void">
| <Volatile: "volatile">
| <Wchar: "wchar_t">
| <While: "while">
| <LeftParen: "(">
| <RightParen: ")">
| <LeftBracket: "[">
| <RightBracket: "]">
| <LeftBrace: "{">
| <RightBrace: "}">
| <Plus: "+">
| <Minus: "-">
| <Star: "*">
| <Div: "/">
| <Mod: "%">
| <Caret: "^">
| <And: "&">
| <Or: "|">
| <Tilde: "~">
| <Not: "!" | "not">
| <Assign: "=">
| <Less: "<">
| <Greater: ">">
| <PlusAssign: "+=">
| <MinusAssign: "-=">
| <StarAssign: "*=">
| <DivAssign: "/=">
| <ModAssign: "%=">
| <XorAssign: "^=">
| <AndAssign: "&=">
| <OrAssign: "|=">
| <LeftShiftAssign: "<<=">
| <RightShiftAssign: ">>=">
| <Equal: "==">
| <NotEqual: "!=">
| <LessEqual: "<=">
| <GreaterEqual: ">=">
| <AndAnd: "&&" | "and">
| <OrOr: "||" | "or">
| <PlusPlus: "++">
| <MinusMinus: "--">
| <Comma: ",">
| <ArrowStar: "->*">
| <Arrow: "->">
| <Question: "?">
| <Colon: ":">
| <Doublecolon: "::">
| <Semi: ";">
| <Dot: ".">
| <DotStar: ".*">
| <Ellipsis: "...">
| <BooleanLiteral: <False_> | <True_>>
| <PointerLiteral: "nullptr">
| <UserDefinedLiteral: UserDefinedIntegerLiteral | UserDefinedFloatingLiteral | UserDefinedStringLiteral | UserDefinedCharacterLiteral>
| <Identifier: Identifiernondigit ( Identifiernondigit | <DIGIT>)* >
| <DecimalLiteral: <NONZERODIGIT> ('\''? <DIGIT>)*>
| <OctalLiteral: '0' ('\''? <OCTALDIGIT>)*>
| <HexadecimalLiteral: ('0x' | '0X') <HEXADECIMALDIGIT> ('\''? <HEXADECIMALDIGIT>)*>
| <BinaryLiteral: ('0b' | '0B') <BINARYDIGIT> ('\''? <BINARYDIGIT>)*>
| <Integersuffix: Unsignedsuffix Longsuffix? | Unsignedsuffix Longlongsuffix? | Longsuffix Unsignedsuffix? | Longlongsuffix Unsignedsuffix?>
| <Unsignedsuffix: [uU]>
| <Longsuffix: [lL]>
| <Longlongsuffix: "ll" | "LL">
| <CharacterLiteral: ('u' | 'U' | 'L')? '\'' <Cchar>+ '\''>
| <FloatingLiteral: Fractionalconstant Exponentpart? Floatingsuffix? | Digitsequence Exponentpart Floatingsuffix?>
| <StringLiteral: Encodingprefix? ( Rawstring | '"' <Schar>* '"' ) >
| <Hexquad: <HEXADECIMALDIGIT> <HEXADECIMALDIGIT> <HEXADECIMALDIGIT> <HEXADECIMALDIGIT> >
| <Universalcharactername: '\\' ('u' <Hexquad> | 'U' <Hexquad> <Hexquad>)>
| <Udsuffix: <Identifier>>
| <UserDefinedIntegerLiteral: DecimalLiteral Udsuffix | OctalLiteral Udsuffix | HexadecimalLiteral Udsuffix | BinaryLiteral Udsuffix>
| <UserDefinedFloatingLiteral: Fractionalconstant Exponentpart? Udsuffix | Digitsequence Exponentpart Udsuffix>
| <UserDefinedStringLiteral: StringLiteral Udsuffix>
| <UserDefinedCharacterLiteral: CharacterLiteral Udsuffix>
}

// Skips white spaces and comments
SPECIAL_TOKEN:
{
  < WHITESPACE: [" ", "\t", "\r", "\n"] >
| < SINGLE_LINE_COMMENT: "//" (~["\n", "\r"])* >
| < MULTI_LINE_COMMENT: "/*" (~"*/")* "*/" >
}
```{% endraw %}

Please note that JavaCC uses the {% raw %}`SKIP`{% endraw %} option to handle whitespace and comments. The {% raw %}`SPECIAL_TOKEN

## ChatGPT 응답:
> `{% endraw %} section defines regular expressions to skip whitespace, single-line comments, and multi-line comments. This way, they are ignored during the parsing process.

