---
layout: post
title: "Adapter & Composite Pattern"
date: 2024-02-13
tags: [Composite, Adapter, Pattern]
categories: blog
---

## 사용자 입력:
> 자바에서 기존 두 개의 클래스 A, B 를 사용해서   C 클래스에서 A,B개의 메소드를 이용해서 새로운 기능을 만든다. 그런데, C클래스에서도 A,B 의 일부 기능을 써야 한다. 그리고, A,B클래스는 절대 수정하면 안된다. 이럴때 사용되는 패턴 예시

## ChatGPT 응답:
> 자바에서 기존의 두 클래스 A, B를 수정하지 않고, 이들의 기능을 활용하여 새로운 클래스 C에서 새로운 기능을 만들고 A, B의 기능 일부를 사용해야 할 때, 디자인 패턴 중 '파사드(Facade) 패턴', '컴포지트(Composite) 패턴', 또는 '어댑터(Adapter) 패턴'을 사용할 수 있습니다. 그러나 여기서는 각 클래스의 기능을 조합하고 확장하는 상황에 가장 적합한 '어댑터(Adapter) 패턴'과 '컴포지트(Composite) 패턴'에 초점을 맞추겠습니다.

### 어댑터(Adapter) 패턴

어댑터 패턴은 한 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환할 때 사용됩니다. 기존 클래스의 인터페이스를 변경하지 않으면서도 새로운 인터페이스를 통해 그 기능을 사용할 수 있도록 해줍니다. 이 경우, 클래스 A와 B의 기능을 클래스 C에서 사용하고자 할 때 A와 B 각각에 대한 어댑터를 만들어 C에서 사용할 수 있습니다.

예를 들어, 클래스 A에는 `doSomething` 메소드가 있고, 클래스 B에는 `performAction` 메소드가 있다고 가정합시다. 클래스 C에서 이 두 메소드를 사용하려면 각각에 대한 어댑터를 만들어서 C에서 호출할 수 있도록 합니다.

### 컴포지트(Composite) 패턴

컴포지트 패턴은 객체들을 트리 구조로 구성하여 개별 객체와 객체의 조합을 클라이언트가 동일하게 다룰 수 있도록 하는 디자인 패턴입니다. 이 패턴은 여러 객체를 그룹화하여 마치 하나의 객체처럼 다루고자 할 때 유용합니다. 클래스 A와 B의 기능을 조합하여 새로운 기능을 만들고자 할 때 사용할 수 있습니다.

예를 들어, 클래스 C가 클래스 A와 B의 기능을 조합하여 사용해야 한다면, A와 B를 컴포지트 패턴의 '리프(Leaf)'로 간주하고, C를 '컴포지트(Composite)' 객체로 구현하여 A와 B의 기능을 조합하고 확장할 수 있습니다.

### 예시 코드

```java
// 기존 클래스 A
class A {
    public void doSomething() {
        System.out.println("A does something");
    }
}

// 기존 클래스 B
class B {
    public void performAction() {
        System.out.println("B performs action");
    }
}

// 어댑터 패턴 사용 예시
class AAdapter {
    private A a;

    public AAdapter(A a) {
        this.a = a;
    }

    public void execute() {
        a.doSomething();
    }
}

class BAdapter {
    private B b;

    public BAdapter(B b) {
        this.b = b;
    }

    public void execute() {
        b.performAction();
    }
}

// 클래스 C에서 A, B의 기능 사용
class C {
    private AAdapter aAdapter;
    private BAdapter bAdapter;

    public C(A a, B b) {
        this.aAdapter = new AAdapter(a);
        this.bAdapter = new BAdapter(b);
    }

    public void doNewThing() {
        aAdapter.execute();
        bAdapter.execute();
        System.out.println("C does a new thing using A and B");
    }
}
```

위의 예시에서 `C` 클래스는 `A`와 `B`의 기능을 활용하여 새로운 기능을 수행합니다. `AAdapter`와 `BAdapter`는 각각 `A`와 `B`의 메소드를 `C`에서 사용할 수 있도록 인터페이스를 조정합니다. 이

