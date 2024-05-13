---
layout: post
title: "자바 MultiValueMap 사용법"
date: 2023-07-07
tags: [용법, MultiValueMap, 자바]
categories: blog
---

## 사용자 입력:
> 자바 아파치 재단의 MultiValueMap 사용법 알려줘.

## ChatGPT 응답:
> 자바에서 Apache Commons 라이브러리를 사용하여 MultiValueMap을 활용할 수 있습니다. MultiValueMap은 하나의 키에 여러 개의 값을 연결할 수 있는 자료구조입니다. 이를 사용하면 하나의 키에 여러 개의 값을 저장하고 검색하는 것이 간단해집니다. 아래는 MultiValueMap을 사용하는 방법에 대한 예시입니다.

먼저, Apache Commons 라이브러리를 프로젝트에 추가해야 합니다. Maven을 사용하는 경우 pom.xml 파일에 다음 종속성을 추가합니다:

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.4</version>
</dependency>
```

Gradle을 사용하는 경우 build.gradle 파일에 다음 종속성을 추가합니다:

```gradle
implementation 'org.apache.commons:commons-collections4:4.4'
```

이제 MultiValueMap을 사용하여 여러 값을 연결할 수 있습니다. 다음은 예시 코드입니다:

```java
import org.apache.commons.collections4.MultiValueMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;

public class MultiValueMapExample {
    public static void main(String[] args) {
        MultiValueMap<String, String> multiValueMap = new ArrayListValuedHashMap<>();

        // 값을 추가합니다.
        multiValueMap.put("key", "value1");
        multiValueMap.put("key", "value2");

        // 하나의 키에 연결된 모든 값을 가져옵니다.
        System.out.println(multiValueMap.get("key")); // 출력: [value1, value2]

        // 모든 키를 가져옵니다.
        System.out.println(multiValueMap.keySet()); // 출력: [key]

        // 키와 연결된 모든 값을 가져옵니다.
        System.out.println(multiValueMap.entries()); // 출력: [key=value1, key=value2]

        // 값의 개수를 가져옵니다.
        System.out.println(multiValueMap.size()); // 출력: 2

        // 특정 값을 포함하는지 확인합니다.
        System.out.println(multiValueMap.containsValue("value1")); // 출력: true

        // 특정 값을 가진 모든 키를 가져옵니다.
        System.out.println(multiValueMap.keys()); // 출력: [key, key]

        // 값을 제거합니다.
        multiValueMap.remove("key", "value1");

        // 특정 키의 모든 값을 제거합니다.
        multiValueMap.removeAll("key");

        // MultiValueMap이 비어있는지 확인합니다.
        System.out.println(multiValueMap.isEmpty()); // 출력: true
    }
}
```

위의 예시에서는 `ArrayListValuedHashMap` 클래스를 사용하여 MultiValueMap을 생성했습니다. 다른 구현체도 사용할 수 있으며, `ArrayListValuedHashMap`은 키와 값의 순서를 보존하고 중복 값을 허용합니다.

이와 같이 MultiValueMap을 사용하면 자바에서 여러 값을 하나의 키에 연결하여 관리할 수 있습니다.

