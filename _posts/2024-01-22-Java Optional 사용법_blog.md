---
layout: post
title: "Java Optional 사용법"
date: 2024-01-22
tags: [Java, Optional, 용법]
categories: blog
---

## 사용자 입력:
> 자바에서 Optional은 언제 쓰는가? optional도 not null 체크 해야 되는 것 같은데.

## ChatGPT 응답:
> Java의 `Optional`은 주로 메소드에서 반환값으로 사용되며, 메소드가 어떤 경우에는 값을 반환하지 않을 수 있는 상황을 나타낼 때 유용합니다. 주로 다음과 같은 경우에 `Optional`을 사용합니다:

1. **메소드에서 값이 없을 때 반환**: 메소드가 어떤 이유로 값이 없을 수 있는 경우, `Optional`을 사용하여 명시적으로 값이 없음을 나타낼 수 있습니다. 이는 메소드의 사용자에게 반환값이 항상 존재하지 않을 수 있다는 것을 알려줍니다.

    ```java
    public Optional<String> findValueById(int id) {
        // id에 해당하는 값을 찾지 못한 경우
        if (/* 값이 없는 조건 */) {
            return Optional.empty();
        } else {
            return Optional.of("foundValue");
        }
    }
    ```

2. **null 대신 사용**: Java에서 `null`은 오류의 원인이 될 수 있기 때문에, `Optional`은 값이 없는 상황을 나타낼 때 `null` 대신 사용할 수 있습니다. 이는 코드에서 `NullPointerException`을 방지할 수 있습니다.

    ```java
    Optional<String> optionalValue = /* 어떤 값이나 없을 수 있는 값 */;
    
    // 값이 있는지 체크
    if (optionalValue.isPresent()) {
        String value = optionalValue.get();
        // 값이 있는 경우 처리
    } else {
        // 값이 없는 경우 처리
    }
    ```

3. **컬렉션에서 요소 가져오기**: 컬렉션에서 요소를 가져올 때, 해당 요소가 없을 수 있는 경우에 `Optional`을 사용할 수 있습니다.

    ```java
    List<String> values = Arrays.asList("one", "two", "three");
    
    Optional<String> firstElement = values.stream().findFirst();
    
    // 값이 있는지 체크 후 처리
    if (firstElement.isPresent()) {
        String value = firstElement.get();
        // 값이 있는 경우 처리
    } else {
        // 값이 없는 경우 처리
    }
    ```

`Optional`을 사용할 때에도 널 체크를 해야 하는데, 이는 `Optional`이 값이 없는 경우 `null`이 아닌 `Optional.empty()`를 반환하기 때문입니다. 따라서 `Optional`을 사용할 때에도 값이 있는지 여부를 확인하고 안전하게 처리해야 합니다.

## 사용자 입력:
> parentOptional.isEmpty() 해도 java.util.NoSuchElementException: No value present 이런 오류가 생기던데.

## ChatGPT 응답:
> `parentOptional.isEmpty()` 메소드는 `Optional`이 비어있는지 여부를 체크하는 메소드가 아닙니다. 실제로는 `Optional`에 값이 있는지 여부를 확인하기 위해 `isPresent()` 메소드를 사용해야 합니다. `isEmpty()`는 자바 11 이후에 추가된 `Optional` 클래스의 메소드로, 해당 메소드는 내부적으로 `isPresent()`와 같은 동작을 수행합니다.

따라서 올바른 방법은 다음과 같이 `isPresent()`를 사용하는 것입니다:

```java
if (parentOptional.isPresent()) {
    // Optional에 값이 있는 경우 처리
} else {
    // Optional이 비어있는 경우 처리
}
```

이를테면 다음과 같이 사용할 수 있습니다:

```java
Optional<String> parentOptional = /* 어떤 Optional */;
if (parentOptional.isPresent()) {
    // Optional에 값이 있는 경우
    String value = parentOptional.get();
    // 값에 대한 처리
} else {
    // Optional이 비어있는 경우
    // 비어있는 경우의 처리
}
```

`Optional`의 `isPresent()` 메소드를 사용하여 값이 존재하는지 여부를 확인하고, 값에 접근할 때는 `get()` 메소드를 사용할 수 있습니다. 다만, `get()`을 호출하기 전에 `isPresent()`로 반드시 체크해야 `NoSuchElementException`을 방지할 수 있습니다.

## 사용자 입력:
> parentOtpinal의 값이 Optional.empty() 이면 어떻게 돼?

## ChatGPT 응답:
> `Optional.empty()`는 값이 없는 `Optional`을 나타냅니다. 따라서 `parentOptional`이 `Optional.empty()`인 경우, `isPresent()` 메소드는 `false`를 반환하게 됩니다. 이를테면 다음과 같이 확인할 수 있습니다:

```java
Optional<String> parentOptional = Optional.empty();

if (parentOptional.isPresent()) {
    // Optional에 값이 있는 경우
    String value = parentOptional.get();
    // 값에 대한 처리
} else {
    // Optional이 비어있는 경우
    // 비어있는 경우의 처리
    System.out.println("parentOptional is empty.");
}
```

`isPresent()`가 `false`를 반환하면 `Optional`이 비어있음을 나타내므로, `else` 블록이 실행되어 해당 상황을 처리할 수 있습니다. 이것이 `Optional`을 사용하는 핵심 아이디어 중 하나입니다.

