---
layout: post
title: "Java 출력 null 처리"
date: 2023-12-05
tags: [처리, Java, 출력, null]
categories: blog
---

## 사용자 입력:
> 자바에서 System.out.println("FILE_NM["+sFileNM+"]);을 찍는데, sFileNM이 null이면 어떻게 되나?

## ChatGPT 응답:
> Java에서 `System.out.println("FILE_NM[" + sFileNM + "]");` 구문을 사용할 때, `sFileNM` 변수가 `null`이라면 출력 결과는 다음과 같습니다:

```
FILE_NM[null]
```

Java에서 문자열 연결을 수행할 때, `null` 값이 문자열로 변환되면 `"null"` 문자열이 됩니다. 따라서 `sFileNM`이 `null`인 경우, 이 변수의 값 대신 문자열 `"null"`이 출력됩니다.

