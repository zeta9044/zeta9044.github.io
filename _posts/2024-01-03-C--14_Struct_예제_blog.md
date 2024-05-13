---
layout: post
title: "C++14 Struct 예제"
date: 2024-01-03
tags: [C, 예제, Struct]
categories: blog
---

## 사용자 입력:
> c++ 14의 struct 예제

## ChatGPT 응답:
> C++14에서 `struct`를 사용하는 예제를 제공해 드리겠습니다. `struct`는 C++에서 데이터 구조를 정의하는 데 사용되는 키워드입니다. 여기서는 간단한 `Person` 구조체를 만들어 이름과 나이를 저장하는 예제를 들겠습니다.

```cpp
#include <iostream>
#include <string>

// Person 구조체 정의
struct Person {
    std::string name;
    int age;

    // Person의 정보를 출력하는 함수
    void printInfo() {
        std::cout << "Name: " << name << ", Age: " << age << std::endl;
    }
};

int main() {
    // Person 객체 생성 및 초기화
    Person person1;
    person1.name = "Alice";
    person1.age = 30;

    // Person 정보 출력
    person1.printInfo();

    return 0;
}
```

이 예제에서 `Person` 구조체는 `name`과 `age`라는 두 멤버 변수를 가지고 있습니다. `printInfo` 멤버 함수는 `Person`의 이름과 나이를 출력합니다. `main` 함수에서는 `Person` 타입의 객체 `person1`을 생성하고, 이름과 나이를 설정한 후 `printInfo` 함수를 호출하여 정보를 출력합니다.

