---
layout: post
title: "Collections.singletonList 설명"
tags: [설명, singletonList, Collections]
---

## 사용자 입력:
> 자바의 Collections.singletonList 에 대해서 알려줘.

## ChatGPT 응답:
> {% raw %}`Collections.singletonList`{% endraw %}은 Java의 {% raw %}`java.util.Collections`{% endraw %} 유틸리티 클래스에 있는 정적 메소드입니다. 이 메소드는 단일 요소를 포함하는 불변(immutable) 리스트를 생성하고 반환합니다. 이 메소드를 사용하면 명시된 요소만 포함하는 최적화된 불변 리스트를 쉽게 만들 수 있습니다. 불변 리스트는 생성 후에 그 내용을 변경할 수 없기 때문에, 내부 요소를 추가하거나 제거할 수 없습니다.

{% raw %}`Collections.singletonList`{% endraw %} 메소드는 특히 단일 요소로 리스트를 초기화할 필요가 있을 때 유용합니다. 예를 들어, 메소드에 단일 요소를 인수로 전달하면 해당 요소만 포함하는 리스트를 반환합니다. 이 방법은 메모리 사용량을 최소화하고, 변경 불가능한 리스트를 통해 데이터의 불변성을 보장하는 데 도움이 됩니다.

### 사용 예시:

{% raw %}```java
import java.util.List;
import java.util.Collections;

public class SingletonListExample {
    public static void main(String[] args) {
        // 단일 요소를 포함하는 불변 리스트 생성
        List<String> singletonList = Collections.singletonList("Element");

        // 리스트 출력
        System.out.println(singletonList);

        // 리스트는 불변이므로, 요소 추가 시 UnsupportedOperationException 발생
        // singletonList.add("Another Element"); // 이 코드를 실행하면 예외 발생
    }
}
```{% endraw %}

이 예시에서 {% raw %}`Collections.singletonList("Element")`{% endraw %}는 "Element"라는 단일 문자열을 포함하는 불변 리스트를 생성합니다. 이 리스트에 다른 요소를 추가하려고 시도하면 {% raw %}`UnsupportedOperationException`{% endraw %}이 발생합니다. 이러한 특성 덕분에, {% raw %}`Collections.singletonList`{% endraw %}는 불변 데이터를 전달하거나 처리할 때 유용하게 사용됩니다.

