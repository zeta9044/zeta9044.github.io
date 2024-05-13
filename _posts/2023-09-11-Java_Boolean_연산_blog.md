---
layout: post
title: "Java Boolean 연산"
tags: [Boolean, 연산, Java]
---

## 사용자 입력:
> 자바에서 boolean 곱하기

## ChatGPT 응답:
> 자바에서 {% raw %}`boolean`{% endraw %} 값끼리의 곱셈(AND 연산)은 논리 연산자 {% raw %}`&&`{% endraw %}를 사용하여 수행합니다. {% raw %}`&&`{% endraw %} 연산자는 두 개의 {% raw %}`boolean`{% endraw %} 피연산자를 받아 두 값이 모두 {% raw %}`true`{% endraw %}일 경우에만 결과가 {% raw %}`true`{% endraw %}가 되고, 그 외의 경우에는 {% raw %}`false`{% endraw %}가 됩니다.

예를 들어, 두 개의 {% raw %}`boolean`{% endraw %} 변수 {% raw %}`a`{% endraw %}와 {% raw %}`b`{% endraw %}가 있고, 이 두 변수를 AND 연산하려면 다음과 같이 할 수 있습니다:

{% raw %}```java
boolean a = true;
boolean b = false;
boolean result = a && b; // result 변수에는 false가 저장됨
```{% endraw %}

위의 코드에서 {% raw %}`result`{% endraw %} 변수에는 {% raw %}`a`{% endraw %}와 {% raw %}`b`{% endraw %}를 AND 연산한 결과인 {% raw %}`false`{% endraw %}가 저장됩니다.

AND 연산을 사용하여 여러 개의 {% raw %}`boolean`{% endraw %} 값을 결합하려면 각 {% raw %}`boolean`{% endraw %} 값을 순차적으로 {% raw %}`&&`{% endraw %} 연산자로 연결하면 됩니다.

