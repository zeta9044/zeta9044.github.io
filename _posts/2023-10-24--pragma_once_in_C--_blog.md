---
layout: post
title: "#pragma once in C++"
tags: [in, C, once, pragma]
---

## 사용자 입력:
> C++에서 #pragma once 파일 처음에 이게 있는데, 문법에 맞나?

## ChatGPT 응답:
> 네, {% raw %}`#pragma once`{% endraw %}는 C++에서 흔히 사용되는 전처리 지시자입니다. 이 지시자는 주로 헤더 파일의 중복 포함을 방지하기 위해 사용됩니다.

전통적으로 헤더 파일의 중복 포함을 방지하기 위해 {% raw %}`#ifndef`{% endraw %}, {% raw %}`#define`{% endraw %}, {% raw %}`#endif`{% endraw %}를 사용하는 방법이 있습니다. 예를 들어, {% raw %}`header.h`{% endraw %}라는 헤더 파일이 있다면, 다음과 같은 가드를 사용하여 중복 포함을 방지할 수 있습니다:

{% raw %}```cpp
#ifndef HEADER_H
#define HEADER_H

// ... 헤더 파일의 내용 ...

#endif
```{% endraw %}

하지만 {% raw %}`#pragma once`{% endraw %}는 이러한 방식보다 더 간단하게 중복 포함을 방지할 수 있습니다. 해당 지시자를 헤더 파일의 시작 부분에 추가하기만 하면 됩니다:

{% raw %}```cpp
#pragma once

// ... 헤더 파일의 내용 ...
```{% endraw %}

{% raw %}`#pragma once`{% endraw %}는 대부분의 컴파일러에서 지원되지만, 표준 C++에는 포함되어 있지 않습니다. 그러나 실제로는 거의 모든 현대 컴파일러에서 지원되기 때문에 널리 사용됩니다.

다만, 크로스 플랫폼 개발을 할 때는 컴파일러마다의 동작 차이나 일부 예외적인 상황을 고려하여 전통적인 인클루드 가드를 사용하는 것도 고려해볼 만합니다.

