---
layout: post
title: "자바8에서 원소 넣은 순서대로 출력되는 맵 - LinkedHashMap"
tags: [자바, LinkedHashMap, 순서대로, 출력, 원소, 맵]
---

## 사용자 입력:
> 자바8의 맵중 원소 넣은 순서대로 toString되는 자료형은?

## ChatGPT 응답:
> 자바 8에서 원소를 넣은 순서대로 {% raw %}`toString()`{% endraw %} 메서드로 출력되는 Map 자료형은 {% raw %}`LinkedHashMap`{% endraw %}입니다. {% raw %}`LinkedHashMap`{% endraw %}은 해시 테이블과 연결 리스트를 결합하여 구현되어 있어, 원소들이 삽입된 순서대로 순회됩니다. 이는 {% raw %}`HashMap`{% endraw %}과는 대조적이며, {% raw %}`HashMap`{% endraw %}은 원소들이 특정 순서 없이 저장되는 것이 일반적입니다. {% raw %}`LinkedHashMap`{% endraw %}은 원소의 삽입 순서를 유지할 뿐만 아니라, 접근 순서를 유지하는 모드도 제공합니다. 이를 통해 가장 최근에 접근한 원소가 맵의 끝으로 이동하게 할 수 있습니다.

