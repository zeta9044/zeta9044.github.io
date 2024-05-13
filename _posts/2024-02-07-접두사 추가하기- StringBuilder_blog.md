---
layout: post
title: "접두사 추가하기: StringBuilder"
date: 2024-02-07
tags: [추가, 접두사, StringBuilder]
categories: blog
---

## 사용자 입력:
> 자바 StringBuilder 변수 앞에 prefix를 붙일려면?

## ChatGPT 응답:
> 자바의 `StringBuilder` 클래스를 사용할 때 변수(즉, 문자열) 앞에 접두사(prefix)를 추가하고 싶다면, `insert` 메소드를 사용할 수 있습니다. `insert` 메소드를 사용하면 `StringBuilder` 객체의 특정 위치에 문자열을 삽입할 수 있으며, 접두사를 추가하기 위해서는 문자열의 시작 위치인 0을 인덱스로 사용하면 됩니다.

다음은 `StringBuilder` 객체에 접두사를 추가하는 방법을 보여주는 예시 코드입니다:

```java
StringBuilder sb = new StringBuilder("World");
String prefix = "Hello ";

// 접두사를 StringBuilder 객체의 시작 부분에 추가합니다.
sb.insert(0, prefix);

// 결과 출력
System.out.println(sb.toString()); // "Hello World"
```

위의 예시에서는 `StringBuilder` 객체 `sb`가 초기에 "World"라는 문자열을 가지고 있습니다. `insert` 메소드를 사용하여 0번 인덱스, 즉 문자열의 맨 앞에 "Hello "라는 접두사를 추가합니다. 이후 `sb.toString()`을 호출하면 "Hello World"라는 결과를 얻을 수 있습니다.

