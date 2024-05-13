---
layout: post
title: "한글 Javadoc 주석"
date: 2024-01-19
tags: [Javadoc, 주석, 한글]
categories: blog
---

## 사용자 입력:
>     public static String getTokenName(int index) {
        return convertFirstCharToLowerCase(CPP14Lexer.ruleNames[index]);
    }

    public static String convertFirstCharToLowerCase(String input) {
        return input.isEmpty() ? input : input.substring(0, 1).toLowerCase() + input.substring(1);
    }  두개의 함수 javadoc 주석 한글로 만들어줘.

## ChatGPT 응답:
> ```java
/**
 * 지정된 인덱스에 대한 토큰 이름을 반환합니다.
 *
 * @param index 토큰의 인덱스
 * @return 지정된 인덱스에 대한 토큰 이름, 첫 문자가 소문자로 변환됩니다.
 */
public static String getTokenName(int index) {
    return convertFirstCharToLowerCase(CPP14Lexer.ruleNames[index]);
}

/**
 * 주어진 문자열의 첫 번째 문자를 소문자로 변환하여 반환합니다.
 *
 * @param input 대상 문자열
 * @return 첫 번째 문자가 소문자로 변환된 문자열, 입력이 비어 있는 경우 입력 그대로 반환됩니다.
 */
public static String convertFirstCharToLowerCase(String input) {
    return input.isEmpty() ? input : input.substring(0, 1).toLowerCase() + input.substring(1);
}
```

