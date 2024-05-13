---
layout: post
title: "ANTLR4 Listener에서 AST 이름 가져오기"
tags: [AST, 이름]
---

## 사용자 입력:
> antlr4로 작성된 Listner에서 AST명칭 가져오는 법

## ChatGPT 응답:
> ANTLR4는 문법을 기반으로 파서와 리스너 또는 방문자 패턴을 자동으로 생성해주는 도구입니다. 리스너를 사용하여 파싱된 트리를 순회하며 작업을 수행할 때 AST(Abstract Syntax Tree)의 특정 노드를 방문하게 됩니다. 각 노드의 타입(즉, 문법 규칙의 이름)을 가져오려면 아래와 같은 방법을 사용할 수 있습니다.

먼저, ANTLR가 생성한 리스너에서 관심 있는 메서드를 오버라이드합니다:

{% raw %}```java
public class MyCustomListener extends MyGrammarBaseListener {
    @Override
    public void enterSomeRule(MyGrammarParser.SomeRuleContext ctx) {
        // 여기서 ctx 객체를 사용하여 AST 정보에 액세스
    }
}
```{% endraw %}

이제 {% raw %}`ctx`{% endraw %} 객체를 사용하여 AST 노드의 이름(문법 규칙의 이름)을 가져올 수 있습니다. 아래는 {% raw %}`ctx`{% endraw %} 객체에서 노드의 이름을 가져오는 방법입니다:

{% raw %}```java
String ruleName = MyGrammarParser.ruleNames[ctx.getRuleIndex()];
System.out.println(ruleName);
```{% endraw %}

위의 코드는 {% raw %}`enterSomeRule`{% endraw %} 메서드가 호출될 때 현재 방문 중인 노드의 규칙 이름을 출력합니다. 이 방법을 사용하면 리스너 내의 다른 메서드에서도 특정 노드의 이름을 얻을 수 있습니다.

주의: {% raw %}`MyGrammarParser.ruleNames`{% endraw %}와 같은 자동 생성된 코드는 ANTLR가 생성한 파서의 실제 이름에 따라 다를 수 있습니다. 따라서 예제 코드의 {% raw %}`MyGrammarParser`{% endraw %}를 실제 파서 클래스의 이름으로 대체해야 할 수 있습니다.

