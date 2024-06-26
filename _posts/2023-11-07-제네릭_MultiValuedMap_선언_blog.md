---
layout: post
title: "제네릭 MultiValuedMap 선언"
tags: [선언, 릭, MultiValuedMap]
---

## 사용자 입력:
> 자바8에서 MultiValuedMap<CPP14Parser.StatementContext, CPP14Parser.PointerDeclaratorContext> o ;를 선언하는데, CPP14Parser.StatementContext 부분을 <T extends ParserRuleContext> 이렇게 제네릭으로 바꾸봐.

## ChatGPT 응답:
> {% raw %}`MultiValuedMap`{% endraw %}은 Apache Commons Collections 라이브러리에서 제공하는 인터페이스입니다. 이를 사용하여 키 하나에 여러 값을 매핑할 수 있습니다. 여기서 {% raw %}`CPP14Parser.StatementContext`{% endraw %}와 {% raw %}`CPP14Parser.PointerDeclaratorContext`{% endraw %}는 ANTLR에 의해 생성된 구문 분석 트리를 나타내는 클래스입니다.

제네릭을 사용하여 {% raw %}`MultiValuedMap`{% endraw %}의 첫 번째 타입 파라미터를 {% raw %}`CPP14Parser.StatementContext`{% endraw %}에서 더 범용적인 형태로 변경하려면, 다음과 같이 {% raw %}`T`{% endraw %}를 {% raw %}`ParserRuleContext`{% endraw %}의 하위 타입으로 제한하는 방식으로 선언할 수 있습니다:

{% raw %}```java
MultiValuedMap<T extends ParserRuleContext, CPP14Parser.PointerDeclaratorContext> o;
```{% endraw %}

하지만, Java의 제네릭은 타입 선언 시에 타입 파라미터를 정적으로 결정해야 하기 때문에, 위와 같이 선언만 해두고 나중에 구체적인 타입을 지정하는 것은 불가능합니다. 대신, 클래스나 메서드를 정의할 때 타입 파라미터를 지정해야 합니다.

예를 들어, 어떤 메서드 내에서 이 {% raw %}`MultiValuedMap`{% endraw %}을 사용한다고 가정하면 다음과 같이 메서드를 정의할 수 있습니다:

{% raw %}```java
public <T extends ParserRuleContext> void someMethod() {
    MultiValuedMap<T, CPP14Parser.PointerDeclaratorContext> o = new MultiValuedMapImpl<>();
    // ... 메서드의 나머지 부분
}
```{% endraw %}

또는 클래스 레벨에서 제네릭을 사용하는 경우:

{% raw %}```java
public class SomeClass<T extends ParserRuleContext> {
    private MultiValuedMap<T, CPP14Parser.PointerDeclaratorContext> o;

    public SomeClass() {
        o = new MultiValuedMapImpl<>();
        // ... 클래스의 나머지 부분
    }
    
    // ... 클래스의 다른 메서드들
}
```{% endraw %}

이렇게 하면, {% raw %}`SomeMethod`{% endraw %} 메서드나 {% raw %}`SomeClass`{% endraw %} 클래스를 사용할 때 구체적인 {% raw %}`ParserRuleContext`{% endraw %}의 하위 타입을 지정해주어야 합니다.

## 사용자 입력:
> Cannot resolve symbol 'T' 이렇게 나오는데.

## ChatGPT 응답:
> 제네릭 타입 {% raw %}`T`{% endraw %}를 사용하려면, 그 타입이 유효한 스코프 내에서 선언되어 있어야 합니다. 제가 이전에 보여드린 예시에서는 {% raw %}`T`{% endraw %}가 메서드 또는 클래스 레벨에서 적절하게 선언되어 있어야 합니다.

{% raw %}`T`{% endraw %}에 대한 컴파일 오류인 "Cannot resolve symbol 'T'"는 {% raw %}`T`{% endraw %}가 해당 스코프에서 인식되지 않았음을 의미합니다. 이 오류를 해결하려면 {% raw %}`T`{% endraw %}가 제네릭 타입 파라미터로서 해당 스코프에서 선언되어야 합니다.

클래스 레벨에서 {% raw %}`T`{% endraw %}를 사용하려면 다음과 같이 클래스 선언에 제네릭 타입을 추가해야 합니다:

{% raw %}```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.apache.commons.collections4.MultiValuedMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;

public class MyClass<T extends ParserRuleContext> {
    private MultiValuedMap<T, CPP14Parser.PointerDeclaratorContext> o;

    public MyClass() {
        o = new ArrayListValuedHashMap<>();
    }
    // ... 클래스의 나머지 부분
}
```{% endraw %}

메서드 레벨에서 {% raw %}`T`{% endraw %}를 사용하려면 다음과 같이 메서드 선언에 제네릭 타입을 추가해야 합니다:

{% raw %}```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.apache.commons.collections4.MultiValuedMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;

public class MyClass {

    public <T extends ParserRuleContext> void someMethod() {
        MultiValuedMap<T, CPP14Parser.PointerDeclaratorContext> o = new ArrayListValuedHashMap<>();
        // ... 메서드의 나머지 부분
    }
}
```{% endraw %}

이렇게 하면 {% raw %}`T`{% endraw %}가 유효한 제네릭 타입 파라미터로서 인식될 것이며, "Cannot resolve symbol 'T'" 오류는 해결될 것입니다.

그리고 {% raw %}`MultiValuedMapImpl`{% endraw %}을 사용하는 대신에, {% raw %}`ArrayListValuedHashMap`{% endraw %}과 같은 구체적인 {% raw %}`MultiValuedMap`{% endraw %} 구현을 사용했는데, 이는 {% raw %}`MultiValuedMap`{% endraw %} 인터페이스의 실제 구현체이기 때문입니다. {% raw %}`MultiValuedMapImpl`{% endraw %}이라는 클래스는 Apache Commons Collections에 존재하지 않습니다.

## 사용자 입력:
> MultiValuedMap< 제네릭, List<Object>> o; 이런 선언이 안되나?

## ChatGPT 응답:
> Java에서는 {% raw %}`MultiValuedMap`{% endraw %}의 키를 제네릭 타입으로 선언할 수 있습니다. 하지만, {% raw %}`MultiValuedMap`{% endraw %} 자체가 이미 키에 여러 값을 매핑하는 구조이므로, 값으로 {% raw %}`List<Object>`{% endraw %}를 사용하는 것은 일반적으로 중복된 형태의 데이터 구조를 만드는 것과 같습니다.

그러나 여러분의 요구에 따라 {% raw %}`MultiValuedMap`{% endraw %}의 값으로 {% raw %}`List<Object>`{% endraw %}를 사용하는 것이 필요하다면, 아래와 같이 선언할 수 있습니다:

{% raw %}```java
import org.apache.commons.collections4.MultiValuedMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;
import java.util.List;

public class MyClass<K> {
    private MultiValuedMap<K, List<Object>> o;

    public MyClass() {
        o = new ArrayListValuedHashMap<>();
    }
    // ... 클래스의 나머지 부분
}
```{% endraw %}

이 코드에서 {% raw %}`<K>`{% endraw %}는 제네릭 타입 파라미터를 의미하며, 클래스를 선언할 때 {% raw %}`<K>`{% endraw %}를 사용하여 클래스의 타입 파라미터를 정의하고 있습니다. 그런 다음 {% raw %}`MultiValuedMap<K, List<Object>>`{% endraw %}를 사용하여 {% raw %}`o`{% endraw %}를 선언하고 있습니다.

이렇게 하면 {% raw %}`MyClass`{% endraw %}를 인스턴스화할 때 {% raw %}`K`{% endraw %}에 대한 구체적인 타입을 제공해야 합니다. 예를 들어, {% raw %}`String`{% endraw %}을 키로 사용하려면 다음과 같이 {% raw %}`MyClass`{% endraw %}를 인스턴스화할 수 있습니다:

{% raw %}```java
MyClass<String> myClassInstance = new MyClass<>();
```{% endraw %}

{% raw %}`MultiValuedMap`{% endraw %}의 구현체로 {% raw %}`ArrayListValuedHashMap`{% endraw %}를 사용하고 있으며, 이는 Apache Commons Collections 라이브러리에서 제공하는 {% raw %}`MultiValuedMap`{% endraw %}의 구현 중 하나입니다. 

이런 방식으로 선언하면, 각 키에 대해 {% raw %}`List<Object>`{% endraw %} 타입의 여러 값을 매핑할 수 있습니다. 그러나 이것은 키마다 값의 리스트를 갖는 것이고, {% raw %}`MultiValuedMap`{% endraw %} 자체가 이미 키에 대한 여러 값을 관리하기 때문에, 일반적인 경우에는 {% raw %}`MultiValuedMap<K, Object>`{% endraw %}를 사용하는 것이 더 적절할 수 있습니다.

## 사용자 입력:
> 그러면,  MultiValuedMap<? extends ParserRuleContext, Object> callfuncMap; 이렇게 안되나?

## ChatGPT 응답:
> 네, Java에서는 와일드카드 제네릭 타입을 사용하여 {% raw %}`MultiValuedMap`{% endraw %}을 선언할 수 있습니다. {% raw %}`? extends SomeClass`{% endraw %} 형태의 와일드카드는 "SomeClass 또는 그 하위 타입의 어떤 타입이라도 될 수 있다"는 것을 의미합니다. 따라서, {% raw %}`MultiValuedMap<? extends ParserRuleContext, Object>`{% endraw %} 선언은 유효합니다.

이 선언을 통해 {% raw %}`callfuncMap`{% endraw %}은 {% raw %}`ParserRuleContext`{% endraw %}의 모든 하위 클래스 타입을 키로 받을 수 있는 {% raw %}`MultiValuedMap`{% endraw %}이 됩니다. 예를 들어, ANTLR이 생성한 구문 분석 트리의 여러 컨텍스트 타입들이 이 맵에 키로 들어갈 수 있습니다.

선언 예시는 다음과 같습니다:

{% raw %}```java
import org.apache.commons.collections4.MultiValuedMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;
import org.antlr.v4.runtime.ParserRuleContext;

public class Example {
    MultiValuedMap<? extends ParserRuleContext, Object> callfuncMap;

    public Example() {
        callfuncMap = new ArrayListValuedHashMap<>();
    }
    // ... 클래스의 다른 메서드나 필드들
}
```{% endraw %}

이런 방식으로 선언하면 {% raw %}`callfuncMap`{% endraw %}에는 {% raw %}`ParserRuleContext`{% endraw %} 또는 그 어떤 하위 타입의 인스턴스도 키로 사용할 수 있으며, 해당 키에 대응하는 값으로는 어떤 {% raw %}`Object`{% endraw %}도 사용할 수 있습니다. 그러나 이 맵에 값을 추가할 때는 와일드카드로 인해 제한이 생깁니다. {% raw %}`? extends ParserRuleContext`{% endraw %}는 "특정하지 않은 {% raw %}`ParserRuleContext`{% endraw %}의 하위 타입"을 의미하기 때문에, 이 맵에는 특정한 타입의 객체만 추가할 수 있습니다. 즉, {% raw %}`callfuncMap`{% endraw %}에 값을 추가할 때는 해당 특정 타입을 명시해야 하며, 무엇이든지 받을 수 있는 것은 아닙니다.

## 사용자 입력:
> callfuncMap 에 put할때 예시

## ChatGPT 응답:
> {% raw %}`MultiValuedMap<? extends ParserRuleContext, Object>`{% endraw %} 형태로 선언된 {% raw %}`callfuncMap`{% endraw %}에 값을 추가하려면, 맵을 선언할 때 사용한 와일드카드 제네릭 타입으로 인해 직접적으로 {% raw %}`put`{% endraw %} 메서드를 호출할 수 없습니다. 와일드카드를 사용하는 경우 컴파일러는 특정 {% raw %}`ParserRuleContext`{% endraw %}의 하위 타입이 어떤 것인지 알 수 없기 때문에, 타입 안정성을 보장할 수 없습니다.

따라서, {% raw %}`callfuncMap`{% endraw %}을 {% raw %}`MultiValuedMap<? extends ParserRuleContext, Object>`{% endraw %}로 직접 선언하기 보다는, 특정한 {% raw %}`ParserRuleContext`{% endraw %}의 하위 타입을 지정하는 것이 일반적입니다.

예를 들어, {% raw %}`MultiValuedMap<ParserRuleContext, Object>`{% endraw %} 타입으로 {% raw %}`callfuncMap`{% endraw %}을 선언하면, 어떤 {% raw %}`ParserRuleContext`{% endraw %}의 인스턴스도 키로 사용할 수 있습니다:

{% raw %}```java
import org.apache.commons.collections4.MultiValuedMap;
import org.apache.commons.collections4.multimap.ArrayListValuedHashMap;
import org.antlr.v4.runtime.ParserRuleContext;

public class Example {
    private MultiValuedMap<ParserRuleContext, Object> callfuncMap;

    public Example() {
        callfuncMap = new ArrayListValuedHashMap<>();
    }

    public void putValue(ParserRuleContext key, Object value) {
        callfuncMap.put(key, value);
    }
}
```{% endraw %}

이제 {% raw %}`putValue`{% endraw %} 메서드를 사용하여 {% raw %}`ParserRuleContext`{% endraw %}의 인스턴스와 그에 대응하는 값을 {% raw %}`callfuncMap`{% endraw %}에 추가할 수 있습니다:

{% raw %}```java
Example example = new Example();
ParserRuleContext someContext = // ... 여기에 ParserRuleContext 또는 그 하위 타입의 인스턴스를 생성
Object someValue = // ... 여기에 값을 정의

example.putValue(someContext, someValue);
```{% endraw %}

만약 여러분이 {% raw %}`? extends ParserRuleContext`{% endraw %} 와일드카드를 사용해야만 하는 특별한 경우가 있다면, 일반적으로는 이를 메서드 레벨에서 와일드카드 타입을 사용하지 않는 방식으로 처리해야 합니다. 예를 들어, 제네릭 메서드를 선언하여 이를 처리할 수 있습니다:

{% raw %}```java
public <T extends ParserRuleContext> void putValue(T key, Object value) {
    // 와일드카드가 아닌 구체적인 타입의 MultiValuedMap을 사용해야 함
    MultiValuedMap<T, Object> specificCallfuncMap = (MultiValuedMap<T, Object>) this.callfuncMap;
    specificCallfuncMap.put(key, value);
}
```{% endraw %}

위 메서드는 와일드카드를 사용하지 않고 {% raw %}`ParserRuleContext`{% endraw %}의 구체적인 하위 타입 {% raw %}`T`{% endraw %}에 대해서 {% raw %}`put`{% endraw %} 연산을 수행할 수 있게 합니다. 그러나, 이 메서드 내에서 타입 캐스팅을 사용하는 것은 타입 안전성을 손상시킬 수 있으므로 주의해서 사용해야 합니다. 실제로는 {% raw %}`MultiValuedMap`{% endraw %}의 선언을 와일드카드 없이 사용하는 것이 더 바람직합니다.

