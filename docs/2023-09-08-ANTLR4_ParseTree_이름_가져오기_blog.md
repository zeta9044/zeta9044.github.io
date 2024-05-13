---
layout: post
title: "ANTLR4 ParseTree 이름 가져오기"
date: 2023-09-08
tags: [이름, ParseTree]
categories: blog
---

## 사용자 입력:
> ANTLR4의  ParseTree에서 현재 ParseTree 이름을 가져오려면?

## ChatGPT 응답:
> ANTLR4에서 ParseTree 객체는 문법과 입력 텍스트에 따라 생성되는 구문 분석 트리의 노드를 나타냅니다. ParseTree 인터페이스에는 여러 구현이 있습니다. 예를 들면 `TerminalNodeImpl`, `RuleContext` 등이 있습니다. 

구문 분석 트리에서 현재 노드의 이름 (또는 연관된 토큰 또는 규칙의 이름)을 가져오려면 노드의 종류에 따라 약간 다른 방법을 사용해야 할 수 있습니다.

1. **TerminalNode**: 이것은 리프 노드입니다. 입력 텍스트에서의 개별 토큰을 나타냅니다.
```python
if isinstance(node, TerminalNode):
    token = node.getSymbol()
    tokenName = token.getText()
```

2. **RuleContext**: 이것은 내부 노드입니다. 문법 규칙을 나타냅니다.
```python
if isinstance(node, RuleContext):
    ruleIndex = node.getRuleIndex()
    ruleName = parser.ruleNames[ruleIndex]  # parser는 현재의 Parser 객체입니다.
```

위의 코드 조각은 파이썬을 사용하여 ANTLR4와 함께 작성된 코드를 예로 들고 있습니다. 사용하는 언어와 상황에 따라 약간의 조정이 필요할 수 있습니다.

이렇게 하면 ParseTree의 현재 노드에서 이름을 가져올 수 있습니다.

## 사용자 입력:
> 함수로 만들어 줄래. 자바용이야.

## ChatGPT 응답:
> 물론이죠! ANTLR4를 사용하여 Java에서 ParseTree의 현재 노드의 이름을 가져오는 함수를 작성하겠습니다.

먼저, 필요한 ANTLR 라이브러리를 import 해야 합니다:

```java
import org.antlr.v4.runtime.tree.TerminalNode;
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.Parser;
```

그리고 ParseTree의 현재 노드의 이름을 가져오는 함수를 다음과 같이 작성할 수 있습니다:

```java
public static String getNodeName(Object node, Parser parser) {
    if (node instanceof TerminalNode) {
        TerminalNode terminalNode = (TerminalNode) node;
        return terminalNode.getSymbol().getText();
    } else if (node instanceof ParserRuleContext) {
        ParserRuleContext ruleContext = (ParserRuleContext) node;
        int ruleIndex = ruleContext.getRuleIndex();
        return parser.getRuleNames()[ruleIndex];
    } else {
        return "Unknown node";
    }
}
```

이 함수는 주어진 ParseTree 노드의 이름을 반환합니다. `parser` 매개변수는 현재의 Parser 객체를 나타내며, 이 객체는 규칙 이름 배열 (`getRuleNames()`)을 제공합니다.

이 함수를 사용하여 ParseTree에서 노드의 이름을 쉽게 가져올 수 있습니다.

## 사용자 입력:
> 위 매개변수에서 Parser parser 대신 ANTLR4로 생성된 파서 클래스를 받으면 안돼?

## ChatGPT 응답:
> 물론입니다. ANTLR4로 생성된 구체적인 파서 클래스를 사용하면 해당 파서에 특정한 규칙 이름을 직접 접근할 수 있습니다. 그러나 이렇게 하면 함수가 그 파서 클래스에만 국한되게 됩니다.

예를 들어, ANTLR4로 생성된 `MyLanguageParser`라는 파서 클래스가 있다고 가정하겠습니다. 그러면 함수를 다음과 같이 수정할 수 있습니다:

```java
import org.antlr.v4.runtime.tree.TerminalNode;
import org.antlr.v4.runtime.ParserRuleContext;

public static String getNodeName(Object node, MyLanguageParser parser) {
    if (node instanceof TerminalNode) {
        TerminalNode terminalNode = (TerminalNode) node;
        return terminalNode.getSymbol().getText();
    } else if (node instanceof ParserRuleContext) {
        ParserRuleContext ruleContext = (ParserRuleContext) node;
        int ruleIndex = ruleContext.getRuleIndex();
        return parser.getRuleNames()[ruleIndex];
    } else {
        return "Unknown node";
    }
}
```

이렇게 하면 `MyLanguageParser` 클래스의 `getRuleNames()` 메서드를 사용하여 규칙 이름을 가져올 수 있습니다. 그러나 이 방식의 단점은 이 함수가 `MyLanguageParser` 클래스에만 국한되므로 다른 파서에서는 사용할 수 없다는 것입니다.

## 사용자 입력:
> CPP14Pasrer.class 이렇게 받으면 안돼?

## ChatGPT 응답:
> 네, 클래스 객체를 매개변수로 받는 것도 가능합니다. Java의 `Class` 객체를 사용하면 클래스의 메타데이터에 접근할 수 있습니다. 그러나 ANTLR4에서 규칙 이름 배열은 인스턴스 메서드로 제공되므로, 클래스 레벨에서 직접 접근할 수는 없습니다. 그러나 파서 클래스의 인스턴스를 임시로 생성하여 해당 메서드에 접근하는 방법을 사용할 수 있습니다.

다음은 `CPP14Parser.class`를 받아 해당 파서의 규칙 이름을 가져오는 예제입니다:

```java
import org.antlr.v4.runtime.tree.TerminalNode;
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.Parser;

public static String getNodeName(Object node, Class<? extends Parser> parserClass) {
    if (node instanceof TerminalNode) {
        TerminalNode terminalNode = (TerminalNode) node;
        return terminalNode.getSymbol().getText();
    } else if (node instanceof ParserRuleContext) {
        ParserRuleContext ruleContext = (ParserRuleContext) node;
        int ruleIndex = ruleContext.getRuleIndex();
        try {
            Parser parser = parserClass.getDeclaredConstructor().newInstance();
            return parser.getRuleNames()[ruleIndex];
        } catch (Exception e) {
            e.printStackTrace();
            return "Error fetching rule name";
        }
    } else {
        return "Unknown node";
    }
}
```

이 함수에서는 `CPP14Parser.class`와 같은 클래스 객체를 받아 해당 클래스의 인스턴스를 생성하고, 이를 사용하여 규칙 이름을 가져옵니다.

그러나 이 방식에는 몇 가지 주의 사항이 있습니다:

1. 이 방식은 비효율적일 수 있습니다. 매번 규칙 이름을 가져올 때마다 새로운 파서 인스턴스를 생성하기 때문입니다.
2. 파서 클래스가 기본 생성자 (인자 없는 생성자)를 가지고 있어야 합니다. 그렇지 않으면 인스턴스 생성에 실패할 것입니다.

이러한 제한 사항을 고려하여 사용해야 합니다.

## 사용자 입력:
> 클래스 대신, rulenames 를 가진 String[]을 넘기면?

## ChatGPT 응답:
> 네, `ruleNames` 배열을 직접 전달하는 것이 훨씬 간단하고 효율적입니다. 이렇게 하면 파서 인스턴스를 생성할 필요 없이 직접 규칙 이름에 액세스할 수 있습니다.

다음은 `ruleNames` 배열을 사용하여 노드 이름을 가져오는 함수입니다:

```java
import org.antlr.v4.runtime.tree.TerminalNode;
import org.antlr.v4.runtime.ParserRuleContext;

public static String getNodeName(Object node, String[] ruleNames) {
    if (node instanceof TerminalNode) {
        TerminalNode terminalNode = (TerminalNode) node;
        return terminalNode.getSymbol().getText();
    } else if (node instanceof ParserRuleContext) {
        ParserRuleContext ruleContext = (ParserRuleContext) node;
        int ruleIndex = ruleContext.getRuleIndex();
        return ruleNames[ruleIndex];
    } else {
        return "Unknown node";
    }
}
```

이 함수를 사용할 때 ANTLR4로 생성된 파서의 `ruleNames` 배열을 직접 전달하면 됩니다. 예를 들면, `CPP14Parser`의 경우 다음과 같이 호출할 수 있습니다:

```java
String nodeName = getNodeName(someNode, CPP14Parser.ruleNames);
```

이렇게 하면 함수가 훨씬 더 간결하고 효율적으로 작동합니다.

