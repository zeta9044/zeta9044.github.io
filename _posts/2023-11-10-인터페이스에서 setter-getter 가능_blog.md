---
layout: post
title: "인터페이스에서 setter/getter 가능"
date: 2023-11-10
tags: [인터페이스, setter, getter, 가능]
categories: blog
---

## 사용자 입력:
> 자바 인터페이스에서 setter/getter.를 할 수 있나?

## ChatGPT 응답:
> 자바 인터페이스에서는 `setter`와 `getter` 메소드를 정의할 수 있습니다. 하지만, 인터페이스 자체는 상태(필드)를 가질 수 없기 때문에, 이 메소드들은 구체적인 구현을 포함하지 않습니다. 대신, 이 메소드들은 인터페이스를 구현하는 클래스에서 구현해야 합니다.

예를 들어:

```java
public interface MyInterface {
    void setMyValue(int value); // setter
    int getMyValue(); // getter
}

public class MyClass implements MyInterface {
    private int myValue;

    @Override
    public void setMyValue(int value) {
        this.myValue = value;
    }

    @Override
    public int getMyValue() {
        return this.myValue;
    }
}
```

여기서 `MyInterface`는 `setMyValue`와 `getMyValue` 메소드를 정의하지만, 실제 구현은 `MyClass`에서 이루어집니다. `MyClass`는 `MyInterface`를 구현하고, `myValue`라는 상태를 관리합니다.

