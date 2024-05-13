---
layout: post
title: "C++에서 struct 상속"
date: 2024-02-05
tags: [C, struct, 상속]
categories: blog
---

## 사용자 입력:
> c++에서 struct도 상속 있나?

## ChatGPT 응답:
> 네, C++에서는 구조체(`struct`)도 상속을 사용할 수 있습니다. C++에서 `struct`와 `class`의 주요 차이점은 기본 접근 제어자입니다. `struct`는 기본적으로 `public`이고, `class`는 기본적으로 `private`입니다. 그러나 `struct`와 `class`는 모두 상속을 지원하며, 상속의 구문과 동작 방식은 두 경우 모두 동일합니다.

예를 들어, 아래 코드는 `struct`를 사용한 상속 예시를 보여줍니다:

```cpp
struct Base {
    int baseData;
};

struct Derived : public Base {
    int derivedData;
};

int main() {
    Derived d;
    d.baseData = 1; // Base의 멤버에 접근
    d.derivedData = 2; // Derived의 멤버에 접근
    return 0;
}
```

이 예제에서 `Derived` 구조체는 `Base` 구조체로부터 상속받고 있으며, `Base`의 멤버 변수인 `baseData`에 접근할 수 있습니다. 상속의 종류(공개, 보호, 비공개)에 따라 상속받은 멤버의 접근성이 달라집니다. C++에서 `struct`를 사용하여 클래스처럼 상속 구조를 만들 수 있는 것은 언어의 유연성을 보여줍니다.

## 사용자 입력:
> 다중상속도?

## ChatGPT 응답:
> 네, C++에서는 `struct`를 포함하여 다중 상속도 지원합니다. 다중 상속은 한 클래스나 구조체가 둘 이상의 클래스나 구조체로부터 상속을 받는 것을 의미합니다. 이를 통해 여러 기반 클래스의 속성과 메서드를 하나의 파생 클래스에서 결합할 수 있습니다. 하지만 다중 상속은 다이아몬드 문제와 같은 복잡성을 초래할 수 있기 때문에 신중하게 사용해야 합니다.

다음은 `struct`를 사용한 다중 상속의 예시입니다:

```cpp
struct Base1 {
    int base1Data;
    void showBase1() { /* 구현 */ }
};

struct Base2 {
    int base2Data;
    void showBase2() { /* 구현 */ }
};

struct Derived : public Base1, public Base2 {
    int derivedData;
    void showDerived() {
        showBase1(); // Base1로부터 상속받은 메서드
        showBase2(); // Base2로부터 상속받은 메서드
        // 여기에 추가 구현
    }
};

int main() {
    Derived d;
    d.base1Data = 1; // Base1의 멤버에 접근
    d.base2Data = 2; // Base2의 멤버에 접근
    d.derivedData = 3; // Derived의 멤버에 접근
    d.showDerived(); // Derived의 메서드 호출
    return 0;
}
```

이 예제에서 `Derived` 구조체는 `Base1`과 `Base2` 두 구조체로부터 다중 상속을 받습니다. 이렇게 하여 `Derived`는 두 기반 구조체의 속성과 메서드를 모두 상속받아 사용할 수 있습니다. 다만, 다중 상속에서 이름 충돌이나 다이아몬드 상속 문제 같은 복잡한 상황을 주의해야 합니다. 다이아몬드 문제는 한 클래스가 두 경로를 통해 같은 기반 클래스로부터 상속을 받을 때 발생하며, 이를 해결하기 위해 C++에서는 가상 상속(virtual inheritance)을 제공합니다.

