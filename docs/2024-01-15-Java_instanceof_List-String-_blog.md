---
layout: post
title: "Java instanceof List<String>"
date: 2024-01-15
tags: [String, List, Java, instanceof]
categories: blog
---

## 사용자 입력:
> 자바에서   List<String>  x 라는 변수가 있는데, instanceof를 쓸려면?

## ChatGPT 응답:
> 자바에서 `instanceof` 연산자를 사용하여 객체가 특정 클래스나 인터페이스의 인스턴스인지 확인할 수 있습니다. `List`는 인터페이스이므로, `instanceof`를 사용하여 `x`가 `List<String>`의 인스턴스인지 확인할 수 있습니다.

```java
if (x instanceof List) {
    // x는 List의 인스턴스입니다.
    if (x instanceof List<?>) {
        // x는 매개변수화된 List의 인스턴스입니다.
        if (x instanceof List<String>) {
            // x는 List<String>의 인스턴스입니다.
        }
    }
}
```

위 코드에서 `x`가 `List` 인스턴스인지 먼저 확인하고, 그 후에 `List<?>`와 `List<String>`의 인스턴스인지를 순서대로 확인하고 있습니다. 이렇게 하면 `x`가 어떤 종류의 `List` 인스턴스인지 구체적으로 확인할 수 있습니다.

## 사용자 입력:
> 위에 표시된 x를 o로 바꿔줘.

## ChatGPT 응답:
> 자바 코드에서 변수명을 변경하는 것은 단순히 해당 변수의 이름을 변경하는 것으로 이루어집니다. `x`를 `o`로 변경하면 다음과 같습니다.

```java
if (o instanceof List) {
    // o는 List의 인스턴스입니다.
    if (o instanceof List<?>) {
        // o는 매개변수화된 List의 인스턴스입니다.
        if (o instanceof List<String>) {
            // o는 List<String>의 인스턴스입니다.
        }
    }
}
```

위 코드에서 `x`가 `o`로 변경되었습니다.

