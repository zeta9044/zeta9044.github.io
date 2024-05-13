---
layout: post
title: "ANTLR4 Listener/Visitor 유틸리티"
date: 2023-09-11
tags: [Listener, Visitor, 유틸리티]
categories: blog
---

## 사용자 입력:
> ANTLR4의  Listener/visitor를 사용할 때, RULE traverse Utill 만들어줄래. 자바로.

## ChatGPT 응답:
> 물론이죠! ANTLR4에서 Listener와 Visitor 패턴은 파싱된 구문 트리를 순회하면서 사용자 정의 동작을 수행하는 데 유용합니다. 

먼저, ANTLR4의 Listener 및 Visitor 패턴에 대한 간략한 개요를 제공하겠습니다:

1. **Listener**: ANTLR4가 자동으로 생성하는 Listener는 모든 규칙에 대한 `enter`와 `exit` 메서드를 제공합니다. 사용자는 이 메서드들을 오버라이드하여 원하는 동작을 구현할 수 있습니다.
2. **Visitor**: Visitor는 `visit` 메서드를 제공하며, 각 규칙에 대한 별도의 `visit` 메서드가 있습니다. 사용자는 특정 규칙을 방문할 때마다 해당 규칙에 대한 `visit` 메서드를 오버라이드하여 원하는 동작을 구현할 수 있습니다.

이제 간단한 예제를 통해 Listener와 Visitor 유틸리티를 만들어 보겠습니다.

### 예제

다음은 간단한 ANTLR4 문법 예제입니다:
```
grammar Simple;

start: expr EOF;

expr: INT
    | expr '+' expr
    ;

INT: [0-9]+;
WS: [ \t\r\n]+ -> skip;
```

1. **Listener 유틸리티**
```java
public class SimpleListener extends SimpleBaseListener {
    @Override
    public void enterExpr(SimpleParser.ExprContext ctx) {
        // Custom logic when entering expr rule
        System.out.println("Entering expr: " + ctx.getText());
    }

    @Override
    public void exitExpr(SimpleParser.ExprContext ctx) {
        // Custom logic when exiting expr rule
        System.out.println("Exiting expr: " + ctx.getText());
    }
}
```

2. **Visitor 유틸리티**
```java
public class SimpleVisitor extends SimpleBaseVisitor<Void> {
    @Override
    public Void visitExpr(SimpleParser.ExprContext ctx) {
        // Custom logic when visiting expr rule
        System.out.println("Visiting expr: " + ctx.getText());
        return super.visitExpr(ctx);
    }
}
```

### 사용 방법

1. Listener 사용:
```java
SimpleListener listener = new SimpleListener();
ParseTreeWalker.DEFAULT.walk(listener, parseTree);
```

2. Visitor 사용:
```java
SimpleVisitor visitor = new SimpleVisitor();
visitor.visit(parseTree);
```

이러한 방식으로 Listener와 Visitor를 사용하여 ANTLR4에서 파싱된 구문 트리를 순회하며 사용자 정의 동작을 수행할 수 있습니다.

## 사용자 입력:
> 위는 이미 아는 거고, Listener와 Visitor의 context를 traverse할 수 있는 유틸리티를 만들어 달라는 거야.

## ChatGPT 응답:
> 알겠습니다. Listener와 Visitor를 통해 ANTLR4 구문 트리의 모든 context를 순회하며 접근할 수 있는 유틸리티를 만들겠습니다.

### 1. Listener를 이용한 Traverse Utility

이 유틸리티는 모든 노드를 방문할 때마다 해당 노드의 context 정보를 출력합니다.

```java
public class TraverseListener extends SimpleBaseListener {
    @Override
    public void enterEveryRule(ParserRuleContext ctx) {
        System.out.println("Entering rule: " + ctx.getText());
    }

    @Override
    public void exitEveryRule(ParserRuleContext ctx) {
        System.out.println("Exiting rule: " + ctx.getText());
    }
}
```

### 2. Visitor를 이용한 Traverse Utility

이 유틸리티는 모든 노드를 방문할 때마다 해당 노드의 context 정보를 출력합니다.

```java
public class TraverseVisitor extends SimpleBaseVisitor<Void> {
    @Override
    public Void visitChildren(RuleNode node) {
        System.out.println("Visiting node: " + node.getText());
        return super.visitChildren(node);
    }
}
```

### 사용 방법

1. Listener를 사용하여 traverse:
```java
TraverseListener listener = new TraverseListener();
ParseTreeWalker.DEFAULT.walk(listener, parseTree);
```

2. Visitor를 사용하여 traverse:
```java
TraverseVisitor visitor = new TraverseVisitor();
visitor.visit(parseTree);
```

위의 유틸리티를 사용하면 ANTLR4 구문 트리의 모든 노드를 순회하며 해당 노드의 context 정보를 출력할 수 있습니다.

## 사용자 입력:
> Listener의 이벤트 함수의 매개변수인 Context를 traverse해서 해당 이벤트  함수에서 다른 노드를 Traverse하고 이름 또는 토큰값, 트리순서등을 가져오는 유틸리티 말야.

## ChatGPT 응답:
> 아, 이해했습니다. 원하시는 것은 ANTLR4의 구문 트리의 특정 노드에서 시작하여 그 노드의 하위 노드들을 순회하며 다양한 정보 (이름, 토큰 값, 트리 순서 등)를 가져오는 유틸리티를 만드는 것입니다.

다음은 해당 유틸리티의 예시입니다:

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;

import java.util.ArrayList;
import java.util.List;

public class ContextTraverser {

    public static List<String> getTokens(ParserRuleContext context) {
        List<String> tokens = new ArrayList<>();
        traverse(context, tokens::add);
        return tokens;
    }

    public static List<String> getRuleNames(ParserRuleContext context, SimpleParser parser) {
        List<String> ruleNames = new ArrayList<>();
        traverse(context, node -> {
            if (node instanceof ParserRuleContext) {
                ruleNames.add(parser.getRuleNames()[((ParserRuleContext) node).getRuleIndex()]);
            }
        });
        return ruleNames;
    }

    private static void traverse(ParseTree node, java.util.function.Consumer<ParseTree> action) {
        action.accept(node);
        for (int i = 0; i < node.getChildCount(); i++) {
            traverse(node.getChild(i), action);
        }
    }
}
```

이 유틸리티는 ANTLR4의 `ParserRuleContext`를 받아 해당 노드와 하위 노드들의 토큰 값과 규칙 이름을 가져옵니다. 

### 사용 방법:

예를 들어, `expr` 규칙의 `ParserRuleContext`를 가지고 있다고 가정하면:

```java
ParserRuleContext exprContext = ...;
List<String> tokens = ContextTraverser.getTokens(exprContext);
System.out.println("Tokens: " + tokens);

List<String> ruleNames = ContextTraverser.getRuleNames(exprContext, parser); // parser는 SimpleParser의 인스턴스
System.out.println("Rule Names: " + ruleNames);
```

이 유틸리티를 사용하면 ANTLR4 구문 트리의 특정 노드에서 시작하여 그 노드와 하위 노드들의 토큰 값과 규칙 이름을 쉽게 가져올 수 있습니다.

## 사용자 입력:
> tokens::add가 뭔지 모르겠어.

## ChatGPT 응답:
> 죄송합니다. Java의 람다 표현식과 메서드 참조를 사용하여 설명했는데, 좀 더 간단하게 설명하겠습니다.

Java 8부터는 람다 표현식이 도입되어 함수적 인터페이스를 람다 표현식으로 표현할 수 있게 되었습니다. 이와 관련하여 메서드 참조는 특정 메서드를 직접 참조할 수 있는 기능입니다.

예를 들어, `tokens::add`는 `tokens` 리스트의 `add` 메서드를 참조합니다. 이는 다음 람다 표현식과 동일한 의미입니다:

```java
token -> tokens.add(token)
```

하지만, 이해하기 쉽게 코드를 다시 작성하겠습니다. `tokens::add` 대신에 람다 표현식을 사용하는 방식으로 변경하겠습니다:

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;

import java.util.ArrayList;
import java.util.List;

public class ContextTraverser {

    public static List<String> getTokens(ParserRuleContext context) {
        List<String> tokens = new ArrayList<>();
        traverse(context, token -> tokens.add(token.getText()));
        return tokens;
    }

    public static List<String> getRuleNames(ParserRuleContext context, SimpleParser parser) {
        List<String> ruleNames = new ArrayList<>();
        traverse(context, node -> {
            if (node instanceof ParserRuleContext) {
                ruleNames.add(parser.getRuleNames()[((ParserRuleContext) node).getRuleIndex()]);
            }
        });
        return ruleNames;
    }

    private static void traverse(ParseTree node, java.util.function.Consumer<ParseTree> action) {
        action.accept(node);
        for (int i = 0; i < node.getChildCount(); i++) {
            traverse(node.getChild(i), action);
        }
    }
}
```

이제 `tokens::add` 대신 람다 표현식 `token -> tokens.add(token.getText())`를 사용하였습니다. 이로써 코드의 가독성이 향상되었을 것입니다.

## 사용자 입력:
> 위에  ParserRuleContext를 ParseTree로 바꿔야지 될 것 같지 않아?

## ChatGPT 응답:
> 네, 맞습니다. `ParseTree`는 ANTLR4에서 구문 트리의 모든 노드에 대한 상위 인터페이스이므로, `ParseTree`를 사용하면 더 범용적인 유틸리티를 얻을 수 있습니다.

이를 반영하여 코드를 수정하겠습니다:

```java
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;

import java.util.ArrayList;
import java.util.List;

public class ContextTraverser {

    public static List<String> getTokens(ParseTree context) {
        List<String> tokens = new ArrayList<>();
        traverse(context, node -> tokens.add(node.getText()));
        return tokens;
    }

    public static List<String> getRuleNames(ParseTree context, SimpleParser parser) {
        List<String> ruleNames = new ArrayList<>();
        traverse(context, node -> {
            if (node instanceof RuleNode) {
                ruleNames.add(parser.getRuleNames()[((RuleNode) node).getRuleContext().getRuleIndex()]);
            }
        });
        return ruleNames;
    }

    private static void traverse(ParseTree node, java.util.function.Consumer<ParseTree> action) {
        action.accept(node);
        for (int i = 0; i < node.getChildCount(); i++) {
            traverse(node.getChild(i), action);
        }
    }
}
```

위의 수정된 코드에서는 `ParserRuleContext` 대신 `ParseTree`를 사용하여 모든 노드 타입을 처리할 수 있도록 했습니다.

## 사용자 입력:
> SimpleParser parser 매개변수에, parser의 인스턴스가 들어와야 되는거 아녀?

## ChatGPT 응답:
> 네, 맞습니다. `SimpleParser`의 인스턴스를 통해 규칙 이름을 가져오려면 해당 인스턴스가 필요합니다. 하지만 `ParseTree`나 `ParserRuleContext`를 통해 해당 노드가 속한 파서에 대한 참조를 얻을 수 있기 때문에 이를 이용하면 `SimpleParser`를 파라미터로 전달할 필요가 없습니다.

이를 반영하여 코드를 수정하겠습니다:

```java
import org.antlr.v4.runtime.Parser;
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;

import java.util.ArrayList;
import java.util.List;

public class ContextTraverser {

    public static List<String> getTokens(ParseTree context) {
        List<String> tokens = new ArrayList<>();
        traverse(context, node -> tokens.add(node.getText()));
        return tokens;
    }

    public static List<String> getRuleNames(ParseTree context) {
        List<String> ruleNames = new ArrayList<>();
        Parser parser = (Parser) context.getPayload();
        traverse(context, node -> {
            if (node instanceof RuleNode) {
                ruleNames.add(parser.getRuleNames()[((RuleNode) node).getRuleContext().getRuleIndex()]);
            }
        });
        return ruleNames;
    }

    private static void traverse(ParseTree node, java.util.function.Consumer<ParseTree> action) {
        action.accept(node);
        for (int i = 0; i < node.getChildCount(); i++) {
            traverse(node.getChild(i), action);
        }
    }
}
```

이제 `SimpleParser` 파라미터를 제거하고 `ParseTree`의 `getPayload` 메서드를 사용하여 파서 인스턴스에 대한 참조를 얻었습니다. 이를 통해 규칙 이름을 가져올 수 있습니다.

## 사용자 입력:
> 특성 파서의 인스턴스를 매개변수로 넘길거야.

## ChatGPT 응답:
> 알겠습니다. 특정 `Parser` 인스턴스를 매개변수로 받아서 사용하는 것이 명확하게 동작을 이해하는 데 도움이 될 것입니다.

다음은 해당 `Parser` 인스턴스를 매개변수로 받아 사용하는 수정된 코드입니다:

```java
import org.antlr.v4.runtime.Parser;
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.RuleNode;

import java.util.ArrayList;
import java.util.List;

public class ContextTraverser {

    public static List<String> getTokens(ParseTree context) {
        List<String> tokens = new ArrayList<>();
        traverse(context, node -> tokens.add(node.getText()));
        return tokens;
    }

    public static List<String> getRuleNames(ParseTree context, Parser parser) {
        List<String> ruleNames = new ArrayList<>();
        traverse(context, node -> {
            if (node instanceof RuleNode) {
                ruleNames.add(parser.getRuleNames()[((RuleNode) node).getRuleContext().getRuleIndex()]);
            }
        });
        return ruleNames;
    }

    private static void traverse(ParseTree node, java.util.function.Consumer<ParseTree> action) {
        action.accept(node);
        for (int i = 0; i < node.getChildCount(); i++) {
            traverse(node.getChild(i), action);
        }
    }
}
```

위의 코드에서 `getRuleNames` 메서드는 `ParseTree`와 `Parser`의 인스턴스를 매개변수로 받아 해당 파서에서 규칙 이름을 가져옵니다.

## 사용자 입력:
> getRuleNames(ParseTree context, Parser parser) 에서 Parser가 인터페이스인가?

## ChatGPT 응답:
> 죄송합니다, 제 실수입니다. `Parser`는 `org.antlr.v4.runtime.Parser` 패키지에 있는 추상 클래스입니다, 인터페이스가 아닙니다. 따라서 `SimpleParser`와 같은 구체적인 파서 클래스는 `Parser`를 상속받습니다.

말씀하신 대로 `Parser`는 추상 클래스입니다. 코드에서는 파서의 인스턴스를 받아 해당 파서의 규칙 이름 배열을 사용하기 위해 `Parser` 타입의 매개변수를 사용하고 있습니다. 

이 방식은 모든 ANTLR4 파서에 대해 동작하도록 설계되었습니다. 그러므로 `SimpleParser` 뿐만 아니라 다른 ANTLR4 파서 인스턴스도 받아들일 수 있습니다.

## 사용자 입력:
> ANTLR4의  RuleContext의 child가 terminalNode만 있는지 확인

## ChatGPT 응답:
> ANTLR4에서 `RuleContext`는 파서 규칙에 해당하는 노드를 나타내며, `TerminalNode`는 입력 텍스트에서 토큰에 직접 해당하는 노드를 나타냅니다.

`RuleContext`의 자식 노드가 모두 `TerminalNode`인지 확인하려면, 해당 `RuleContext`의 모든 자식 노드를 순회하며 각각이 `TerminalNode` 인스턴스인지 확인해야 합니다.

다음은 `RuleContext`의 모든 자식 노드가 `TerminalNode`만 있는지 확인하는 유틸리티 메서드입니다:

```java
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.TerminalNode;

public class ANTLRUtils {

    public static boolean hasOnlyTerminalChildren(ParseTree context) {
        for (int i = 0; i < context.getChildCount(); i++) {
            if (!(context.getChild(i) instanceof TerminalNode)) {
                return false;
            }
        }
        return true;
    }
}
```

이 유틸리티 메서드를 사용하면 주어진 `ParseTree` 또는 `RuleContext`의 모든 자식 노드가 `TerminalNode`만 있는지 쉽게 확인할 수 있습니다.

