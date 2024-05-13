---
layout: post
title: "ANTLR4 파서에서 Identifier 찾기"
date: 2023-10-16
tags: [Identifier, 파서, 찾기]
categories: blog
---

## 사용자 입력:
> antlr4로 parser를 만들고 있는데,  context rule하위의 identifier란 토큰을 찾는 방법은?

## ChatGPT 응답:
> ANTLR4를 사용하여 parser를 작성할 때, context rule 내에서 특정 토큰 (예: "identifier")을 찾는 방법은 다음과 같습니다.

1. 먼저, 해당 토큰 (identifier)을 나타내는 터미널 룰 (lexer rule)을 정의해야 합니다. 예를 들어, "identifier"라는 이름의 lexer rule을 정의할 수 있습니다. 아래는 간단한 예시입니다:

```antlr
Identifier: [a-zA-Z_][a-zA-Z0-9_]*;
```

이렇게 정의한 Identifier 룰은 영문자나 밑줄로 시작하고, 영문자, 숫자, 밑줄의 조합으로 이루어진 토큰을 나타냅니다.

2. 이제 context rule (parser rule)에서 이 identifier를 사용하려면 해당 룰에서 identifier를 참조하면 됩니다. 예를 들어, 다음과 같이 사용할 수 있습니다:

```antlr
someRule: Identifier;
```

위의 예에서 `someRule`은 `Identifier` 토큰을 기대하는 파서 규칙입니다. 이렇게하면 파서가 `Identifier` 토큰을 찾아내고 이를 `someRule`에 할당합니다.

3. 이제 파서를 실행할 때 입력 텍스트에 대한 토큰화 및 구문 분석을 수행하면, 파서가 입력 텍스트에서 "identifier" 토큰을 인식하고 이를 파서 규칙에 따라 처리할 것입니다.

간단한 예제를 통해 더 자세히 설명하겠습니다. 다음은 식별자를 파싱하는 간단한 ANTLR4 파서의 일부입니다:

```antlr
grammar MyParser;

// Lexer rules
Identifier: [a-zA-Z_][a-zA-Z0-9_]*;

// Parser rules
expression: Identifier;
```

위의 파서는 "expression" 규칙에서 "Identifier"를 파싱하는 간단한 예입니다. 이제 "expression" 규칙을 사용하여 식별자를 파싱할 수 있습니다:

```java
import org.antlr.v4.runtime.*;

public class Main {
    public static void main(String[] args) throws Exception {
        String input = "myVariable";
        ANTLRInputStream inputStream = new ANTLRInputStream(input);
        MyParserLexer lexer = new MyParserLexer(inputStream);
        CommonTokenStream tokenStream = new CommonTokenStream(lexer);
        MyParserParser parser = new MyParserParser(tokenStream);

        MyParserParser.ExpressionContext expressionContext = parser.expression();
        
        // 이제 expressionContext에서 파싱한 식별자를 사용할 수 있습니다.
        String identifier = expressionContext.Identifier().getText();
        System.out.println("Parsed Identifier: " + identifier);
    }
}
```

위의 코드에서 "expression"을 파싱하면 입력 텍스트에서 "myVariable"라는 식별자를 인식하고, 이를 가져와서 출력합니다.

## 사용자 입력:
> 일반적인 util로 만들어서 찾고 싶어.

## ChatGPT 응답:
> ANTLR4에서 특정 토큰을 찾아내기 위한 일반적인 유틸리티 클래스를 작성할 수 있습니다. 이를 위해서는 파서와 리스너 또는 비지터를 사용하여 파싱한 후, 원하는 토큰을 추출하는 코드를 작성해야 합니다. 다음은 이를 수행하는 간단한 유틸리티 클래스의 예시입니다.

```java
import org.antlr.v4.runtime.*;
import org.antlr.v4.runtime.tree.ParseTree;
import org.antlr.v4.runtime.tree.ParseTreeWalker;

public class TokenFinder {

    public static void findTokens(String input, String ruleName) throws Exception {
        ANTLRInputStream inputStream = new ANTLRInputStream(input);
        YourLexer lexer = new YourLexer(inputStream);
        CommonTokenStream tokenStream = new CommonTokenStream(lexer);
        YourParser parser = new YourParser(tokenStream);

        ParseTree tree = parser.yourStartRule(); // 여기에 시작 규칙을 지정해야 합니다.

        ParseTreeWalker walker = new ParseTreeWalker();
        walker.walk(new YourListener(), tree);
    }

    public static class YourListener extends YourParserBaseListener {
        @Override
        public void exitEveryRule(ParserRuleContext ctx) {
            Token token = ctx.getStop(); // 파서 규칙에서 마지막 토큰 가져오기
            if (token.getType() == YourLexer.IDENTIFIER) {
                System.out.println("Found Identifier: " + token.getText());
            }
        }
    }

    public static void main(String[] args) throws Exception {
        String input = "myVariable = 42;";
        String ruleName = "yourStartRule"; // 시작 규칙의 이름을 지정하세요.
        findTokens(input, ruleName);
    }
}
```

위의 코드에서는 ANTLR4를 사용하여 파싱한 후, `YourListener` 클래스에서 exitEveryRule 메서드를 오버라이드하여 파서 규칙에서 마지막 토큰을 가져온 다음, 해당 토큰이 원하는 토큰인지 확인합니다. 이 경우, "IDENTIFIER" 토큰을 찾아내고 출력합니다.

이렇게 하면 일반적인 유틸리티 클래스를 작성하여 특정 토큰을 찾을 수 있습니다. 원하는 규칙 및 토큰에 대한 조건을 설정하여 유용한 정보를 추출할 수 있습니다.

## 사용자 입력:
> rulecontext아래 특정 ruleContext를 모두 찾아내는 util함수 알려줘.

## ChatGPT 응답:
> ANTLR4에서 특정 `RuleContext` 아래에 있는 모든 `RuleContext`를 찾아내는 유틸리티 함수를 작성하는 것은 비교적 간단합니다. 아래는 이 작업을 수행하는 Java 유틸리티 함수의 예제입니다:

```java
import org.antlr.v4.runtime.ParserRuleContext;
import org.antlr.v4.runtime.tree.ParseTree;
import java.util.ArrayList;
import java.util.List;

public class RuleContextFinder {

    // 주어진 RuleContext 아래에 있는 모든 RuleContext를 찾아 리스트로 반환
    public static List<ParserRuleContext> findAll(ParserRuleContext rootContext, Class<? extends ParserRuleContext> targetContextClass) {
        List<ParserRuleContext> foundContexts = new ArrayList<>();
        findAllRecursive(rootContext, targetContextClass, foundContexts);
        return foundContexts;
    }

    // 재귀적으로 RuleContext를 찾는 메서드
    private static void findAllRecursive(ParserRuleContext context, Class<? extends ParserRuleContext> targetContextClass, List<ParserRuleContext> foundContexts) {
        if (context.getClass().equals(targetContextClass)) {
            foundContexts.add(context);
        }

        for (int i = 0; i < context.getChildCount(); i++) {
            ParseTree child = context.getChild(i);
            if (child instanceof ParserRuleContext) {
                findAllRecursive((ParserRuleContext) child, targetContextClass, foundContexts);
            }
        }
    }

    public static void main(String[] args) {
        // 여기서는 가상의 파서와 규칙을 가정합니다.
        // YourParser와 YourParserRuleContext는 실제 코드에 맞게 변경해야 합니다.
        YourParser parser = new YourParser(null);
        YourParserRuleContext rootContext = parser.yourStartRule(); // 시작 규칙에 대한 파서 규칙을 호출

        // 특정 RuleContext를 찾아내기 위한 클래스 (예: YourSpecificRuleContext)
        Class<? extends ParserRuleContext> targetContextClass = YourSpecificRuleContext.class;

        List<ParserRuleContext> foundContexts = findAll(rootContext, targetContextClass);

        for (ParserRuleContext context : foundContexts) {
            System.out.println("Found: " + context.getText());
        }
    }
}
```

위의 코드에서 `findAll` 함수는 주어진 `rootContext` 아래에서 특정 클래스의 `ParserRuleContext`를 찾아내는 데 사용됩니다. 이 함수는 재귀적으로 트리를 탐색하며 해당 클래스에 해당하는 `RuleContext`를 모두 찾아내고 리스트로 반환합니다.

실제 파서와 규칙에 맞게 코드를 수정하고 `YourSpecificRuleContext`를 찾으려는 규칙의 컨텍스트 클래스로 변경하십시오. 이제 `findAll` 함수를 호출하면 해당 클래스 아래에 있는 모든 `ParserRuleContext` 인스턴스를 찾을 수 있습니다.

