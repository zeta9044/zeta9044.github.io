---
layout: post
title: "ANTLR4 to JavaCC"
tags: [to, JavaCC]
---

## 사용자 입력:
> DTD         :   '<!' .*? '>'            -> skip ;  이것은 ANTLR4 lexer 일부인데, javacc 로 바꾸면?

## ChatGPT 응답:
> ANTL4과 JavaCC는 모두 파서 생성 도구이지만, 문법 및 작성 방식이 다릅니다. 주어진 ANTLR4 렉서 규칙을 JavaCC로 변환하려면 다음과 같이 할 수 있습니다:

{% raw %}```
SKIP : "<!" (~["\r","\n"])* ">"

```{% endraw %}

위 코드는 JavaCC 렉서 파일에 추가해야 합니다. 이 코드는 DTD 주석(<!...>)을 건너뛰도록 설정합니다. 주석 내용이 개행 문자를 포함하지 않도록 제한하였으며, 개행 문자를 제외한 모든 문자에 대해 건너뜁니다.

자세한 내용은 JavaCC 문서를 참조하시기 바랍니다.

