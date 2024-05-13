---
layout: post
title: "RESOURCE_LEAK 문제 해결"
date: 2023-11-27
tags: [해결, 문제]
categories: blog
---

## 사용자 입력:
> "170.     	
171.     	if ( sf.exists())
172.     	{
173.         	BufferedReader bfr = new BufferedReader(new FileReader(sf));
174.         	try 
175.         	{
176.         		String temp = bfr.readLine().trim();"
이것은 자바코드인데, 173라인에 취약점으로 RESOURCE_LEAK가 검출됐다. 해결방법

## ChatGPT 응답:
> 173번 라인에 취약점으로 RESOURCE_LEAK가 검출된 것은 BufferedReader와 FileReader를 생성하고 사용한 후에 리소스를 명시적으로 닫지 않았기 때문입니다. 이러한 리소스 누수를 방지하기 위해 try-with-resources 문을 사용하여 파일을 읽는 코드를 수정할 수 있습니다. 이렇게 수정하면 자동으로 리소스가 닫히므로 리소스 누수 문제를 방지할 수 있습니다.

다음은 수정된 코드 예제입니다:

```java
170.     	
171.     	if (sf.exists()) {
172.         	try (BufferedReader bfr = new BufferedReader(new FileReader(sf))) {
173.             	String temp = bfr.readLine().trim();
174.             	// 이후에 temp 변수를 사용할 수 있습니다.
175.         	}
176.     	}
```

이렇게 수정된 코드에서는 try-with-resources 문을 사용하여 BufferedReader를 생성하고 파일을 읽은 후 자동으로 리소스를 닫아줍니다. 따라서 리소스 누수 문제가 해결됩니다.

