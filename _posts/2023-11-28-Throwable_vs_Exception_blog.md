---
layout: post
title: "Throwable vs Exception"
tags: [Throwable, Exception, vs]
---

## 사용자 입력:
> 자바의 catch절에서 Exception을 Throwable 로 바꾸면, 메세지가 달라지나?

## ChatGPT 응답:
> 자바의 {% raw %}`catch`{% endraw %} 절에서 {% raw %}`Exception`{% endraw %}을 {% raw %}`Throwable`{% endraw %}로 바꾸면 처리할 수 있는 예외의 범위가 확장되고, 이로 인해 잡히는 예외의 유형이 다를 수 있습니다. 이에 따라 메시지가 달라질 수 있습니다. 

{% raw %}`Exception`{% endraw %} 클래스와 {% raw %}`Throwable`{% endraw %} 클래스 사이의 주요 차이점은 다음과 같습니다:

1. **범위**: {% raw %}`Throwable`{% endraw %}은 모든 에러와 예외를 포괄하는 최상위 클래스입니다. 반면, {% raw %}`Exception`{% endraw %}은 프로그램 실행 중에 발생할 수 있는 일반적인 예외 상황을 나타냅니다.

2. **포함하는 유형**:
   - {% raw %}`Exception`{% endraw %}은 {% raw %}`RuntimeException`{% endraw %} (unchecked 예외)과 그 외의 checked 예외들을 포함합니다.
   - {% raw %}`Throwable`{% endraw %}은 {% raw %}`Exception`{% endraw %}과 함께 {% raw %}`Error`{% endraw %} 클래스도 포함합니다. {% raw %}`Error`{% endraw %}는 시스템 에러와 같은 심각한 문제를 나타내며, 일반적으로 애플리케이션에서 처리하지 않습니다.

따라서, {% raw %}`catch`{% endraw %} 절에서 {% raw %}`Exception`{% endraw %} 대신 {% raw %}`Throwable`{% endraw %}을 사용하면, {% raw %}`Error`{% endraw %} 클래스에 속하는 예외들도 잡힐 수 있습니다. 이는 코드의 동작 방식에 영향을 미칠 수 있으며, 예외 처리 로직에 따라 다른 메시지를 출력할 수 있습니다.

그러나, 일반적으로 {% raw %}`Throwable`{% endraw %}을 직접 잡는 것은 권장되지 않습니다. 왜냐하면 이는 {% raw %}`Error`{% endraw %}와 같은 심각한 문제를 잡아내어 정상적인 예외 처리 흐름을 방해할 수 있기 때문입니다. 대부분의 경우, 구체적인 {% raw %}`Exception`{% endraw %} 유형을 사용하는 것이 더 안전하고 관리하기 쉽습니다.

## 사용자 입력:
> 그러면 catch를 나눠서 해야 되나?

## ChatGPT 응답:
> 예, 일반적으로 자바에서는 여러 유형의 예외를 다룰 때 {% raw %}`catch`{% endraw %} 절을 나눠서 사용하는 것이 좋습니다. 이렇게 하는 이유는 다음과 같습니다:

1. **특정 예외 처리**: 각 예외 유형에 맞게 특정한 처리 로직을 적용할 수 있습니다. 예를 들어, {% raw %}`IOException`{% endraw %}은 파일 입출력 오류를, {% raw %}`SQLException`{% endraw %}은 데이터베이스 오류를 처리하는 데 특화된 로직을 적용할 수 있습니다.

2. **코드의 가독성과 유지 관리**: 각 {% raw %}`catch`{% endraw %} 절에서 다루는 예외를 명확하게 하여, 코드의 가독성과 유지 관리가 용이해집니다.

3. **예외 처리의 정확성**: 더 구체적인 예외 유형을 잡음으로써, 예상치 못한 예외가 발생했을 때 적절한 조치를 취할 수 있습니다.

예를 들어, 다음과 같이 여러 {% raw %}`catch`{% endraw %} 절을 사용할 수 있습니다:

{% raw %}```java
try {
    // 예외가 발생할 수 있는 코드
} catch (IOException e) {
    // IOException 처리 로직
} catch (SQLException e) {
    // SQLException 처리 로직
} catch (Exception e) {
    // 그 외 Exception 처리 로직
}
```{% endraw %}

이런 방식으로 각기 다른 예외 유형에 대해 특화된 처리를 할 수 있으며, 코드가 더욱 명확하고 유지보수하기 쉬워집니다. {% raw %}`Throwable`{% endraw %}을 직접 잡는 것은 시스템 오류나 복구 불가능한 예외까지 포함할 수 있기 때문에, 일반적인 애플리케이션에서는 권장되지 않습니다.

## 사용자 입력:
> 아니..throwable말야.


## ChatGPT 응답:
> {% raw %}`Throwable`{% endraw %}을 사용하는 경우에도 예외 처리를 나누어 할 수 있습니다. {% raw %}`Throwable`{% endraw %}은 자바에서 모든 에러와 예외의 최상위 클래스이므로, {% raw %}`Throwable`{% endraw %}을 잡는 것은 모든 종류의 예외와 에러를 잡는 것을 의미합니다. 하지만 일반적인 애플리케이션에서는 {% raw %}`Throwable`{% endraw %}을 직접 잡는 것이 권장되지 않습니다. 그럼에도 불구하고 이를 사용해야 할 경우, 다음과 같이 여러 {% raw %}`catch`{% endraw %} 절을 사용하여 다양한 유형의 예외를 구분하여 처리할 수 있습니다:

{% raw %}```java
try {
    // 예외가 발생할 수 있는 코드
} catch (IOException e) {
    // IOException 처리
} catch (SQLException e) {
    // SQLException 처리
} catch (Exception e) {
    // 기타 Exception 처리
} catch (Throwable t) {
    // Throwable 처리 - 모든 예외와 에러를 포함
}
```{% endraw %}

이 방식에서는 {% raw %}`Throwable`{% endraw %} {% raw %}`catch`{% endraw %} 절이 마지막에 위치합니다. 이는 {% raw %}`Throwable`{% endraw %}이 {% raw %}`Exception`{% endraw %}과 {% raw %}`Error`{% endraw %} 클래스를 모두 포괄하기 때문입니다. {% raw %}`Throwable`{% endraw %}을 잡으면 시스템 오류({% raw %}`Error`{% endraw %} 클래스)까지 포착할 수 있으며, 이러한 오류는 일반적으로 애플리케이션 레벨에서 처리할 수 없는 심각한 문제를 나타내기 때문에 주의가 필요합니다.

따라서, {% raw %}`Throwable`{% endraw %}을 사용할 때는 매우 신중해야 하며, 대부분의 경우 특정 예외 클래스({% raw %}`IOException`{% endraw %}, {% raw %}`SQLException`{% endraw %}, {% raw %}`NullPointerException`{% endraw %} 등)를 사용하는 것이 더 안전하고 효과적입니다.

