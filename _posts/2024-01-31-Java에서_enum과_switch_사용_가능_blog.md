---
layout: post
title: "Java에서 enum과 switch 사용 가능"
tags: [과, 사용, 가능, switch]
---

## 사용자 입력:
> 자바 swith문에서 case에 Enum을 못쓰나?

## ChatGPT 응답:
> Java의 {% raw %}`switch`{% endraw %} 문에서 {% raw %}`enum`{% endraw %}을 사용하는 것은 가능합니다. Java 5 이상 버전에서는 {% raw %}`switch`{% endraw %} 문에서 {% raw %}`enum`{% endraw %}을 사용할 수 있도록 지원합니다. 이를 통해 코드의 가독성을 높이고 오류를 줄일 수 있습니다.

다음은 Java에서 {% raw %}`enum`{% endraw %}을 {% raw %}`switch`{% endraw %} 문과 함께 사용하는 간단한 예시입니다:

{% raw %}```java
public class Test {
    public enum Day {
        MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
    }

    public static void main(String[] args) {
        Day day = Day.MONDAY;

        switch (day) {
            case MONDAY:
                System.out.println("It's Monday!");
                break;
            case TUESDAY:
                System.out.println("It's Tuesday!");
                break;
            // ... 나머지 요일에 대한 case들 ...
            default:
                System.out.println("It's some other day!");
                break;
        }
    }
}
```{% endraw %}

이 예시에서 {% raw %}`Day`{% endraw %}라는 {% raw %}`enum`{% endraw %}을 정의하고, {% raw %}`switch`{% endraw %} 문 내에서 이 {% raw %}`enum`{% endraw %}의 값들을 {% raw %}`case`{% endraw %}로 사용하고 있습니다. {% raw %}`enum`{% endraw %}을 사용하면 문자열이나 정수 값을 직접 사용하는 것보다 오류가 발생할 가능성이 줄어들며 코드를 더 명확하고 관리하기 쉽게 만들 수 있습니다.

