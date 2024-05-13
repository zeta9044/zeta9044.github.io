---
layout: post
title: "Java Uppercase String"
date: 2023-07-11
tags: [Uppercase, String, Java]
categories: blog
---

## 사용자 입력:
> java upper String?

## ChatGPT 응답:
> To convert a string to uppercase in Java, you can use the `toUpperCase()` method. Here's an example:

```java
String str = "Hello, World!";
String upperCaseStr = str.toUpperCase();
System.out.println(upperCaseStr);
```

Output:
```
HELLO, WORLD!
```

The `toUpperCase()` method returns a new string with all characters converted to uppercase. It does not modify the original string, so if you want to store the uppercase string, you need to assign it to a new variable.

Keep in mind that `toUpperCase()` uses the rules of the default locale to convert the string. If you want to perform the conversion using a specific locale, you can use the overloaded version of the method that takes a `Locale` parameter. For example:

```java
String str = "Hello, World!";
String upperCaseStr = str.toUpperCase(Locale.US);
System.out.println(upperCaseStr);
```

Output:
```
HELLO, WORLD!
```

In this example, the `toUpperCase()` method is called with the `Locale.US` parameter, which ensures that the conversion is done using the rules of the US locale.

