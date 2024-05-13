---
layout: post
title: "C++ ANTLR4 파서 함수"
date: 2023-11-09
tags: [C, 함수, 파서]
categories: blog
---

## 사용자 입력:
> Antlr4를 이용해서 C++파서를 만들고 있는데, 매개변수로 현재 노드의 ParserRuleContext 와 찾고 싶은 C++파서의 RuleContext를 지정하면,  말단의  찾고 싶은 C++파서의 RuleContext를 찾아오는 함수를 만들어줘. 제네릭을 이용하고, 반환내용에서 현재 노드는 제외해야지.

## ChatGPT 응답:
> ANTLR4를 사용하여 C++ 파서를 만들 때, 특정 타입의 `RuleContext`를 찾는 함수를 구현하려면, 제네릭을 활용하여 타입 안전성을 유지하면서 파서 트리를 재귀적으로 탐색하는 함수를 만들 수 있습니다. 다음은 Java로 작성된 ANTLR4의 방문자 패턴을 활용한 예시 코드입니다.

이 함수는 `findRuleContext`라고 하며, 현재 노드(`currentContext`)에서 시작하여 지정된 타입의 룰(`targetRuleClass`)을 찾을 때까지 파서 트리를 재귀적으로 탐색합니다. 발견된 경우, 해당 룰 컨텍스트를 반환하고, 그렇지 않으면 `null`을 반환합니다.

```java
public class CPPParserHelper {
    // findRuleContext 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> T findRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        // 현재 노드가 찾고자 하는 타입이면 반환합니다.
        if (targetRuleClass.isInstance(currentContext)) {
            return targetRuleClass.cast(currentContext);
        }
        // 현재 노드의 부모가 없으면 null을 반환합니다. (탐색 종료)
        if (currentContext.getParent() == null) {
            return null;
        }
        // 재귀적으로 부모 노드를 탐색합니다.
        return findRuleContext((ParserRuleContext)currentContext.getParent(), targetRuleClass);
    }
}
```

위 함수는 안전하게 타입을 확인하고 캐스팅하기 위해 제네릭을 사용합니다. 파라미터로 받은 `currentContext`에서 시작해서, `targetRuleClass` 타입의 인스턴스를 찾을 때까지 부모 노드를 거슬러 올라갑니다. 이 함수는 C++ 코드의 구문 분석 중에 호출되어 특정 유형의 노드를 찾는 데 사용될 수 있습니다.

## 사용자 입력:
> 단순히  특정 타입의 RuleContext를 찾는 함수를 구현이 아니라,  트리의 말단에 해당하는 특정 타입의 RuleContext를 찾는 거야.

## ChatGPT 응답:
> 트리의 말단에 해당하는 특정 타입의 `RuleContext`를 찾기 위한 함수를 만들려면, 주어진 노드의 하위 노드를 탐색하면서 해당 타입의 `RuleContext`가 자식이 없는 경우(즉, 말단 노드인 경우)를 찾아야 합니다. 아래는 이를 수행하는 재귀 함수의 예시 코드입니다.

이 함수는 `findDeepestRuleContext`라고 하며, 주어진 컨텍스트(`currentContext`) 내에서 시작하여 지정된 타입의 룰(`targetRuleClass`)이 말단 노드일 때까지 재귀적으로 탐색합니다. 발견된 경우, 해당 룰 컨텍스트를 반환합니다.

```java
public class CPPParserHelper {
    // findDeepestRuleContext 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> T findDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        // 결과를 저장할 변수를 null로 초기화합니다.
        T result = null;
        // 모든 자식들을 순회하며 말단 노드를 찾습니다.
        for (int i = 0; i < currentContext.getChildCount(); i++) {
            ParseTree child = currentContext.getChild(i);
            if (child instanceof ParserRuleContext) {
                ParserRuleContext childContext = (ParserRuleContext) child;
                // 자식 노드가 targetRuleClass의 인스턴스이고 자식이 없으면 결과를 갱신합니다.
                if (targetRuleClass.isInstance(childContext) && childContext.getChildCount() == 0) {
                    result = targetRuleClass.cast(childContext);
                } else {
                    // 재귀적으로 자식 노드들을 탐색합니다.
                    T childResult = findDeepestRuleContext(childContext, targetRuleClass);
                    if (childResult != null) {
                        result = childResult;
                    }
                }
            }
        }
        return result;
    }
}
```

위 코드는 지정된 타입의 `RuleContext` 중에서 자식이 없는 노드를 찾기 위해 현재 노드의 모든 자식을 재귀적으로 탐색합니다. 말단 노드가 발견되면 이를 결과 변수에 저장하고 최종적으로 반환합니다. 이 방법은 말단 노드에 도달할 때까지 트리를 깊이 우선 탐색(DFS) 방식으로 탐색합니다.

## 사용자 입력:
> 그런데, 찾고 싶은 RuleContext가 말단인데, 여러개 있을 수도 있잖아.

## ChatGPT 응답:
> 여러 말단 노드 중에서 특정 타입의 `RuleContext`를 찾기 위해서는, 해당 타입의 모든 인스턴스를 찾은 후에 각각이 말단인지 확인해야 합니다. 이를 위해 우리는 `findDeepestRuleContexts`라는 함수를 만들 것이며, 이 함수는 `targetRuleClass` 타입의 `RuleContext`들 중 말단 노드인 것들을 모두 찾아 리스트로 반환합니다. 다음은 그러한 함수의 예시입니다:

```java
public class CPPParserHelper {
    // findDeepestRuleContexts 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> List<T> findDeepestRuleContexts(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> result = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, result);
        return result;
    }

    // 실제 재귀 탐색을 수행하는 보조 함수입니다.
    private static <T extends ParserRuleContext> void findDeepestRuleContextsHelper(ParserRuleContext currentContext, Class<T> targetRuleClass, List<T> result) {
        // 모든 자식 노드를 순회합니다.
        for (int i = 0; i < currentContext.getChildCount(); i++) {
            ParseTree child = currentContext.getChild(i);
            if (child instanceof ParserRuleContext) {
                ParserRuleContext childContext = (ParserRuleContext) child;
                // 자식 노드가 targetRuleClass의 인스턴스이고 자식이 없으면 리스트에 추가합니다.
                if (targetRuleClass.isInstance(childContext) && childContext.getChildCount() == 0) {
                    result.add(targetRuleClass.cast(childContext));
                } else {
                    // 재귀적으로 자식 노드들을 탐색합니다.
                    findDeepestRuleContextsHelper(childContext, targetRuleClass, result);
                }
            }
        }
    }
}
```

위 함수는 `findDeepestRuleContextsHelper`를 호출하여 모든 자식 노드를 재귀적으로 탐색하고, `targetRuleClass` 타입의 말단 노드들을 리스트에 추가합니다. 이 리스트는 그 후 호출자에게 반환됩니다. 이렇게 함으로써, 말단 노드들 중에서 원하는 타입의 모든 인스턴스를 찾을 수 있습니다.

## 사용자 입력:
> 
여기에서 위에 함수에서 List<T>가 1이어서, List<T>가 아닌 T를 반환하는 함수도 추가해줘.

## ChatGPT 응답:
> 만약 단일 말단 `RuleContext` 객체를 반환하는 함수가 필요하다면, 우리는 위에서 제공된 `findDeepestRuleContexts` 함수를 기반으로 하여 첫 번째 말단 노드만 반환하는 함수를 만들 수 있습니다. 이 함수는 같은 타입의 여러 말단 노드가 존재할 수 있지만, 그 중 첫 번째로 발견된 것만을 반환할 것입니다.

다음은 단일 말단 `RuleContext` 객체를 반환하는 함수입니다:

```java
public class CPPParserHelper {
    // findFirstDeepestRuleContext 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> T findFirstDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> deepestRuleContexts = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, deepestRuleContexts);
        
        // 리스트가 비어있지 않다면 첫 번째 요소를 반환합니다.
        if (!deepestRuleContexts.isEmpty()) {
            return deepestRuleContexts.get(0);
        }
        // 해당 타입의 말단 노드가 없다면 null을 반환합니다.
        return null;
    }

    // findDeepestRuleContextsHelper는 위에서 정의한 대로입니다.
}
```

이 함수는 이전에 정의된 `findDeepestRuleContextsHelper` 함수를 사용하여 모든 말단 노드를 찾은 후, 그 중 첫 번째 요소를 반환합니다. 만약 해당 타입의 말단 노드가 없다면 `null`을 반환합니다. 이 함수는 첫 번째 발견된 말단 노드만이 필요할 때 유용합니다.

## 사용자 입력:
> 아니..First가 아니라, List 크기가 1인 것 찾는 거야.

## ChatGPT 응답:
> 아, 이해했습니다. 단일 말단 `RuleContext`를 찾되, 그런 유형의 말단 노드가 하나만 존재하는 경우에 한해 반환하는 함수를 만들어야 합니다. 만약 해당 유형의 말단 노드가 여러 개 있거나 전혀 없는 경우에는 `null`을 반환해야 합니다. 다음은 그런 함수의 예시입니다:

```java
public class CPPParserHelper {
    // findSingleDeepestRuleContext 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> T findSingleDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> deepestRuleContexts = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, deepestRuleContexts);

        // 리스트의 크기가 정확히 1이면 단일 말단 노드를 반환합니다.
        if (deepestRuleContexts.size() == 1) {
            return deepestRuleContexts.get(0);
        }
        // 말단 노드가 여러 개이거나 없는 경우 null을 반환합니다.
        return null;
    }

    // findDeepestRuleContextsHelper는 위에서 정의한 대로입니다.
}
```

이 함수는 모든 말단 노드를 찾은 후에 리스트의 크기가 정확히 1인지 확인합니다. 만약 그렇다면 그 단일 말단 노드를 반환하고, 그렇지 않으면 `null`을 반환합니다. 이 방식으로, 단일 말단 노드만을 찾을 수 있으며, 이는 리스트에 정확히 하나의 요소만 존재할 때 유용한 방법입니다.

## 사용자 입력:
> findDeepestRuleContextsHelper에서 currentContext가 null인게 발생해.

## ChatGPT 응답:
> `currentContext`가 `null`이 될 수 있는 상황은 일반적으로 파서 트리를 순회하다가 트리의 끝에 도달했을 때입니다. 재귀 함수에서 이러한 상황을 처리하려면 `null` 체크를 추가해야 합니다. 다음은 `null` 상황을 처리하는 `findDeepestRuleContextsHelper` 함수의 개선된 버전입니다:

```java
public class CPPParserHelper {
    // findSingleDeepestRuleContext 함수는 제네릭 타입 T로 RuleContext의 서브클래스를 받습니다.
    public static <T extends ParserRuleContext> T findSingleDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        if (currentContext == null) {
            return null;
        }

        List<T> deepestRuleContexts = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, deepestRuleContexts);

        // 리스트의 크기가 정확히 1이면 단일 말단 노드를 반환합니다.
        if (deepestRuleContexts.size() == 1) {
            return deepestRuleContexts.get(0);
        }
        // 말단 노드가 여러 개이거나 없는 경우 null을 반환합니다.
        return null;
    }

    // 실제 재귀 탐색을 수행하는 보조 함수입니다.
    private static <T extends ParserRuleContext> void findDeepestRuleContextsHelper(ParserRuleContext currentContext, Class<T> targetRuleClass, List<T> result) {
        // 현재 컨텍스트가 null인 경우는 더 이상 탐색하지 않습니다.
        if (currentContext == null) {
            return;
        }
        
        // 모든 자식 노드를 순회합니다.
        for (int i = 0; i < currentContext.getChildCount(); i++) {
            ParseTree child = currentContext.getChild(i);
            if (child instanceof ParserRuleContext) {
                ParserRuleContext childContext = (ParserRuleContext) child;
                // 자식 노드가 targetRuleClass의 인스턴스이고 자식이 없으면 리스트에 추가합니다.
                if (targetRuleClass.isInstance(childContext) && childContext.getChildCount() == 0) {
                    result.add(targetRuleClass.cast(childContext));
                } else {
                    // 재귀적으로 자식 노드들을 탐색합니다.
                    findDeepestRuleContextsHelper(childContext, targetRuleClass, result);
                }
            }
        }
    }
}
```

위 코드에서 `findSingleDeepestRuleContext` 함수는 `currentContext`가 `null`인지 먼저 확인하고, 그럴 경우 더 이상 진행하지 않고 `null`을 반환합니다. `findDeepestRuleContextsHelper` 함수 역시 `currentContext`가 `null`일 때는 재귀 호출을 중단하도록 변경되었습니다. 이렇게 함으로써, `null` 포인터 예외를 방지할 수 있습니다.

## 사용자 입력:
> 네가 제시한 함수. 리스트 무조건 0나오는데.

## ChatGPT 응답:
> 제공된 코드가 정상적으로 작동하지 않는다면, 문제를 해결하기 위해 다음과 같은 점검 사항을 고려해야 합니다:

1. **재귀 로직 확인**: 말단 노드를 찾는 재귀 로직이 올바르게 구현되어 있는지 확인해야 합니다.
2. **말단 노드의 정의**: 말단 노드가 자식이 없는 노드인지, 또는 특정 조건을 만족해야 하는 노드인지 명확히 해야 합니다.
3. **타입 체크**: `targetRuleClass.isInstance(childContext)`가 실제로 원하는 타입의 인스턴스인지 확인해야 합니다.

아래 코드는 이러한 점검 사항을 고려하여 수정된 버전입니다. 여기서는 말단 노드를 찾는 로직을 다시 작성하여, 자식 노드가 없는 경우에만 해당 노드를 결과 리스트에 추가합니다.

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;

import java.util.ArrayList;
import java.util.List;

public class CPPParserHelper {

    public static <T extends ParserRuleContext> T findSingleDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> deepestRuleContexts = findDeepestRuleContexts(currentContext, targetRuleClass);
        if (deepestRuleContexts.size() == 1) {
            return deepestRuleContexts.get(0);
        }
        return null;
    }

    public static <T extends ParserRuleContext> List<T> findDeepestRuleContexts(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> result = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, result);
        return result;
    }

    private static <T extends ParserRuleContext> void findDeepestRuleContextsHelper(ParserRuleContext currentContext, Class<T> targetRuleClass, List<T> result) {
        if (currentContext == null) {
            return;
        }
        // Check if currentContext is an instance of targetRuleClass and has no children.
        if (targetRuleClass.isInstance(currentContext) && currentContext.getChildCount() == 0) {
            result.add(targetRuleClass.cast(currentContext));
        } else {
            // Otherwise, recurse into each child that is also a ParserRuleContext.
            for (int i = 0; i < currentContext.getChildCount(); i++) {
                ParseTree child = currentContext.getChild(i);
                if (child instanceof ParserRuleContext) {
                    findDeepestRuleContextsHelper((ParserRuleContext) child, targetRuleClass, result);
                }
            }
        }
    }
}
```

이 코드는 말단 노드를 찾는 보조 함수에서 재귀적으로 자식 노드를 탐색하며, 타겟 클래스의 인스턴스인지 확인하고 자식이 없는 노드를 리스트에 추가합니다. `findSingleDeepestRuleContext` 함수는 이 리스트를 검사하여 단 하나의 요소만 포함하고 있는지 확인하고, 그렇다면 그 요소를 반환합니다.

여전히 문제가 해결되지 않는 경우, ANTLR4의 실제 구문 분석 트리와 파서 규칙을 검토하여 파서의 구조가 올바른지 확인해야 합니다. 또한, 테스트 중인 특정 입력에 대해 예상되는 결과가 실제로 말단 노드에 해당하는지도 확인해야 합니다.

## 사용자 입력:
> 똑같이 리스트가 0이야.

## ChatGPT 응답:
> 제공된 코드가 리스트의 크기가 0으로 나오는 문제가 발생한다면, 다음 두 가지를 확인해야 합니다:

1. **입력**: `currentContext`가 올바른 `ParserRuleContext` 객체를 가리키고 있는지 확인하세요. 이 객체는 파싱 트리의 시작 노드를 나타내야 합니다.
2. **타겟 타입**: `targetRuleClass`가 올바른 `RuleContext` 타입을 나타내는지 확인하세요. 이 클래스는 ANTLR에 의해 생성된 파서 클래스 내에 존재해야 합니다.

만약 여전히 문제가 해결되지 않는다면, 문제가 발생하는 상황을 재현할 수 있는 파싱 트리의 구조와 구체적인 예시를 제공해주시면, 더 정확한 해결책을 제공할 수 있을 것입니다.

한편, 코드 로직 자체에는 문제가 없어 보이지만, 실제로 말단 노드를 찾는 과정에서 예상과 다른 상황이 발생하고 있다는 점을 고려해야 합니다. 예를 들어, `targetRuleClass`의 인스턴스가 파싱 트리의 어느 노드에도 존재하지 않거나, 말단 노드가 아니라서 리스트에 추가되지 않을 수 있습니다.

또한, ANTLR이 생성하는 파싱 트리는 때때로 예상치 못한 노드를 포함할 수 있으며, 이는 특히 사용자가 정의한 파싱 규칙이 의도한 대로 작동하지 않을 때 발생할 수 있습니다. 따라서, 실제 파싱 트리를 시각적으로 확인하거나 디버깅을 통해 각 단계에서 무슨 일이 일어나고 있는지 자세히 조사해야 할 수 있습니다. 

문제 해결을 위해 다음과 같은 추가적인 디버깅 단계를 시도할 수 있습니다:

- **트리 시각화**: ANTLR로 생성된 파싱 트리를 시각화하여 실제로 어떤 노드들이 있는지 확인합니다.
- **디버그 출력**: `findDeepestRuleContextsHelper` 함수 내부에 로그 스테이트먼트를 추가하여, 각 재귀 호출 시점에서 `currentContext`의 상태와 자식 노드의 수를 출력합니다.
- **단위 테스트**: 특정한 작은 예시 입력을 사용하여 함수를 단위 테스트하고, 예상된 결과가 나오는지 확인합니다.

이러한 접근 방법을 통해 문제의 원인을 좁혀나갈 수 있습니다.

## 사용자 입력:
> 디버그 출력 추가해봐.

## ChatGPT 응답:
> 디버그 출력을 추가하기 위해서는 함수 내부에 특정 체크 포인트에서 현재 상태를 출력하는 명령을 추가해야 합니다. 이를 통해 함수가 어떤 노드를 방문하고 있는지, 그리고 각 노드의 자식 수는 얼마인지 등의 정보를 로깅할 수 있습니다.

다음은 `findDeepestRuleContextsHelper` 함수에 디버그 출력을 추가한 예시입니다:

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;

import java.util.ArrayList;
import java.util.List;

public class CPPParserHelper {

    // 디버그 메시지를 출력할 수 있도록 로깅 메소드를 추가합니다.
    private static void log(String message) {
        System.out.println(message); // 콘솔에 메시지를 출력합니다.
    }

    public static <T extends ParserRuleContext> T findSingleDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        if (currentContext == null) {
            log("The starting context is null.");
            return null;
        }

        List<T> deepestRuleContexts = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, deepestRuleContexts, 0); // 깊이 파라미터를 추가하여 각 노드의 깊이를 추적합니다.

        if (deepestRuleContexts.size() == 1) {
            return deepestRuleContexts.get(0);
        } else {
            log("Expected a single deepest rule context, but found: " + deepestRuleContexts.size());
            return null;
        }
    }

    private static <T extends ParserRuleContext> void findDeepestRuleContextsHelper(ParserRuleContext currentContext, Class<T> targetRuleClass, List<T> result, int depth) {
        // 현재 컨텍스트와 깊이 정보를 로깅합니다.
        log("Visiting node at depth " + depth + ": " + currentContext.getClass().getSimpleName());

        if (currentContext == null) {
            return;
        }
        
        // 현재 컨텍스트가 타겟 클래스의 인스턴스인지 확인하고 자식이 없으면 리스트에 추가합니다.
        if (targetRuleClass.isInstance(currentContext) && currentContext.getChildCount() == 0) {
            result.add(targetRuleClass.cast(currentContext));
            log("Added a deepest rule context of type " + currentContext.getClass().getSimpleName() + " at depth " + depth);
        } else {
            // 자식 노드들에 대해 재귀 호출을 수행합니다.
            for (int i = 0; i < currentContext.getChildCount(); i++) {
                ParseTree child = currentContext.getChild(i);
                if (child instanceof ParserRuleContext) {
                    findDeepestRuleContextsHelper((ParserRuleContext) child, targetRuleClass, result, depth + 1); // 깊이를 1 증가시키면서 재귀 호출합니다.
                }
            }
        }
    }
}
```

위 코드에서 `log` 메소드는 각 단계에서 콘솔에 메시지를 출력합니다. `findDeepestRuleContextsHelper` 함수는 이제 깊이 매개변수를 포함하여, 각 노드의 깊이를 추적하고 이를 로깅합니다. 이를 통해 어떤 노드들이 탐색되었는지, 어떤 노드들이 결과 리스트에 추가되었는지 등을 확인할 수 있습니다.

실제 개발 환경에서는 `System.out.println` 대신 적절한 로깅 프레임워크를 사용하는 것이 좋습니다.

## 사용자 입력:
>         // Check if currentContext is an instance of targetRuleClass and has no children.
        if (targetRuleClass.isInstance(currentContext) && currentContext.getChildCount() == 0) {
            result.add(targetRuleClass.cast(currentContext));
        } 이부분에서 단순히  currentContext.getChildCount() == 0 이 아니라, 하위에 targetRuleClass.isInstance(currentContext)가 없을 때 추가하는 거지.

## ChatGPT 응답:
> 알겠습니다. 해당 로직을 수정하여, `currentContext`가 `targetRuleClass`의 인스턴스이면서, 하위에 `targetRuleClass` 타입의 노드가 더 이상 존재하지 않을 때만 결과 리스트에 추가하는 코드로 변경하겠습니다.

이를 위해서는 재귀 함수를 통해 `currentContext`의 모든 자식 노드를 확인하고, 같은 타입의 자식 노드가 없을 때만 `currentContext`를 결과에 추가해야 합니다. 아래 코드는 그러한 로직을 구현한 것입니다:

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;

import java.util.ArrayList;
import java.util.List;

public class CPPParserHelper {

    public static <T extends ParserRuleContext> T findSingleDeepestRuleContext(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> deepestRuleContexts = findDeepestRuleContexts(currentContext, targetRuleClass);
        if (deepestRuleContexts.size() == 1) {
            return deepestRuleContexts.get(0);
        }
        return null;
    }

    public static <T extends ParserRuleContext> List<T> findDeepestRuleContexts(ParserRuleContext currentContext, Class<T> targetRuleClass) {
        List<T> result = new ArrayList<>();
        findDeepestRuleContextsHelper(currentContext, targetRuleClass, result);
        return result;
    }

    private static <T extends ParserRuleContext> void findDeepestRuleContextsHelper(ParserRuleContext currentContext, Class<T> targetRuleClass, List<T> result) {
        if (currentContext == null) {
            return;
        }
        
        // Check if currentContext is an instance of targetRuleClass
        if (targetRuleClass.isInstance(currentContext)) {
            // Check if there are no children of type targetRuleClass
            boolean hasTargetRuleClassChild = false;
            for (int i = 0; i < currentContext.getChildCount(); i++) {
                ParseTree child = currentContext.getChild(i);
                if (targetRuleClass.isInstance(child)) {
                    hasTargetRuleClassChild = true;
                    break;
                }
            }
            
            // If there are no children of type targetRuleClass, then this is the deepest node of that type.
            if (!hasTargetRuleClassChild) {
                result.add(targetRuleClass.cast(currentContext));
            }
        }
        
        // Recurse into each child that is also a ParserRuleContext
        for (int i = 0; i < currentContext.getChildCount(); i++) {
            ParseTree child = currentContext.getChild(i);
            if (child instanceof ParserRuleContext) {
                findDeepestRuleContextsHelper((ParserRuleContext) child, targetRuleClass, result);
            }
        }
    }
}
```

이제 `findDeepestRuleContextsHelper` 함수는 `currentContext`가 `targetRuleClass`의 인스턴스일 때, 그 자식 노드 중 `targetRuleClass`의 인스턴스가 있는지 확인합니다. 만약 자식 노드 중에 해당 타입의 인스턴스가 없다면, `currentContext`는 가장 깊은 노드로 간주되어 결과 리스트에 추가됩니다. 이 로직은 `currentContext`의 모든 자식 노드를 재귀적으로 탐색하며, 동일 타입의 더 깊은 노드가 존재하지 않을 때만 해당 노드를 말단 노드로 간주합니다.

