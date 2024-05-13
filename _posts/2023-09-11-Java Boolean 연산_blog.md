---
layout: post
title: "Java Boolean 연산"
date: 2023-09-11
tags: [연산, Java, Boolean]
categories: blog
---

## 사용자 입력:
> 자바에서 boolean 곱하기

## ChatGPT 응답:
> 자바에서 `boolean` 값끼리의 곱셈(AND 연산)은 논리 연산자 `&&`를 사용하여 수행합니다. `&&` 연산자는 두 개의 `boolean` 피연산자를 받아 두 값이 모두 `true`일 경우에만 결과가 `true`가 되고, 그 외의 경우에는 `false`가 됩니다.

예를 들어, 두 개의 `boolean` 변수 `a`와 `b`가 있고, 이 두 변수를 AND 연산하려면 다음과 같이 할 수 있습니다:

```java
boolean a = true;
boolean b = false;
boolean result = a && b; // result 변수에는 false가 저장됨
```

위의 코드에서 `result` 변수에는 `a`와 `b`를 AND 연산한 결과인 `false`가 저장됩니다.

AND 연산을 사용하여 여러 개의 `boolean` 값을 결합하려면 각 `boolean` 값을 순차적으로 `&&` 연산자로 연결하면 됩니다.

