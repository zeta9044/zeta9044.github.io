---
layout: post
title: "ANTLR4 getPreviousSiblingOfType 함수"
date: 2023-09-12
tags: [함수, getPreviousSiblingOfType]
categories: blog
---

## 사용자 입력:
> ANTLR4에서     public <T extends ParserRuleContext> T getPreviousSiblingOfType(ParserRuleContext ctx, Class<T> ruleType) {
        if (ctx.getParent() == null) return null;

        List<ParseTree> siblings = ctx.getParent().children;
        int index = siblings.indexOf(ctx);
        while (index > 0) {
            index--;
            if (ruleType.isInstance(siblings.get(index))) {
                return ruleType.cast(siblings.get(index));
            }
        }
        return null;
    } 이 함수를 쓰는 예제 제시해봐.

## ChatGPT 응답:
> ANTLR4를 사용하여 `getPreviousSiblingOfType` 함수를 호출하는 예제를 제공해 드리겠습니다. 이 함수는 주어진 `ParserRuleContext`의 형제 노드 중에서 특정 유형의 노드를 찾아 반환하는 데 사용됩니다.

먼저, ANTLR4를 사용하여 파싱 트리를 생성하고 해당 트리를 기반으로 함수를 호출하는 예제 코드를 보여 드리겠습니다. 다음은 간단한 산술 표현식을 파싱하고 그 중에서 특정 유형의 노드를 찾는 예제입니다.

```java
import org.antlr.v4.runtime.*;
import org.antlr.v4.runtime.tree.*;

public class Example {
    public static void main(String[] args) throws Exception {
        // ANTLR 입력 스트림 생성
        String expression = "3 + 4 * (5 - 2)";
        CharStream input = CharStreams.fromString(expression);

        // 파서 생성
        ArithmeticLexer lexer = new ArithmeticLexer(input);
        CommonTokenStream tokens = new CommonTokenStream(lexer);
        ArithmeticParser parser = new ArithmeticParser(tokens);

        // 파싱 시작
        ArithmeticParser.ExpressionContext context = parser.expression();

        // getPreviousSiblingOfType 함수 호출
        ArithmeticParser.AddSubContext addSubContext = getPreviousSiblingOfType(context, ArithmeticParser.AddSubContext.class);
        if (addSubContext != null) {
            System.out.println("Found AddSubContext: " + addSubContext.getText());
        } else {
            System.out.println("AddSubContext not found.");
        }
    }

    public static <T extends ParserRuleContext> T getPreviousSiblingOfType(ParserRuleContext ctx, Class<T> ruleType) {
        if (ctx.getParent() == null) return null;

        List<ParseTree> siblings = ctx.getParent().children;
        int index = siblings.indexOf(ctx);
        while (index > 0) {
            index--;
            if (ruleType.isInstance(siblings.get(index))) {
                return ruleType.cast(siblings.get(index));
            }
        }
        return null;
    }
}
```

이 코드는 `ArithmeticLexer` 및 `ArithmeticParser`를 사용하여 산술 표현식을 파싱하고, `getPreviousSiblingOfType` 함수를 호출하여 `AddSubContext` 타입의 형제 노드를 찾습니다. 이 예제에서는 `"3 + 4 * (5 - 2)"` 문자열을 파싱하고, `AddSubContext` 타입의 형제 노드를 찾아내고 해당 노드의 텍스트를 출력합니다.

