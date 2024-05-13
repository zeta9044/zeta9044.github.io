---
layout: post
title: "Java - Convert First Letter"
tags: [Convert, Letter, First, Java]
---

## 사용자 입력:
> 자바에서 문자열 첫자를 소문자로 변경하는 함수

## ChatGPT 응답:
> 자바에서 문자열의 첫 글자를 소문자로 변경하려면 다음과 같이 하면 됩니다. 

{% raw %}```java
public class Main {
    public static void main(String[] args) {
        String originalString = "HelloWorld";
        String modifiedString = convertFirstCharToLowerCase(originalString);
        System.out.println(modifiedString);
    }

    public static String convertFirstCharToLowerCase(String input) {
        if (input == null || input.isEmpty()) {
            return input;  // 빈 문자열이나 null일 경우 변환 없이 그대로 반환
        }
        char[] charArray = input.toCharArray();
        charArray[0] = Character.toLowerCase(charArray[0]);
        return new String(charArray);
    }
}
```{% endraw %}

위의 예제에서 {% raw %}`convertFirstCharToLowerCase`{% endraw %} 함수는 주어진 문자열의 첫 번째 문자를 소문자로 변경합니다. 입력 문자열이 {% raw %}`null`{% endraw %}이거나 빈 문자열인 경우에는 그대로 반환합니다. 이 함수를 호출하여 문자열의 첫 글자를 소문자로 변경할 수 있습니다.

## 사용자 입력:
> 위 내용보다 더 간단하게

## ChatGPT 응답:
> Java 8 이상에서는 {% raw %}`substring`{% endraw %} 메서드와 {% raw %}`toLowerCase`{% endraw %} 메서드를 사용하여 더 간단하게 구현할 수 있습니다. 예를 들어:

{% raw %}```java
public class Main {
    public static void main(String[] args) {
        String originalString = "HelloWorld";
        String modifiedString = convertFirstCharToLowerCase(originalString);
        System.out.println(modifiedString);
    }

    public static String convertFirstCharToLowerCase(String input) {
        return input.isEmpty() ? input : input.substring(0, 1).toLowerCase() + input.substring(1);
    }
}
```{% endraw %}

이 예제에서는 {% raw %}`convertFirstCharToLowerCase`{% endraw %} 함수가 입력 문자열이 비어있지 않은 경우 첫 번째 문자를 소문자로 변환하고 나머지 부분은 그대로 가져와서 새로운 문자열을 생성합니다. 이렇게 하면 코드가 더 간결해집니다.

