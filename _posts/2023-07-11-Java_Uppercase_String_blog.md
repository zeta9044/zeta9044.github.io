---
layout: post
title: "Java Uppercase String"
tags: [Uppercase, String, Java]
---

## 사용자 입력:
> java upper String?

## ChatGPT 응답:
> To convert a string to uppercase in Java, you can use the {% raw %}`toUpperCase()`{% endraw %} method. Here's an example:

{% raw %}```java
String str = "Hello, World!";
String upperCaseStr = str.toUpperCase();
System.out.println(upperCaseStr);
```{% endraw %}

Output:
{% raw %}```
HELLO, WORLD!
```{% endraw %}

The {% raw %}`toUpperCase()`{% endraw %} method returns a new string with all characters converted to uppercase. It does not modify the original string, so if you want to store the uppercase string, you need to assign it to a new variable.

Keep in mind that {% raw %}`toUpperCase()`{% endraw %} uses the rules of the default locale to convert the string. If you want to perform the conversion using a specific locale, you can use the overloaded version of the method that takes a {% raw %}`Locale`{% endraw %} parameter. For example:

{% raw %}```java
String str = "Hello, World!";
String upperCaseStr = str.toUpperCase(Locale.US);
System.out.println(upperCaseStr);
```{% endraw %}

Output:
{% raw %}```
HELLO, WORLD!
```{% endraw %}

In this example, the {% raw %}`toUpperCase()`{% endraw %} method is called with the {% raw %}`Locale.US`{% endraw %} parameter, which ensures that the conversion is done using the rules of the US locale.

