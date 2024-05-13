---
layout: post
title: "Java Constructor Inheritance"
date: 2023-08-22
tags: [Constructor, Java, Inheritance]
categories: blog
---

## 사용자 입력:
> java에서  B클래스를 상속받는  A 클래스가 있는데,  A클래스의  constuctor를 만든 예제 알려줘.

## ChatGPT 응답:
> 물론입니다! Java에서는 하위 클래스가 상위 클래스의 생성자를 호출할 수 있습니다. 아래 예제에서는 B 클래스가 A 클래스를 상속하고, A 클래스의 생성자를 호출하는 예제를 보여드리겠습니다.

```java
class A {
    private int valueA;

    // A 클래스의 생성자
    public A(int valueA) {
        this.valueA = valueA;
        System.out.println("A 클래스의 생성자 호출: " + valueA);
    }
}

class B extends A {
    private int valueB;

    // B 클래스의 생성자
    public B(int valueA, int valueB) {
        super(valueA);  // A 클래스의 생성자 호출
        this.valueB = valueB;
        System.out.println("B 클래스의 생성자 호출: " + valueB);
    }
}

public class Main {
    public static void main(String[] args) {
        B bObject = new B(10, 20); // B 클래스의 생성자 호출
    }
}
```

위의 코드에서 `A` 클래스는 `valueA`라는 인스턴스 변수와 파라미터를 받는 생성자를 가지고 있습니다. `B` 클래스는 `A` 클래스를 상속받으며, `valueB`라는 인스턴스 변수와 파라미터를 받는 생성자를 가지고 있습니다. `B` 클래스의 생성자에서는 `super(valueA)`를 호출하여 `A` 클래스의 생성자를 호출하고, 그 후에 `B` 클래스의 생성자 로직을 수행합니다.

이 코드를 실행하면 다음과 같은 결과가 나타날 것입니다:

```
A 클래스의 생성자 호출: 10
B 클래스의 생성자 호출: 20
```

이와 같이 상속 관계에서 상위 클래스의 생성자를 호출하여 초기화할 수 있습니다.

