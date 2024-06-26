---
layout: post
title: "C 언어와 namespace 관련"
tags: [언어, namespace, C, 관련]
---

## 사용자 입력:
> C언어에 namespace 키워드가 있나?

## ChatGPT 응답:
> C 언어에는 {% raw %}`namespace`{% endraw %} 키워드가 없습니다. {% raw %}`namespace`{% endraw %}는 C++에 도입된 기능으로, 이름 충돌을 방지하고 코드를 더 잘 구성할 수 있게 해줍니다. C 언어에서는 전역 변수와 함수 이름이 프로그램 전체에서 고유해야 하며, {% raw %}`namespace`{% endraw %}와 같은 기능을 사용하여 이름을 그룹화하거나 구분하는 것이 불가능합니다.

C에서는 일반적으로 함수 이름이나 전역 변수에 접두어를 붙여 서로 다른 라이브러리나 모듈 간의 이름 충돌을 방지하는 전략을 사용합니다. 예를 들어, 라이브러리 A와 B가 모두 {% raw %}`init`{% endraw %} 함수를 가지고 있다면, 이를 {% raw %}`a_init`{% endraw %}과 {% raw %}`b_init`{% endraw %}과 같이 구별할 수 있습니다.

