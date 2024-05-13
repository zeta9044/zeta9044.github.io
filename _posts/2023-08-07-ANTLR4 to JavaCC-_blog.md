---
layout: post
title: "ANTLR4 to JavaCC."
date: 2023-08-07
tags: [to, JavaCC]
categories: blog
---

## 사용자 입력:
> balancedTokenSeq: balancedtoken+;

balancedtoken:
	LeftParen balancedTokenSeq RightParen
	| LeftBracket balancedTokenSeq RightBracket
	| LeftBrace balancedTokenSeq RightBrace
	| ~(
		LeftParen
		| RightParen
		| LeftBrace
		| RightBrace
		| LeftBracket
		| RightBracket
	)+; 이것은  cpp파서를 정의한 antlr4의 일부구문인데, 이것을  javaCC로 변환해줘. 명칭은 그대로 유지해줘.

## ChatGPT 응답:
> 아래는 주어진 Antlr4 문법을 JavaCC로 변환한 코드입니다. 명칭은 그대로 유지되었습니다.

```java
PARSER_BEGIN(balancedTokenParser)

import java.io.*;
import java.util.*;

public class balancedTokenParser {
    public static void main(String[] args) throws ParseException {
        try {
            balancedTokenParser parser = new balancedTokenParser(new java.io.FileInputStream(args[0]));
            parser.balancedTokenSeq();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

PARSER_END(balancedTokenParser)

TOKEN :
{
  <LeftParen: "(">
| <RightParen: ")">
| <LeftBracket: "[">
| <RightBracket: "]">
| <LeftBrace: "{">
| <RightBrace: "}">
}

void balancedTokenSeq() :
{}
{
  balancedtoken()+
}

void balancedtoken() :
{}
{
  <LeftParen> balancedTokenSeq() <RightParen>
| <LeftBracket> balancedTokenSeq() <RightBracket>
| <LeftBrace> balancedTokenSeq() <RightBrace>
| ~(<LeftParen> | <RightParen> | <LeftBrace> | <RightBrace> | <LeftBracket> | <RightBracket>)
}
```

위의 코드는 `balancedTokenParser`라는 클래스를 생성하며, 입력으로 파일을 받아 파싱하여 `balancedTokenSeq`를 찾아내는 기능을 제공합니다. 주어진 Antlr4 문법과 동일한 동작을 수행하도록 작성되었습니다.

JavaCC를 사용할 때에는 위의 코드를 `balancedTokenParser.jj`와 같은 확장자로 저장하고, JavaCC 툴을 이용하여 파서 클래스를 생성해야 합니다. 자세한 사용법은 JavaCC 문서를 참고하시기 바랍니다.

## 사용자 입력:
> | ~(<LeftParen> | <RightParen> | <LeftBrace> | <RightBrace> | <LeftBracket> | <RightBracket>) 이 부분이 오류라고 나오네.

## ChatGPT 응답:
> 안타깝게도 JavaCC에서는 Antlr4와 달리 토큰의 이름을 부등호 기호(`<`와 `>`)로 묶지 않습니다. 그래서 `<`와 `>`를 제거해야 합니다. 또한, JavaCC에서는 `+` 기호를 사용하여 반복을 나타내지 않고, 대신 메소드 내에서 반복 로직을 직접 구현해야 합니다.

따라서 `| ~(<LeftParen> | <RightParen> | <LeftBrace> | <RightBrace> | <LeftBracket> | <RightBracket>)` 부분을 수정해야 합니다. 아래는 수정된 JavaCC 코드입니다:

```java
PARSER_BEGIN(balancedTokenParser)

import java.io.*;
import java.util.*;

public class balancedTokenParser {
    public static void main(String[] args) throws ParseException {
        try {
            balancedTokenParser parser = new balancedTokenParser(new java.io.FileInputStream(args[0]));
            parser.balancedTokenSeq();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

PARSER_END(balancedTokenParser)

TOKEN :
{
  <LeftParen: "(">
| <RightParen: ")">
| <LeftBracket: "[">
| <RightBracket: "]">
| <LeftBrace: "{">
| <RightBrace: "}">
}

void balancedTokenSeq() :
{}
{
  balancedtoken() ( balancedtoken() )*
}

void balancedtoken() :
{}
{
  <LeftParen> balancedTokenSeq() <RightParen>
| <LeftBracket> balancedTokenSeq() <RightBracket>
| <LeftBrace> balancedTokenSeq() <RightBrace>
| ~[(){}[\]]
}
```

위 코드에서 `balancedTokenSeq()` 메소드 내에서 `balancedtoken()`을 반복 호출하도록 하였고, `balancedtoken()` 메소드 내에서 `~[(){}[\]]`를 사용하여 해당 문자들을 제외한 모든 문자를 나타냈습니다.

이제 수정된 코드로 JavaCC를 실행하면 올바르게 동작할 것입니다.

## 사용자 입력:
> ~(
		LeftParen
		| RightParen
		| LeftBrace
		| RightBrace
		| LeftBracket
		| RightBracket
	) 을 자바CC토큰으로 표현해줘.

## ChatGPT 응답:
> JavaCC에서 토큰을 표현할 때는 Regular Expression(정규 표현식)을 사용합니다. 따라서 주어진 표현식 `~( LeftParen | RightParen | LeftBrace | RightBrace | LeftBracket | RightBracket )`을 JavaCC 토큰으로 표현하려면 다음과 같이 하면 됩니다:

```java
TOKEN :
{
  <LeftParen: "(">
| <RightParen: ")">
| <LeftBracket: "[">
| <RightBracket: "]">
| <LeftBrace: "{">
| <RightBrace: "}">
| <NotParenBraceBracket: ~[(){}[\]]>
}
```

위 코드에서 `<NotParenBraceBracket: ~[(){}[\]]>`라는 토큰 정의를 추가하였습니다. 이제 `NotParenBraceBracket` 토큰은 괄호와 중괄호, 대괄호가 아닌 모든 문자를 나타냅니다.

이렇게 추가한 토큰 정의를 사용하여 `balancedtoken()` 메소드 내의 표현식을 다음과 같이 수정합니다:

```java
void balancedtoken() :
{}
{
  <LeftParen> balancedTokenSeq() <RightParen>
| <LeftBracket> balancedTokenSeq() <RightBracket>
| <LeftBrace> balancedTokenSeq() <RightBrace>
| <NotParenBraceBracket>
}
```

이제 JavaCC에서 이 코드를 실행하면 주어진 표현식을 올바르게 인식할 수 있을 것입니다.

