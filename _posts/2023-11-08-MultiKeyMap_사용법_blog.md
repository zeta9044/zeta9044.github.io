---
layout: post
title: "MultiKeyMap 사용법"
date: 2023-11-08
tags: [용법, MultiKeyMap]
categories: blog
---

## 사용자 입력:
> apache collections4에 muti  key를 가진 map 자료형이 있나? 사용법은?

## ChatGPT 응답:
> Apache Commons Collections 라이브러리에는 다양한 맵 구현이 포함되어 있습니다. 그 중 하나가 여러 키를 가질 수 있는 `MultiKeyMap`입니다. 이 자료형은 키가 두 개 이상인 맵을 구현할 때 유용합니다. 각 키의 조합이 고유해야 하며, 이 조합이 하나의 값을 매핑합니다.

`MultiKeyMap`의 사용 예는 다음과 같습니다:

1. 라이브러리 추가:
   Apache Commons Collections를 사용하려면, 먼저 이 라이브러리를 프로젝트에 추가해야 합니다. Maven 프로젝트의 경우 `pom.xml`에 다음 종속성을 추가할 수 있습니다.
   ```xml
   <dependency>
       <groupId>org.apache.commons</groupId>
       <artifactId>commons-collections4</artifactId>
       <version>4.4</version> <!-- 사용하는 버전에 따라 버전 번호를 확인하세요 -->
   </dependency>
   ```

2. `MultiKeyMap` 인스턴스 생성:
   ```java
   MultiKeyMap<Object, String> map = new MultiKeyMap<>();
   ```

3. 값 추가:
   `MultiKeyMap`에 값 추가는 여러 키를 사용하여 수행됩니다.
   ```java
   map.put("Key1", "Key2", "결합된 키에 매핑된 값");
   ```

4. 값 검색:
   값 검색은 추가했던 동일한 키의 조합을 사용하여 수행됩니다.
   ```java
   String value = map.get("Key1", "Key2");
   ```

5. 값 제거:
   특정 키 조합에 대한 값을 제거할 수도 있습니다.
   ```java
   map.remove("Key1", "Key2");
   ```

`MultiKeyMap`을 사용하면 각 키 조합이 유일하다는 것을 기억하는 것이 중요합니다. 같은 키 조합을 다시 사용하면 기존 값이 새 값으로 대체됩니다. 이 자료구조는 두 개 이상의 키를 조합하여 하나의 값에 액세스해야 하는 복잡한 데이터 구조에 적합합니다.

## 사용자 입력:
> MultiKeyMap도 sort할 수 있나?  멀티 키의 순서 정렬로

## ChatGPT 응답:
> `MultiKeyMap` 자체에는 정렬 기능이 내장되어 있지 않습니다. 그러나, 당신이 `MultiKeyMap`의 내용을 정렬하고 싶다면, 몇 가지 접근 방식을 사용할 수 있습니다.

1. **Map을 정렬 가능한 구조로 변환**:
   `MultiKeyMap`의 내용을 정렬할 때는 키를 정렬 기준으로 사용하여 새로운 `SortedMap`이나 `TreeMap`과 같은 정렬 가능한 구조로 변환해야 합니다.

2. **커스텀 Comparator**:
   멀티 키의 각 조합을 비교할 수 있는 커스텀 `Comparator`를 구현하고, 이를 사용하여 키의 컬렉션을 정렬합니다. 이 방법은 다소 복잡할 수 있으며, 모든 키 조합을 비교할 수 있도록 상세한 로직을 작성해야 합니다.

3. **정렬된 리스트 만들기**:
   `MultiKeyMap`에서 모든 키를 리스트로 추출한 다음, `Collections.sort()` 또는 `List.sort()` 메소드를 사용하여 정렬할 수 있습니다. 이 때, 커스텀 `Comparator`를 사용하여 키들을 원하는 순서로 정렬할 수 있습니다.

다음은 `MultiKeyMap`의 키를 추출하고 정렬하는 방법의 예시입니다:

```java
MultiKeyMap<Object, String> multiKeyMap = new MultiKeyMap<>();

// MultiKeyMap에 데이터 추가
multiKeyMap.put("Key1", "Key2", "Value1");
multiKeyMap.put("Key3", "Key4", "Value2");
// ...

// 모든 키를 리스트로 추출
List<MultiKey<? extends Object>> keys = new ArrayList<>(multiKeyMap.keySet());

// 커스텀 Comparator로 정렬
keys.sort(new Comparator<MultiKey<? extends Object>>() {
    @Override
    public int compare(MultiKey<? extends Object> o1, MultiKey<? extends Object> o2) {
        // 정렬 로직 구현
        // 예를 들어 첫 번째 키를 기준으로 정렬한다면:
        return ((String) o1.getKey(0)).compareTo((String) o2.getKey(0));
    }
});

// 정렬된 키를 사용하여 정렬된 값들의 리스트를 생성
List<String> sortedValues = keys.stream()
                                .map(key -> multiKeyMap.get(key.getKey(0), key.getKey(1)))
                                .collect(Collectors.toList());
```

이 예시에서는 `MultiKeyMap`의 키들을 리스트로 추출하고, 첫 번째 키를 기준으로 정렬하는 커스텀 `Comparator`를 사용하여 정렬합니다. 그리고 정렬된 키에 따른 값을 리스트로 만듭니다. 실제 사용 시에는 키의 데이터 타입과 정렬 조건에 맞춰 `Comparator`를 구현해야 합니다.

## 사용자 입력:
> 위에는 첫번째 키 기준으로 구현되어 있는데, 첫번째키로 정렬을 완료하고 하나서, 2번째키로 정렬하는 방법을 알려줘. 물론, 첫번째 키 순서는 바뀌면 안되겠지.

## ChatGPT 응답:
> 두 개의 키에 대해 `MultiKeyMap`을 정렬하고 싶을 때, 첫 번째 키로 우선 정렬하고 그 다음에 두 번째 키로 정렬을 유지하려면, 첫 번째 키가 동일할 경우에만 두 번째 키를 비교하는 방식의 `Comparator`를 구현해야 합니다. 이는 Java의 `Comparator`를 연쇄적으로 사용하여 수행할 수 있습니다.

다음은 첫 번째 키로 정렬한 후 두 번째 키로 정렬을 유지하는 `Comparator` 구현 예시입니다:

```java
// Comparator를 생성하여 첫 번째 키로 정렬하고, 동일할 경우 두 번째 키로 정렬
Comparator<MultiKey<? extends Object>> multiKeyComparator = new Comparator<MultiKey<? extends Object>>() {
    @Override
    public int compare(MultiKey<? extends Object> o1, MultiKey<? extends Object> o2) {
        // 첫 번째 키 비교
        int firstKeyComparison = ((Comparable) o1.getKey(0)).compareTo(o2.getKey(0));
        
        // 첫 번째 키가 같으면 두 번째 키로 비교
        if (firstKeyComparison == 0) {
            return ((Comparable) o1.getKey(1)).compareTo(o2.getKey(1));
        }
        
        // 첫 번째 키가 다르면 첫 번째 키 비교 결과 반환
        return firstKeyComparison;
    }
};

// 키 추출 및 정렬
List<MultiKey<? extends Object>> keys = new ArrayList<>(multiKeyMap.keySet());
keys.sort(multiKeyComparator);

// 정렬된 키를 사용하여 정렬된 값들의 리스트를 생성
List<String> sortedValues = keys.stream()
                                .map(key -> multiKeyMap.get(key.getKey(0), key.getKey(1)))
                                .collect(Collectors.toList());
```

이 `Comparator`는 첫 번째 키를 비교하여 정렬하고, 만약 첫 번째 키가 같은 경우에는 두 번째 키를 비교하여 정렬 순서를 결정합니다. 이 방식을 사용하면 첫 번째 키의 순서를 유지하면서 두 번째 키에 대해 세부 정렬을 할 수 있습니다. 

각 키의 데이터 타입이 `Comparable` 인터페이스를 구현하고 있어야 한다는 점을 유의해야 합니다. 그렇지 않은 경우, 적절한 타입 캐스팅과 예외 처리가 필요할 수 있습니다.

## 사용자 입력:
> 위에 정렬된 MultiKeyMap에서 value만 추출하는 방법은?

## ChatGPT 응답:
> 정렬된 `MultiKeyMap`의 키들을 바탕으로 값을 추출하는 것은 스트림을 사용하여 간단하게 할 수 있습니다. 이미 정렬된 키 리스트를 가지고 있기 때문에, 그 순서대로 `MultiKeyMap`에서 값을 추출하면 됩니다. 

아래의 코드는 정렬된 키 리스트(`keys`)를 이용하여 값들만 추출하여 리스트(`sortedValues`)로 만드는 방법을 보여줍니다:

```java
// 정렬된 키 리스트에서 값들만 추출하여 새로운 리스트 생성
List<String> sortedValues = keys.stream()
                                .map(key -> multiKeyMap.get(key.getKey(0), key.getKey(1)))
                                .collect(Collectors.toList());
```

이 코드 스니펫은 Java 8 이상의 버전에서 작동하는데, 스트림 API를 사용하여 각 `MultiKey`에 대한 값을 `multiKeyMap.get()` 메소드로 검색하고, 결과값을 리스트로 수집합니다(`collect(Collectors.toList())`). 결과는 정렬된 키에 대응하는 값들의 정렬된 리스트입니다.

만약 스트림을 사용하지 않고 싶다면, 간단한 반복문을 사용할 수도 있습니다:

```java
List<String> sortedValues = new ArrayList<>();
for(MultiKey<? extends Object> key : keys) {
    sortedValues.add(multiKeyMap.get(key.getKey(0), key.getKey(1)));
}
```

이 코드는 `keys` 리스트를 순회하면서 각 `MultiKey`에 대응하는 값을 `multiKeyMap`에서 검색하고, `sortedValues` 리스트에 추가합니다. 결과적으로 `sortedValues` 리스트에는 정렬된 순서대로 값이 담기게 됩니다.

## 사용자 입력:
> 만약에 key가 3개이면, 정렬하는 방법은?

## ChatGPT 응답:
> 키가 세 개인 `MultiKeyMap`을 정렬하려면, `Comparator`를 구현할 때 세 개의 키 모두를 고려해야 합니다. 첫 번째 키를 우선 정렬 기준으로 삼고, 첫 번째 키가 동일할 경우 두 번째 키로 비교하며, 첫 번째와 두 번째 키가 모두 동일할 경우에만 세 번째 키를 비교하는 방식으로 정렬합니다.

다음은 세 개의 키를 갖는 `MultiKeyMap`을 정렬하는 `Comparator` 구현 예시입니다:

```java
Comparator<MultiKey<? extends Object>> multiKeyComparator = new Comparator<MultiKey<? extends Object>>() {
    @Override
    public int compare(MultiKey<? extends Object> o1, MultiKey<? extends Object> o2) {
        // 첫 번째 키 비교
        int firstKeyComparison = ((Comparable) o1.getKey(0)).compareTo(o2.getKey(0));
        if (firstKeyComparison != 0) {
            return firstKeyComparison;
        }

        // 두 번째 키 비교
        int secondKeyComparison = ((Comparable) o1.getKey(1)).compareTo(o2.getKey(1));
        if (secondKeyComparison != 0) {
            return secondKeyComparison;
        }

        // 세 번째 키 비교
        return ((Comparable) o1.getKey(2)).compareTo(o2.getKey(2));
    }
};

// 키 추출 및 정렬
List<MultiKey<? extends Object>> keys = new ArrayList<>(multiKeyMap.keySet());
keys.sort(multiKeyComparator);

// 정렬된 키를 사용하여 정렬된 값들의 리스트를 생성
List<String> sortedValues = keys.stream()
                                .map(key -> multiKeyMap.get(key.getKey(0), key.getKey(1), key.getKey(2)))
                                .collect(Collectors.toList());
```

위의 `Comparator`는 첫 번째 키의 비교 결과가 0이 아닌 경우, 즉 첫 번째 키가 동일하지 않은 경우에는 첫 번째 키의 비교 결과를 바로 반환합니다. 첫 번째 키가 동일하다면 두 번째 키를 비교하고, 두 번째 키도 동일하다면 세 번째 키를 비교하여 최종 결과를 반환합니다.

이 방법을 사용하면 첫 번째 키가 가장 우선시되며, 첫 번째 키가 동일한 경우 두 번째 키가, 첫 번째와 두 번째 키가 모두 동일한 경우에는 세 번째 키가 정렬 기준이 됩니다. 그 결과로 생성되는 `sortedValues` 리스트는 세 키 모두를 고려하여 정렬된 값들의 리스트가 됩니다.

이 예시에서는 모든 키가 `Comparable` 인터페이스를 구현하고 있다고 가정하고 있습니다. 만약 구현하고 있지 않다면, 적절한 타입 캐스팅과 예외 처리를 해야 합니다. 또한, 실제 비교 로직은 키의 실제 데이터 타입과 비교하고자 하는 기준에 따라 달라질 수 있습니다.

## 사용자 입력:
> 추출한 value에서 key를 추출할 수 있나?

## ChatGPT 응답:
> `MultiKeyMap`에서 값을 추출한 후에는 보통 그 값에 대응하는 키를 알고 있습니다. 그러나, 만약 키가 알려져 있지 않다면, 주어진 값에 대응하는 모든 키를 찾는 것은 `MultiKeyMap`의 API로 직접적으로 제공되지 않습니다. 이러한 작업을 수행하려면, 맵을 반복 순회하며 값을 검사해야 합니다.

예를 들어, 특정 값을 가지는 모든 키 조합을 찾으려면 다음과 같이 할 수 있습니다:

```java
String targetValue = "찾고자 하는 값";
List<MultiKey<?>> keysWithTargetValue = new ArrayList<>();

for (MultiKey<?> key : multiKeyMap.keySet()) {
    if (multiKeyMap.get(key.getKey(0), key.getKey(1), key.getKey(2)).equals(targetValue)) {
        keysWithTargetValue.add(key);
    }
}
```

이 코드는 `MultiKeyMap`의 모든 키를 순회하며, 각 키에 대응하는 값을 `targetValue`와 비교합니다. 값이 일치하면 해당 키를 결과 리스트에 추가합니다. 이 작업을 통해 주어진 값에 대응하는 모든 키 조합을 찾을 수 있습니다.

그러나 이 방법은 큰 맵에서는 비효율적일 수 있습니다. 값에서 키를 찾는 작업은 맵의 기본적인 사용 방법과 반대이므로, 이러한 작업이 자주 필요한 경우라면 `MultiKeyMap` 대신 다른 데이터 구조를 사용하는 것을 고려해야 할 수 있습니다. 예를 들어, 양방향 맵 구조인 `BidiMap`이나 값에 대한 키를 저장하는 별도의 맵을 유지하는 것 등의 방법이 있습니다.

## 사용자 입력:
>  MultiKeyMap<CPP14Parser.StatementContext,Integer,Integer,AIFuncCall> callfuncMap =  new MultiKeyMap<>(); 이렇게 선언못하나?

## ChatGPT 응답:
> `MultiKeyMap`을 사용할 때, Java의 제네릭은 키의 타입에 대한 정보만을 필요로 하며, 값의 타입에 대한 정보는 필요하지 않습니다. 키는 `MultiKey` 객체의 조합으로 구성되며, `MultiKeyMap`의 제네릭은 이 `MultiKey`의 타입을 명시합니다. 값을 선언할 때는 단일 타입 매개변수만 사용됩니다.

예를 들어, `MultiKeyMap`의 키가 `Integer` 타입 두 개와 `String` 타입 하나로 구성되고, 값이 `AIFuncCall` 타입인 경우 다음과 같이 선언할 수 있습니다:

```java
MultiKeyMap<MultiKey<?>, AIFuncCall> callfuncMap = new MultiKeyMap<>();
```

여기서 `MultiKey<?>`는 키의 조합을 의미하고, `AIFuncCall`은 맵에 저장될 값의 타입입니다.

키를 세 개 사용하는 경우에는 `MultiKey`에 세 개의 키를 넣어서 사용합니다. 예를 들어, 값에 키를 매핑하는 방법은 다음과 같습니다:

```java
CPP14Parser.StatementContext statementContext = // ... get or create a StatementContext
Integer key1 = // ... some Integer key
Integer key2 = // ... another Integer key
AIFuncCall value = // ... create an AIFuncCall

callfuncMap.put(new MultiKey<>(statementContext, key1, key2), value);
```

이렇게 `MultiKey` 객체를 만들 때 세 개의 키를 제공하여 `MultiKeyMap`에 추가합니다. `MultiKeyMap`은 내부적으로 `MultiKey` 객체를 키로 사용하여 값에 대한 참조를 저장합니다.

`MultiKeyMap`에 대한 제네릭 선언은 키의 타입만 명시하며, 값의 타입은 별도의 매개변수로 지정합니다. 따라서, 키의 타입 조합을 제네릭으로 지정하는 방법은 존재하지 않습니다. 값의 타입만 지정하면 됩니다.

## 사용자 입력:
> CPP14Parser.StatementContext statementContext  이것은 antlr4 rule인데, callfuncMap.put(new MultiKey<>(statementContext, key1, key2), value); 여기서 에러나..

## ChatGPT 응답:
> `MultiKeyMap`을 사용할 때 `MultiKey`를 만들기 위해 제공하는 객체들은 `equals()`와 `hashCode()` 메소드를 적절하게 구현해야 합니다. 이는 `MultiKeyMap`이 내부적으로 `hashCode`를 사용하여 키를 효율적으로 찾기 때문입니다. ANTLR4에서 생성된 `ParserRuleContext` (예: `CPP14Parser.StatementContext`) 객체는 이러한 메소드들을 기본적으로 오버라이드하지 않을 수 있습니다.

`MultiKeyMap.put` 메소드를 사용할 때 에러가 발생한다면, 그 이유는 여러 가지일 수 있습니다:

1. **hashCode/equals 구현**: `statementContext` 객체가 `hashCode()`와 `equals()` 메소드를 적절하게 구현하고 있지 않아서, `MultiKey` 객체를 올바르게 관리하지 못할 수 있습니다.

2. **타입 문제**: `key1`과 `key2`가 `Integer` 타입이 아니거나, `value`가 `AIFuncCall` 타입이 아닌 경우 타입 불일치로 인한 에러가 발생할 수 있습니다.

3. **라이브러리 충돌**: ANTLR4 런타임과 Apache Commons Collections 라이브러리 간의 호환성 문제로 인해 충돌이 발생할 수 있습니다.

에러를 정확히 해결하려면, 에러 메시지를 확인해야 합니다. 에러 메시지는 문제가 무엇인지, 어디에서 발생하는지에 대한 중요한 단서를 제공합니다. 만약 `hashCode()`와 `equals()` 메소드의 구현이 문제라면, `CPP14Parser.StatementContext` 클래스를 확장하여 이 메소드들을 오버라이드하거나, `statementContext`를 감싸는 래퍼 클래스를 만들어 이 메소드들을 적절히 구현할 수 있습니다.

예를 들어:

```java
public class StatementContextWrapper {
    private final CPP14Parser.StatementContext statementContext;

    public StatementContextWrapper(CPP14Parser.StatementContext statementContext) {
        this.statementContext = statementContext;
    }

    // equals와 hashCode 메소드 구현
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        StatementContextWrapper that = (StatementContextWrapper) o;
        return Objects.equals(statementContext, that.statementContext);
    }

    @Override
    public int hashCode() {
        return Objects.hash(statementContext);
    }

    // 필요한 경우 statementContext에 대한 추가적인 접근자 메소드를 구현할 수 있습니다.
}
```

그런 다음 `MultiKeyMap`에 값을 넣을 때 `StatementContextWrapper`를 사용할 수 있습니다:

```java
callfuncMap.put(new MultiKey<>(new StatementContextWrapper(statementContext), key1, key2), value);
```

이렇게 하면 `statementContext`가 올바른 `equals`와 `hashCode` 구현을 가진 객체로 래핑되어 `MultiKeyMap`이 올바르게 작동하게 됩니다.

