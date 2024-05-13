---
layout: post
title: "Extract Full String from ANTLR4 RuleContext"
date: 2023-09-12
tags: [String, Full, RuleContext, Extract, from]
categories: blog
---

## 사용자 입력:
> ANTLR4의 RuleContext에서 skip도 포함하는  전체 문자열을 추출 할려면?

## ChatGPT 응답:
> ANTLR4에서 `RuleContext` 객체는 파싱 중에 발생하는 문맥에 대한 정보를 나타냅니다. 일반적으로 `RuleContext` 객체를 사용하여 구문 분석된 트리의 특정 부분에 접근할 수 있습니다.

`RuleContext`에서 전체 문자열을 추출하려면, 해당 `RuleContext`의 시작 토큰과 끝 토큰을 찾아 그 사이의 텍스트를 가져올 수 있습니다. 이것은 skip, hidden tokens 등 모든 토큰들을 포함할 수 있습니다.

다음은 `RuleContext`에서 전체 문자열을 추출하는 방법입니다:

```python
def get_full_text(ctx):
    # 토큰 스트림을 가져옴
    tokens = ctx.parser.getTokenStream()

    # 시작 토큰과 끝 토큰의 인덱스를 가져옴
    start_idx = ctx.start.tokenIndex
    stop_idx = ctx.stop.tokenIndex

    # 토큰 스트림에서 해당 범위의 텍스트를 가져옴
    return tokens.getText(start_idx, stop_idx)
```

이 함수를 사용하면 `RuleContext`에서 해당 범위의 전체 텍스트를 가져올 수 있습니다. 

참고로, skip 또는 hidden channel에서의 토큰들은 기본적으로 토큰 스트림에 포함되지 않을 수 있습니다. 이러한 토큰들도 포함시키려면 토큰 스트림을 생성할 때 해당 채널들을 포함시켜야 합니다.

## 사용자 입력:
> 자바코드로 알려줘.

## ChatGPT 응답:
> 물론이죠! ANTLR4에서 Java를 사용하여 `RuleContext`에서 전체 문자열을 추출하려면 다음과 같이 할 수 있습니다:

```java
public static String getFullText(ParserRuleContext ctx) {
    if (ctx.start == null || ctx.stop == null || ctx.start.getStartIndex() < 0 || ctx.stop.getStopIndex() < 0) {
        return "";
    }

    TokenStream tokens = ctx.start.getTokenSource().getTokenStream();
    if (tokens == null) {
        return "";
    }

    int startIdx = ctx.start.getTokenIndex();
    int stopIdx = ctx.stop.getTokenIndex();
    if (startIdx > stopIdx || stopIdx > tokens.size()) {
        return "";
    }

    return tokens.getText(Interval.of(startIdx, stopIdx));
}
```

이 코드는 주어진 `RuleContext`의 시작 토큰과 끝 토큰 사이의 텍스트를 반환합니다. 

또한 `RuleContext`의 시작 또는 끝 토큰이 없거나 유효하지 않은 경우에 대한 처리를 포함하고 있습니다. 

이 함수를 사용하여 파싱된 트리의 특정 노드에서 전체 텍스트를 쉽게 추출할 수 있습니다.

## 사용자 입력:
> 위의 매개변수가 ParserTree로도 할 수 있나?

## ChatGPT 응답:
> 네, 가능합니다.

ANTLR4에서 `ParserRuleContext`는 `ParseTree`의 하위 클래스입니다. 따라서 `ParseTree` 객체를 사용하여 전체 문자열을 추출하려면 약간의 수정이 필요합니다. 

다음은 `ParseTree`를 인수로 받아 전체 문자열을 추출하는 Java 함수입니다:

```java
public static String getFullText(ParseTree tree) {
    if (tree instanceof ParserRuleContext) {
        return getFullText((ParserRuleContext) tree);
    }

    if (tree.getChildCount() == 0) {
        return tree.getText();
    }

    Token startToken = ((TerminalNode) tree.getChild(0)).getSymbol();
    Token stopToken = ((TerminalNode) tree.getChild(tree.getChildCount() - 1)).getSymbol();

    if (startToken == null || stopToken == null) {
        return "";
    }

    TokenStream tokens = startToken.getTokenSource().getTokenStream();
    if (tokens == null) {
        return "";
    }

    int startIdx = startToken.getTokenIndex();
    int stopIdx = stopToken.getTokenIndex();

    if (startIdx > stopIdx || stopIdx > tokens.size()) {
        return "";
    }

    return tokens.getText(Interval.of(startIdx, stopIdx));
}
```

위의 함수는 `ParseTree`를 받아 해당 트리의 전체 문자열을 반환합니다. 만약 `ParseTree`가 `ParserRuleContext`의 인스턴스라면 이전에 제공된 `getFullText(ParserRuleContext ctx)` 함수를 호출합니다. 그렇지 않으면 처음과 마지막 자식 노드의 토큰을 사용하여 전체 문자열을 추출합니다.

이렇게 수정하면 `ParseTree`를 사용하여 전체 문자열을 쉽게 추출할 수 있습니다.

