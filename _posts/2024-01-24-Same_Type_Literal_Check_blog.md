---
layout: post
title: "Same Type Literal Check"
date: 2024-01-24
tags: [Type, Check, Literal, Same]
categories: blog
---

## 사용자 입력:
>     public static boolean isValue(String in) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        boolean foundIntegerLiteral = false;
        boolean foundCharLiteral = false;
        boolean foundFloatingLiteral = false;
        boolean foundStringLiteral = false;

        // 모든 토큰을 순회하며 해당 리터럴 타입이 존재하는지 확인
        for (Token token : tokens.getTokens()) {
            switch (token.getType()) {
                case CPP14Lexer.IntegerLiteral:
                    foundIntegerLiteral = true;
                    break;
                case CPP14Lexer.CharacterLiteral:
                    foundCharLiteral = true;
                    break;
                case CPP14Lexer.FloatingLiteral:
                    foundFloatingLiteral = true;
                    break;
                case CPP14Lexer.StringLiteral:
                    foundStringLiteral = true;
                    break;
            }
        }

        // 리터럴 종류 결정
        return (foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) ||
                (!foundIntegerLiteral && foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) ||
                (!foundIntegerLiteral && !foundCharLiteral && foundFloatingLiteral && !foundStringLiteral) ||
                (!foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && foundStringLiteral);
    }  이 자바 함수는 token을 읽어서 종류별로 만나면 break; 되는데, 그러지 말고 입력된 문자열 in 이 같은 종류로만 되어 있을 때 참값으로 반환하는 함수로 바꿔봐.

## ChatGPT 응답:
> 아래는 입력된 문자열이 같은 종류의 리터럴로만 이루어져 있는 경우에 참값을 반환하는 함수입니다.

```java
public static boolean isSameLiteralType(String in) {
    CharStream input = CharStreams.fromString(in);
    CPP14Lexer lexer = new CPP14Lexer(input);
    CommonTokenStream tokens = new CommonTokenStream(lexer);
    tokens.fill();

    int foundTypes = 0;

    for (Token token : tokens.getTokens()) {
        int tokenType = token.getType();

        // 만나는 첫 번째 토큰의 타입 기록
        if (foundTypes == 0) {
            foundTypes = tokenType;
        }

        // 이전에 기록한 타입과 현재 타입이 다르면 종류가 섞여 있음
        if (foundTypes != tokenType) {
            return false;
        }
    }

    // 모든 토큰이 같은 타입일 경우에만 참값 반환
    return true;
}
```

이 함수는 입력된 문자열에서 모든 토큰이 같은 타입인지 확인하고, 같은 타입이면 참값을 반환합니다. 만약 토큰의 타입이 다른 것을 만나면 즉시 `false`를 반환하며 종료합니다.

## 사용자 입력:
> 야. 리터럴 케이스는 유지 해야지.

## ChatGPT 응답:
> 알겠습니다. 리터럴의 종류를 유지하면서 해당 리터럴 종류로만 이루어져 있는 경우에 참값을 반환하는 함수는 다음과 같이 수정될 수 있습니다.

```java
public static boolean isSameLiteralType(String in) {
    CharStream input = CharStreams.fromString(in);
    CPP14Lexer lexer = new CPP14Lexer(input);
    CommonTokenStream tokens = new CommonTokenStream(lexer);
    tokens.fill();

    boolean foundIntegerLiteral = false;
    boolean foundCharLiteral = false;
    boolean foundFloatingLiteral = false;
    boolean foundStringLiteral = false;

    for (Token token : tokens.getTokens()) {
        switch (token.getType()) {
            case CPP14Lexer.IntegerLiteral:
                foundIntegerLiteral = true;
                break;
            case CPP14Lexer.CharacterLiteral:
                foundCharLiteral = true;
                break;
            case CPP14Lexer.FloatingLiteral:
                foundFloatingLiteral = true;
                break;
            case CPP14Lexer.StringLiteral:
                foundStringLiteral = true;
                break;
        }
    }

    // 하나의 리터럴 종류만 발견된 경우에만 참값 반환
    return (foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) ||
            (!foundIntegerLiteral && foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) ||
            (!foundIntegerLiteral && !foundCharLiteral && foundFloatingLiteral && !foundStringLiteral) ||
            (!foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && foundStringLiteral);
}
```

이 함수는 입력된 문자열에서 발견된 리터럴 종류를 확인하고, 입력된 문자열이 하나의 리터럴 종류로만 이루어져 있는 경우에 참값을 반환합니다.

## 사용자 입력:
> 모든 토큰이 같은 종류일때만이라고.

## ChatGPT 응답:
> 알겠습니다. 이 경우에는 모든 토큰이 같은 종류일 때만 참값을 반환하도록 수정하겠습니다.

```java
public static boolean isSameLiteralType(String in) {
    CharStream input = CharStreams.fromString(in);
    CPP14Lexer lexer = new CPP14Lexer(input);
    CommonTokenStream tokens = new CommonTokenStream(lexer);
    tokens.fill();

    int firstTokenType = -1;  // 첫 번째 토큰의 타입을 저장할 변수

    for (Token token : tokens.getTokens()) {
        int tokenType = token.getType();

        // 만나는 첫 번째 토큰의 타입 기록
        if (firstTokenType == -1) {
            firstTokenType = tokenType;
        }

        // 이전에 기록한 타입과 현재 타입이 다르면 종류가 섞여 있음
        if (firstTokenType != tokenType) {
            return false;
        }
    }

    // 모든 토큰이 같은 타입일 경우에만 참값 반환
    return true;
}
```

이 함수는 입력된 문자열에서 모든 토큰이 같은 타입인 경우에만 참값을 반환하도록 수정되었습니다.

## 사용자 입력:
> 바보야. 같은 종류의 리터럴로만 이루어질 때 참값이라고.

## ChatGPT 응답:
> 알겠습니다. 이해가 더 잘못된 것 같아서 죄송합니다. 아래는 같은 종류의 리터럴로만 이루어져 있을 때 참값을 반환하는 함수입니다.

```java
public static boolean isSameLiteralType(String in) {
    CharStream input = CharStreams.fromString(in);
    CPP14Lexer lexer = new CPP14Lexer(input);
    CommonTokenStream tokens = new CommonTokenStream(lexer);
    tokens.fill();

    int firstLiteralType = -1;  // 첫 번째 리터럴의 타입을 저장할 변수

    for (Token token : tokens.getTokens()) {
        int tokenType = token.getType();

        // 리터럴 토큰인 경우
        if (isLiteralType(tokenType)) {
            // 만나는 첫 번째 리터럴의 타입 기록
            if (firstLiteralType == -1) {
                firstLiteralType = tokenType;
            }

            // 이전에 기록한 타입과 현재 타입이 다르면 종류가 섞여 있음
            if (firstLiteralType != tokenType) {
                return false;
            }
        }
    }

    // 모든 리터럴이 같은 타입일 경우에만 참값 반환
    return true;
}

// 특정 토큰이 리터럴 타입인지 확인하는 함수
private static boolean isLiteralType(int tokenType) {
    return tokenType == CPP14Lexer.IntegerLiteral ||
           tokenType == CPP14Lexer.CharacterLiteral ||
           tokenType == CPP14Lexer.FloatingLiteral ||
           tokenType == CPP14Lexer.StringLiteral;
}
```

이 함수는 입력된 문자열에서 모든 리터럴이 같은 타입인 경우에만 참값을 반환합니다.

## 사용자 입력:
> javadoc에 맞게 한글로 주석 달아.

## ChatGPT 응답:
> 물론이죠. 아래는 주석을 한글로 작성한 코드입니다.

```java
/**
 * 입력된 문자열이 같은 종류의 리터럴로만 이루어져 있는지 여부를 확인하는 함수.
 * @param in 입력 문자열
 * @return 같은 종류의 리터럴로만 이루어져 있으면 참값, 그렇지 않으면 거짓값
 */
public static boolean isSameLiteralType(String in) {
    CharStream input = CharStreams.fromString(in);
    CPP14Lexer lexer = new CPP14Lexer(input);
    CommonTokenStream tokens = new CommonTokenStream(lexer);
    tokens.fill();

    int firstLiteralType = -1;  // 첫 번째 리터럴의 타입을 저장할 변수

    for (Token token : tokens.getTokens()) {
        int tokenType = token.getType();

        // 리터럴 토큰인 경우
        if (isLiteralType(tokenType)) {
            // 만나는 첫 번째 리터럴의 타입 기록
            if (firstLiteralType == -1) {
                firstLiteralType = tokenType;
            }

            // 이전에 기록한 타입과 현재 타입이 다르면 종류가 섞여 있음
            if (firstLiteralType != tokenType) {
                return false;
            }
        }
    }

    // 모든 리터럴이 같은 타입일 경우에만 참값 반환
    return true;
}

/**
 * 특정 토큰이 리터럴 타입인지 확인하는 함수.
 * @param tokenType 확인할 토큰의 타입
 * @return 리터럴 타입이면 참값, 그렇지 않으면 거짓값
 */
private static boolean isLiteralType(int tokenType) {
    return tokenType == CPP14Lexer.IntegerLiteral ||
           tokenType == CPP14Lexer.CharacterLiteral ||
           tokenType == CPP14Lexer.FloatingLiteral ||
           tokenType == CPP14Lexer.StringLiteral;
}
```

이제 주석이 Javadoc 형식에 맞게 한글로 작성되었습니다.

