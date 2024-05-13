---
layout: post
title: "Custom 오류 처리 인터페이스"
date: 2024-02-14
tags: [처리, Custom, 인터페이스, 오류]
categories: blog
---

## 사용자 입력:
> package org.antlr.symtab;

public interface ErrorListener {
    void onError(String message, Exception e);
}
 주석 달아줘.

## ChatGPT 응답:
> 이 Java 인터페이스 `ErrorListener`는 ANTLR 심볼 테이블 라이브러리의 일부입니다. ANTLR(Another Tool for Language Recognition)은 파서 생성기로, 언어 처리를 위한 강력한 도구를 제공합니다. 이 인터페이스는 심볼 테이블 작업 중 발생하는 오류를 처리하는 데 사용됩니다. `ErrorListener` 인터페이스는 오류 처리를 위한 하나의 메소드 `onError`를 정의합니다. 이 메소드는 오류가 발생할 때 호출되며, 오류 메시지와 발생한 예외에 대한 정보를 매개변수로 받습니다. 사용자는 이 인터페이스를 구현하여 커스텀 오류 처리 로직을 정의할 수 있습니다.

```java
package org.antlr.symtab;

/**
 * ErrorListener 인터페이스는 ANTLR 심볼 테이블 라이브러리에서
 * 오류를 듣기 위한 목적으로 정의됩니다.
 * 이를 통해 사용자는 심볼 테이블 작업 중 발생하는 오류를
 * 사용자 정의 방식으로 처리할 수 있습니다.
 */
public interface ErrorListener {
    /**
     * onError 메소드는 오류 발생 시 호출됩니다.
     * 이 메소드는 오류 메시지와 발생한 예외에 대한 정보를 매개변수로 받아,
     * 사용자가 정의한 오류 처리 로직을 실행할 수 있게 합니다.
     *
     * @param message 발생한 오류에 대한 메시지입니다.
     * @param e 오류 상황에 대한 예외 정보입니다.
     */
    void onError(String message, Exception e);
}
```

