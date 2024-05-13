---
layout: post
title: "JUnit4 Assert Exception 사용법"
date: 2023-11-22
tags: [Assert, Exception, 용법]
categories: blog
---

## 사용자 입력:
> Junit4에서 Assert Exception 쓰는 법

## ChatGPT 응답:
> JUnit4에서 예외를 확인하는 방법은 주로 `@Test` 애너테이션과 함께 `expected` 속성을 사용하는 것입니다. 이 방법은 테스트 중에 특정 타입의 예외가 발생할 것으로 예상할 때 사용됩니다. 예를 들어, `IllegalArgumentException`이 발생할 것으로 예상하는 테스트 메소드는 다음과 같이 작성될 수 있습니다:

```java
import org.junit.Test;
import static org.junit.Assert.*;

public class MyTestClass {

    @Test(expected = IllegalArgumentException.class)
    public void testMethod() {
        // 여기에 예외를 발생시킬 수 있는 코드를 작성
    }
}
```

이 코드에서 `testMethod`는 `IllegalArgumentException`이 발생할 것으로 예상됩니다. 만약 이 예외가 발생하면 테스트는 성공으로 간주됩니다. 예외가 발생하지 않으면 테스트는 실패로 간주됩니다.

또 다른 방법은 `try-catch` 블록을 사용하는 것입니다. 이 방법은 예외의 특정 속성(예: 메시지)을 검증하고자 할 때 유용합니다. 예를 들어:

```java
public class MyTestClass {

    @Test
    public void testMethod() {
        try {
            // 예외를 발생시킬 수 있는 코드
            fail("Expected IllegalArgumentException was not thrown");
        } catch (IllegalArgumentException e) {
            // 필요한 경우 예외 메시지 등을 검증
            assertEquals("Expected error message", e.getMessage());
        }
    }
}
```

이 경우, 예외가 발생하지 않으면 `fail()` 메소드가 호출되어 테스트가 실패합니다. `IllegalArgumentException`이 발생하면 catch 블록 내의 코드가 실행되고, 여기에서 추가적인 검증을 수행할 수 있습니다.

