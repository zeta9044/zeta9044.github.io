---
layout: post
title: "Thread Safe List in Java"
date: 2023-10-18
tags: [Thread, in, List, Java, Safe]
categories: blog
---

## 사용자 입력:
> java8이고, thread safe한 List 알려줘.

## ChatGPT 응답:
> Java 8에서는 `Collections.synchronizedList()` 메서드를 사용하여 기존의 `List`를 스레드 안전(thread safe)하게 만들 수 있습니다. 그렇게 하면 다중 스레드 환경에서 해당 리스트에 동시에 접근하더라도 데이터의 일관성이 유지됩니다.

예시:

```java
List<String> list = new ArrayList<>();
List<String> synchronizedList = Collections.synchronizedList(list);
```

그러나 `Collections.synchronizedList()`로 생성된 리스트를 사용할 때 몇 가지 주의 사항이 있습니다:

1. **반복자 사용 시 동기화**: `synchronizedList`에 대한 반복자(iterators) 사용 시 외부에서 수동으로 리스트를 동기화해야 합니다. 그렇지 않으면 동시 수정에 의한 `ConcurrentModificationException`이 발생할 수 있습니다.

```java
synchronized (synchronizedList) {
    for (String s : synchronizedList) {
        // Do something
    }
}
```

2. **동기화로 인한 성능 저하**: 동기화는 동시성을 보장하기 위해 필요하지만, 그로 인해 성능에 영향을 줄 수 있습니다. 따라서 필요한 경우에만 동기화된 리스트를 사용하고, 가능하면 동시성을 처리하는 다른 전략을 고려하는 것이 좋습니다.

3. **다른 동시성 컬렉션 고려**: Java는 `java.util.concurrent` 패키지에서 다양한 동시성 컬렉션을 제공합니다. 예를 들면, `CopyOnWriteArrayList`는 읽기 작업이 빈번하고 쓰기 작업이 드물 때 유용하며, 스레드 안전성을 제공합니다.

```java
List<String> concurrentList = new CopyOnWriteArrayList<>();
```

사용하는 상황에 따라 적합한 동시성 컬렉션을 선택하는 것이 중요합니다.

