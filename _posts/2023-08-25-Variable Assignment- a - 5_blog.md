---
layout: post
title: "Variable Assignment: a = 5"
date: 2023-08-25
tags: [a, Assignment, Variable]
categories: blog
---

## 사용자 입력:
> a = 5;

## ChatGPT 응답:
> You've assigned the value `5` to the variable `a`. How can I assist you further with this?

## 사용자 입력:
> a=5;
b=2;
c=a+b;
c=a-b;
이것을 표현하는 간단한 antlr4 파서 만들어봐.

## ChatGPT 응답:
> ANTLR4는 파서 및 렉서를 생성하기 위한 도구입니다. 지정된 문법을 기반으로 소스 코드를 분석할 수 있는 파서와 렉서를 생성할 수 있습니다.

당신이 원하는 간단한 문법의 파서를 생성하기 위해 ANTLR4 문법을 작성해보겠습니다.

먼저, `SimpleMath.g4`라는 파일을 생성합니다. 

```antlr
grammar SimpleMath;

// 파서 규칙
prog:   (stat ';')+ ;  // 최소 하나 이상의 문장
stat:   assignment     // 변수 할당
    |   expression     // 수식
    ;

assignment:   ID '=' expression ;

expression:   expression '+' expression
    |   expression '-' expression
    |   ID
    |   INT
    ;

// 렉서 규칙
ID  :   [a-zA-Z]+ ;      // 변수명은 알파벳으로 이루어진다.
INT :   [0-9]+ ;        // 정수
WS  :   [ \t\r\n]+ -> skip ; // 공백은 무시
'=' :   '=' ;
'+' :   '+' ;
'-' :   '-' ;
';' :   ';' ;
```

이제 ANTLR4로 이 문법 파일을 사용하여 렉서 및 파서 코드를 생성할 수 있습니다.

터미널에서 다음 명령을 실행하세요:

```
antlr4 SimpleMath.g4 -Dlanguage=Python3
```

이렇게 하면 `SimpleMathLexer.py`, `SimpleMathParser.py` 등의 파이썬 파일들이 생성됩니다. 이 파일들을 사용하여 주어진 입력에 대한 파싱을 수행할 수 있습니다.

참고로, 위의 문법은 매우 기본적인 수준의 문법입니다. 실제로 사용하려면 좀 더 세밀한 조정과 규칙 확장이 필요할 수 있습니다.

