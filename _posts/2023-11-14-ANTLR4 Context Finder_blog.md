---
layout: post
title: "ANTLR4 Context Finder"
date: 2023-11-14
tags: [Finder, Context]
categories: blog
---

## 사용자 입력:
> Antlr4로의 parseRuleContext를 찾는 유틸함수인데, 기능은 다음과 같아. rootContext하위에 존재하는 모든 targetContextClass를 리스트로 반환한다. 조건은 rootContext와 하위에 rootContext와 같은 이름의 룰이 나오기 전까지.

## ChatGPT 응답:
> ANTLR4에서 파싱 트리를 탐색하여 특정 컨텍스트 유형의 모든 인스턴스를 찾는 유틸리티 함수를 작성하는 것에 대한 요청을 이해했습니다. 이 함수는 주어진 `rootContext` 아래에 있는 모든 `targetContextClass` 인스턴스를 찾아 리스트로 반환할 것입니다. 여기서 주의해야 할 점은 `rootContext`와 동일한 이름의 룰이 나타날 때까지만 탐색한다는 것입니다.

다음은 ANTLR4를 사용하여 이 기능을 수행하는 Java 함수의 예시입니다:

```java
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;
import java.util.ArrayList;
import java.util.List;

public class ContextFinder {

    public static <T extends ParserRuleContext> List<T> findAllTargetContexts(
            ParserRuleContext rootContext, Class<T> targetContextClass) {
        List<T> foundContexts = new ArrayList<>();
        searchContexts(rootContext, targetContextClass, foundContexts, rootContext.getRuleIndex());
        return foundContexts;
    }

    private static <T extends ParserRuleContext> void searchContexts(
            ParseTree current, Class<T> targetContextClass, List<T> found, int rootRuleIndex) {
        if (current instanceof RuleNode) {
            RuleNode ruleNode = (RuleNode) current;
            if (ruleNode.getRuleContext().getRuleIndex() == rootRuleIndex && ruleNode != current) {
                // Stop searching when a context with the same rule index as rootContext is found
                return;
            }
            if (targetContextClass.isInstance(current)) {
                found.add(targetContextClass.cast(current));
            }
            for (int i = 0; i < ruleNode.getChildCount(); i++) {
                searchContexts(ruleNode.getChild(i), targetContextClass, found, rootRuleIndex);
            }
        }
    }
}
```

이 코드는 지정된 `rootContext` 아래의 파싱 트리를 재귀적으로 탐색하여 `targetContextClass` 타입의 모든 인스턴스를 찾습니다. 탐색은 `rootContext`와 동일한 룰 인덱스를 가진 다른 컨텍스트를 만나면 중단됩니다.

이 유틸리티 함수를 사용하려면 ANTLR4로 생성된 파서와 컨텍스트 클래스를 가지고 있어야 합니다. 함수를 호출할 때, `rootContext`와 찾고자 하는 타깃 컨텍스트 클래스를 인자로 전달하면 됩니다.

## 사용자 입력:
> 첫번째 매개변수를 Class<T>로 지정해도 되나?

## ChatGPT 응답:
> 첫 번째 매개변수를 `Class<T>`로 지정하는 것은 `ParserRuleContext`의 하위 타입이 아닌 완전히 다른 유형의 컨텍스트를 찾는 것과 관련이 있을 수 있습니다. 이 경우, 함수는 주어진 `rootContext` 아래에서 특정 클래스 타입의 인스턴스를 찾도록 설계될 수 있습니다. 그러나 ANTLR4의 파싱 트리에서는 일반적으로 `ParserRuleContext`의 하위 클래스 인스턴스를 찾는 것이 일반적입니다.

만약 `Class<T>`를 매개변수로 사용하고자 한다면, 이는 ANTLR4의 파싱 트리에서 특정 타입의 노드(예: `TerminalNodeImpl` 또는 다른 특정 노드 타입)를 찾고자 할 때 유용할 수 있습니다. 이 경우 함수의 구현은 약간 달라질 것이며, `ParserRuleContext`의 인스턴스를 찾는 대신 `Class<T>` 타입의 인스턴스를 찾는 방식으로 수정될 필요가 있습니다.

다음은 `Class<T>`를 첫 번째 매개변수로 사용하는 함수의 예시입니다:

```java
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;
import java.util.ArrayList;
import java.util.List;

public class ContextFinder {

    public static <T> List<T> findAllTargetContexts(
            Class<T> targetClass, ParserRuleContext rootContext) {
        List<T> foundContexts = new ArrayList<>();
        searchContexts(rootContext, targetClass, foundContexts, rootContext.getRuleIndex());
        return foundContexts;
    }

    private static <T> void searchContexts(
            ParseTree current, Class<T> targetClass, List<T> found, int rootRuleIndex) {
        if (current instanceof RuleNode) {
            RuleNode ruleNode = (RuleNode) current;
            if (ruleNode.getRuleContext().getRuleIndex() == rootRuleIndex && ruleNode != current) {
                // Stop searching when a context with the same rule index as rootContext is found
                return;
            }
            if (targetClass.isInstance(current)) {
                found.add(targetClass.cast(current));
            }
            for (int i = 0; i < ruleNode.getChildCount(); i++) {
                searchContexts(ruleNode.getChild(i), targetClass, found, rootRuleIndex);
            }
        }
    }
}
```

이 함수는 `targetClass` 타입의 모든 인스턴스를 찾기 위해 파싱 트리를 탐색합니다. 이러한 접근 방식은 더 범용적이며, `ParserRuleContext`의 하위 클래스뿐만 아니라 다른 타입의 노드도 처리할 수 있습니다.

## 사용자 입력:
> 내가 잘못 이해시켰다.... 첫번째 매개변수를 ParseRuleContext가 아니라 <T extend ParseRuleContext> 란 말이었는데.

## ChatGPT 응답:
> 알겠습니다. 첫 번째 매개변수를 `<T extends ParserRuleContext>`로 지정하는 것은 가능합니다. 이 경우 함수는 특정 `ParserRuleContext`의 하위 클래스 유형인 `T`를 매개변수로 받게 됩니다. 이러한 접근 방식은 함수가 특정 유형의 컨텍스트에 대해 더 유연하게 작동할 수 있게 해줍니다.

함수의 수정된 버전은 다음과 같이 될 것입니다:

```java
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;
import java.util.ArrayList;
import java.util.List;

public class ContextFinder {

    public static <T extends ParserRuleContext> List<T> findAllTargetContexts(
            T rootContext, Class<T> targetContextClass) {
        List<T> foundContexts = new ArrayList<>();
        searchContexts(rootContext, targetContextClass, foundContexts, rootContext.getRuleIndex());
        return foundContexts;
    }

    private static <T extends ParserRuleContext> void searchContexts(
            ParseTree current, Class<T> targetContextClass, List<T> found, int rootRuleIndex) {
        if (current instanceof RuleNode) {
            RuleNode ruleNode = (RuleNode) current;
            if (ruleNode.getRuleContext().getRuleIndex() == rootRuleIndex && ruleNode.getRuleContext() != current) {
                // Stop searching when a context with the same rule index as rootContext is found
                return;
            }
            if (targetContextClass.isInstance(current)) {
                found.add(targetContextClass.cast(current));
            }
            for (int i = 0; i < ruleNode.getChildCount(); i++) {
                searchContexts(ruleNode.getChild(i), targetContextClass, found, rootRuleIndex);
            }
        }
    }
}
```

이 버전에서는 `rootContext`를 `ParserRuleContext`의 하위 클래스의 인스턴스로 지정합니다. 이 방식은 특정 컨텍스트 유형에 대한 탐색을 더욱 특화시켜줍니다. 예를 들어, 특정 문법 규칙에 해당하는 컨텍스트 유형에 대한 탐색을 지정할 수 있습니다. 이 함수는 이제 해당 유형의 모든 인스턴스를 찾아 리스트로 반환할 것입니다.

## 사용자 입력:
> findAllTargetContexts 작명이 좀 아쉽다. root와 root 이름 사이 내용을 찾는 건데..

## ChatGPT 응답:
> 이해했습니다. 함수의 작업 내용을 고려하여 더 적절한 이름을 고려해보면, 이 함수는 주어진 `rootContext`와 같은 이름의 룰이 나타날 때까지 특정 컨텍스트 유형을 찾는 작업을 수행합니다. 따라서 함수 이름은 이 기능을 명확히 반영할 필요가 있습니다.

함수의 목적을 고려할 때, 다음과 같은 이름들이 적절할 수 있습니다:

1. `findContextsUntilDuplicateRule`: 이 이름은 주어진 `rootContext` 아래에서 동일한 이름의 룰이 나타날 때까지 컨텍스트를 찾는다는 것을 나타냅니다.

2. `searchContextsBeforeRuleRepeat`: 이 이름은 같은 룰이 반복되기 전까지 컨텍스트를 탐색한다는 의미를 내포합니다.

3. `gatherContextsUntilRuleReoccurrence`: 이 이름은 특정 룰이 다시 나타날 때까지 컨텍스트를 모은다는 개념을 전달합니다.

4. `collectContextsBeforeMatchingRule`: 이 이름은 일치하는 룰이 나타나기 전까지 컨텍스트를 수집한다는 개념을 강조합니다.

이러한 이름들은 함수가 수행하는 작업의 본질을 더 잘 반영하며, 코드를 읽는 사람에게 함수의 목적을 더 명확하게 전달할 수 있습니다.

## 사용자 입력:
> 접두어로 findAll이고 다음에 들어갈 말을 골라줘. 짧게.

## ChatGPT 응답:
> `findAll` 접두어를 사용하고 짧고 명확한 함수 이름을 위해, 다음과 같은 이름을 제안합니다:

1. `findAllUntilRepeat`
2. `findAllBeforeRecur`
3. `findAllUpToMatch`
4. `findAllPriorRule`

이러한 이름들은 함수가 주어진 `rootContext` 아래에서 특정 조건이 충족될 때까지 특정 컨텍스트 유형을 찾는다는 개념을 간결하게 전달합니다.

