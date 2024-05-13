---
layout: post
title: "int 매개변수에 null"
tags: [null, 매개변수, int]
---

## 사용자 입력:
> 자바에서 함수의 매개변수가 int형이 있는데, 여기에 null로 인자를 넣을 수 있나? a(int i) 인데, a(null)이렇게.

## ChatGPT 응답:
> 자바에서 기본 자료형인 {% raw %}`int`{% endraw %}에는 {% raw %}`null`{% endraw %} 값을 할당할 수 없습니다. {% raw %}`null`{% endraw %}은 참조형 변수에만 사용될 수 있으며, {% raw %}`int`{% endraw %}와 같은 기본 자료형에는 적용되지 않습니다. 만약 {% raw %}`null`{% endraw %}을 사용하고 싶다면, 해당 기본 자료형의 래퍼 클래스를 사용해야 합니다. 예를 들어, {% raw %}`int`{% endraw %}의 래퍼 클래스인 {% raw %}`Integer`{% endraw %}를 사용하면 {% raw %}`null`{% endraw %} 값을 할당할 수 있습니다.

예시:
{% raw %}```java
public void a(Integer i) {
    // ...
}

// 이제 a(null)을 호출할 수 있음
a(null);
```{% endraw %}

이 경우 {% raw %}`a`{% endraw %} 함수는 {% raw %}`Integer`{% endraw %} 객체를 매개변수로 받으며, {% raw %}`null`{% endraw %}을 인자로 전달할 수 있습니다. 하지만 이렇게 하면 {% raw %}`null`{% endraw %} 값에 대한 처리를 주의해야 합니다. {% raw %}`null`{% endraw %}을 사용하면 {% raw %}`NullPointerException`{% endraw %}이 발생할 위험이 있기 때문에, 코드에서 {% raw %}`null`{% endraw %} 체크를 해주는 것이 좋습니다.

