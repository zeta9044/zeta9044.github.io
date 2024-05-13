---
layout: post
title: "Java 8 HashMap Loop"
tags: [Loop, HashMap, Java]
---

## 사용자 입력:
> 자바의 해시맵에서 key갯수만큼 loop돌리는 방법. java8이상. 함수적 방법 제시

## ChatGPT 응답:
> Java 8부터는 스트림 API를 사용하여 컬렉션을 함수적으로 처리할 수 있습니다. 자바의 {% raw %}`HashMap`{% endraw %}에서 키만을 대상으로 반복하는 경우, 다음 방법을 사용할 수 있습니다:

1. {% raw %}`keySet()`{% endraw %} 메소드를 사용하여 {% raw %}`HashMap`{% endraw %}의 키 세트를 가져옵니다.
2. {% raw %}`stream()`{% endraw %} 메소드를 사용하여 키 세트를 스트림으로 변환합니다.
3. {% raw %}`forEach()`{% endraw %} 메소드를 사용하여 각 키에 대한 작업을 수행합니다.

예제:

{% raw %}```java
import java.util.HashMap;

public class HashMapExample {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("one", 1);
        map.put("two", 2);
        map.put("three", 3);
        
        // Java 8 함수적 방법을 사용하여 키를 출력
        map.keySet().stream().forEach(key -> {
            System.out.println(key);
        });
    }
}
```{% endraw %}

위의 코드는 {% raw %}`HashMap`{% endraw %}에 저장된 모든 키를 출력합니다. {% raw %}`forEach`{% endraw %} 내부에서는 원하는 작업을 수행할 수 있습니다.

## 사용자 입력:
> value를 대상으로 할려면?

## ChatGPT 응답:
> {% raw %}`HashMap`{% endraw %}에서 값만을 대상으로 반복하려면 {% raw %}`values()`{% endraw %} 메소드를 사용하여 {% raw %}`HashMap`{% endraw %}의 값 컬렉션을 가져올 수 있습니다. 그런 다음 해당 컬렉션을 스트림으로 변환하고 {% raw %}`forEach()`{% endraw %}를 사용하여 각 값에 대한 작업을 수행합니다.

예제:

{% raw %}```java
import java.util.HashMap;

public class HashMapExample {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("one", 1);
        map.put("two", 2);
        map.put("three", 3);
        
        // Java 8 함수적 방법을 사용하여 값(value)를 출력
        map.values().stream().forEach(value -> {
            System.out.println(value);
        });
    }
}
```{% endraw %}

위의 코드는 {% raw %}`HashMap`{% endraw %}에 저장된 모든 값을 출력합니다. {% raw %}`forEach`{% endraw %} 내부에서는 원하는 작업을 수행할 수 있습니다.

## 사용자 입력:
> 해시맵은 동일키가 중복해서 들어가는 건 아니고, override되지?

## ChatGPT 응답:
> 네, 맞습니다. {% raw %}`HashMap`{% endraw %}에서 동일한 키로 값을 추가하게 되면 이전에 해당 키에 대해 저장된 값은 덮어씌워집니다 (즉, 오버라이드됩니다). 따라서 {% raw %}`HashMap`{% endraw %}에는 동일한 키에 대해 여러 개의 값이 존재할 수 없습니다.

예를 들어:

{% raw %}```java
import java.util.HashMap;

public class HashMapExample {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("one", 1);
        System.out.println(map.get("one"));  // 출력: 1
        
        map.put("one", 100);
        System.out.println(map.get("one"));  // 출력: 100
    }
}
```{% endraw %}

위의 코드에서 "one"이라는 키에 1이라는 값을 먼저 저장하고, 다시 "one"이라는 키에 100이라는 값을 저장했습니다. 결과적으로 "one" 키의 값은 100으로 오버라이드되었습니다.

