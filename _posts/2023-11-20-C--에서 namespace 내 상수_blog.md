---
layout: post
title: "C++에서 namespace 내 상수"
date: 2023-11-20
tags: [namespace, C, 상수, 내]
categories: blog
---

## 사용자 입력:
> C++에서 namespace를 정의할 때, 상수도 정의할 수 있나?

## ChatGPT 응답:
> 네, C++에서 `namespace` 내부에 상수를 정의할 수 있습니다. `namespace`는 서로 다른 라이브러리나 모듈의 이름 충돌을 방지하기 위해 사용되며, 함수, 변수, 상수 등을 포함할 수 있습니다. 상수를 정의하는 예시는 다음과 같습니다:

```cpp
namespace MyNamespace {
    const int MY_CONSTANT = 100;
}
```

이렇게 정의한 상수 `MY_CONSTANT`는 `MyNamespace::MY_CONSTANT` 형태로 사용할 수 있습니다. 이는 `namespace`가 해당 상수에 대한 유효한 범위를 제공하기 때문입니다.

