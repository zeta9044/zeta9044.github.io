---
layout: post
title: "C++14 Struct 예제"
tags: [C, 예제, Struct]
---

## 사용자 입력:
> c++ 14의 struct 예제

## ChatGPT 응답:
> C++14에서 {% raw %}`struct`{% endraw %}를 사용하는 예제를 제공해 드리겠습니다. {% raw %}`struct`{% endraw %}는 C++에서 데이터 구조를 정의하는 데 사용되는 키워드입니다. 여기서는 간단한 {% raw %}`Person`{% endraw %} 구조체를 만들어 이름과 나이를 저장하는 예제를 들겠습니다.

{% raw %}```cpp
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
```{% endraw %}

이 예제에서 {% raw %}`Person`{% endraw %} 구조체는 {% raw %}`name`{% endraw %}과 {% raw %}`age`{% endraw %}라는 두 멤버 변수를 가지고 있습니다. {% raw %}`printInfo`{% endraw %} 멤버 함수는 {% raw %}`Person`{% endraw %}의 이름과 나이를 출력합니다. {% raw %}`main`{% endraw %} 함수에서는 {% raw %}`Person`{% endraw %} 타입의 객체 {% raw %}`person1`{% endraw %}을 생성하고, 이름과 나이를 설정한 후 {% raw %}`printInfo`{% endraw %} 함수를 호출하여 정보를 출력합니다.

