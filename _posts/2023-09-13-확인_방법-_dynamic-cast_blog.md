---
layout: post
title: "확인 방법 - dynamic_cast"
tags: [방법, 확인]
---

## 사용자 입력:
> ANTLR4로 CPP14언어를  개발할 때,  String getFuncAccType(ParserRuleContext ctx) 이런 함수가 있는데,  ctx가  CPP14Parser.FunctionDefinitionContext인지 확인하는 방법.

## ChatGPT 응답:
> ANTLR4를 사용하여 C++ 언어로 파서를 생성할 때, 특정 context가 특정 타입인지 확인하려면 C++의 {% raw %}`dynamic_cast`{% endraw %}를 사용할 수 있습니다.

{% raw %}`CPP14Parser::FunctionDefinitionContext`{% endraw %} 타입인지 확인하려면 다음과 같이 할 수 있습니다:

{% raw %}```cpp
CPP14Parser::FunctionDefinitionContext* funcDefCtx = dynamic_cast<CPP14Parser::FunctionDefinitionContext*>(ctx);
if (funcDefCtx) {
    // ctx는 CPP14Parser::FunctionDefinitionContext 타입입니다.
} else {
    // ctx는 CPP14Parser::FunctionDefinitionContext 타입이 아닙니다.
}
```{% endraw %}

이 방법을 사용하면 {% raw %}`ctx`{% endraw %}가 {% raw %}`CPP14Parser::FunctionDefinitionContext`{% endraw %} 타입인지 쉽게 확인할 수 있습니다.

## 사용자 입력:
> 자바소스로 알려줘.

## ChatGPT 응답:
> 죄송합니다. 자바를 사용할 때 {% raw %}`ctx`{% endraw %}가 {% raw %}`CPP14Parser.FunctionDefinitionContext`{% endraw %}인지 확인하는 방법을 알려드리겠습니다.

자바에서는 {% raw %}`instanceof`{% endraw %} 연산자를 사용하여 객체가 특정 클래스의 인스턴스인지 확인할 수 있습니다.

예를 들면:

{% raw %}```java
if (ctx instanceof CPP14Parser.FunctionDefinitionContext) {
    // ctx는 CPP14Parser.FunctionDefinitionContext 타입입니다.
} else {
    // ctx는 CPP14Parser.FunctionDefinitionContext 타입이 아닙니다.
}
```{% endraw %}

이 코드를 사용하면 {% raw %}`ctx`{% endraw %}가 {% raw %}`CPP14Parser.FunctionDefinitionContext`{% endraw %} 타입인지 쉽게 확인할 수 있습니다.

