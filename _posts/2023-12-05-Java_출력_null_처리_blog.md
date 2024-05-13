---
layout: post
title: "Java 출력 null 처리"
tags: [Java, 출력, null, 처리]
---

## 사용자 입력:
> 자바에서 System.out.println("FILE_NM["+sFileNM+"]);을 찍는데, sFileNM이 null이면 어떻게 되나?

## ChatGPT 응답:
> Java에서 {% raw %}`System.out.println("FILE_NM[" + sFileNM + "]");`{% endraw %} 구문을 사용할 때, {% raw %}`sFileNM`{% endraw %} 변수가 {% raw %}`null`{% endraw %}이라면 출력 결과는 다음과 같습니다:

{% raw %}```
FILE_NM[null]
```{% endraw %}

Java에서 문자열 연결을 수행할 때, {% raw %}`null`{% endraw %} 값이 문자열로 변환되면 {% raw %}`"null"`{% endraw %} 문자열이 됩니다. 따라서 {% raw %}`sFileNM`{% endraw %}이 {% raw %}`null`{% endraw %}인 경우, 이 변수의 값 대신 문자열 {% raw %}`"null"`{% endraw %}이 출력됩니다.

