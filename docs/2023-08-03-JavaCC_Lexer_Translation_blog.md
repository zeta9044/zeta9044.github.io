---
layout: post
title: "JavaCC Lexer Translation"
date: 2023-08-03
tags: [JavaCC, Translation, Lexer]
categories: blog
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

fragment Udsuffix: Identifier;  이것은 antlr4의 렉서구문인데, 명칭 그대로 javaCC의  토큰 구문으로 바꿔줘.

## ChatGPT 응답:
> JavaCC does not support EBNF notation directly as ANTLR does. In particular, it does not support the `*` (zero or more), `+` (one or more), `?` (zero or one) operators, and it doesn't have the concept of "fragments" that ANTLR uses. However, you can achieve similar results by using optional blocks (`[]`), repetition blocks (`() *` or `() +`), and choices (`|`).

Here is an example how you can translate the lexer grammar to JavaCC. Please note that this example is not a complete translation of your ANTLR grammar, but it should give you a basic understanding of how to proceed with the translation:

```
TOKEN :
{
  < DECIMAL_LITERAL: (NON_ZERO_DIGIT (["'"]? DIGIT)*) | ("0" (["'"]? OCTAL_DIGIT)*) | (("0x" | "0X") HEX_DIGIT (["'"]? HEX_DIGIT)*) | (("0b" | "0B") BINARY_DIGIT (["'"]? BINARY_DIGIT)*) >
| < NON_ZERO_DIGIT: ["1"-"9"] >
| < DIGIT: ["0"-"9"] >
| < OCTAL_DIGIT: ["0"-"7"] >
| < HEX_DIGIT: ["0"-"9", "a"-"f", "A"-"F"] >
| < BINARY_DIGIT: ["0"-"1"] >
| < UNSIGNED_SUFFIX: ["u", "U"] >
| < LONG_SUFFIX: ["l", "L"] >
| < LONG_LONG_SUFFIX: "ll" | "LL" >
}

// A simplified example of a token with a choice
TOKEN :
{
  < CHOICE_TOKEN: "token1" | "token2" | "token3" >
}

// Optional blocks
TOKEN :
{
  < OPTIONAL_TOKEN: ["a"] >
}

// Repetition blocks
TOKEN :
{
  < REPETITION_TOKEN: ("a")* >
| < REPETITION_TOKEN_PLUS: ("a")+ >
}

// String concatenation (sequence of characters)
TOKEN :
{
  < CONCAT_TOKEN: "a" "b" "c" >
}
```

Please replace "TOKEN" with your actual token names. Each rule should have a unique name.

I would highly recommend to read the JavaCC documentation in order to familiarize yourself with its syntax. JavaCC has its own ways to define grammars and some features may not map 1:1 from ANTLR to JavaCC.

