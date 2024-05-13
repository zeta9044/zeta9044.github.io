---
layout: post
title: "ANTLR4 TerminalNode 출력"
date: 2024-02-16
tags: [TerminalNode, 출력]
categories: blog
---

## 사용자 입력:
> antlr4에서 TerminalNode가 문자열리터럴이면, 출력하는 유틸함수 만들어봐.

## ChatGPT 응답:
> ANTLR4(ANother Tool for Language Recognition)에서 `TerminalNode`는 파싱된 트리에서 토큰을 나타내는 노드입니다. 문자열 리터럴인 `TerminalNode`를 찾아서 출력하는 유틸리티 함수를 만들려면, ANTLR4의 방문자(Visitor) 또는 리스너(Listener) 패턴을 사용할 수 있습니다. 여기서는 방문자 패턴을 사용하여 간단한 예제를 만들어 보겠습니다.

ANTLR4의 방문자 클래스를 확장하여 특정 노드를 방문할 때 원하는 동작을 정의할 수 있습니다. 이 경우, 모든 `TerminalNode`를 방문하고, 해당 노드가 문자열 리터럴인지 확인한 다음, 문자열 리터럴이면 출력하는 함수를 구현합니다. 문자열 리터럴을 식별하는 방법은 사용하는 문법에 따라 다를 수 있으므로, 예제에서는 단순화를 위해 특정 토큰 타입(예: `STRING_LITERAL`)을 확인하는 방식을 사용합니다.

먼저, ANTLR4로 생성된 파서와 방문자 클래스를 가정하고 시작합니다. `YourLanguageVisitor`라는 이름의 방문자 클래스를 확장하고, `visitTerminal` 메서드를 오버라이드하여 구현할 것입니다. 이 예제에서는 ANTLR4가 생성한 파서와 방문자의 정확한 이름을 모르기 때문에, 실제 구현에서는 이 이름들을 실제 프로젝트에서 생성된 클래스 이름으로 변경해야 합니다.

```java
import org.antlr.v4.runtime.tree.TerminalNode;
import org.antlr.v4.runtime.Token;
import your.language.parser.YourLanguageParser;
import your.language.parser.YourLanguageParserBaseVisitor;

public class StringLiteralPrinterVisitor extends YourLanguageParserBaseVisitor<Void> {
    @Override
    public Void visitTerminal(TerminalNode node) {
        Token token = node.getSymbol();
        if (token.getType() == YourLanguageParser.STRING_LITERAL) {
            System.out.println("String Literal Found: " + token.getText());
        }
        return null; // 계속해서 다른 노드 방문을 위해 null 반환
    }
}
```

이 코드에서 `YourLanguageParser.STRING_LITERAL`는 파서가 생성한 토큰 타입 중 문자열 리터럴에 해당하는 타입을 나타냅니다. 실제 사용 시 `YourLanguageParser`와 `YourLanguageParserBaseVisitor`를 해당 언어에 맞게 변경해야 합니다.

사용 방법은 다음과 같습니다:

1. 파싱할 입력으로부터 `ParseTree`를 생성합니다.
2. `StringLiteralPrinterVisitor` 인스턴스를 생성합니다.
3. 생성된 `ParseTree`에 대해 `visit` 메서드를 호출하여 문자열 리터럴을 출력합니다.

```java
YourLanguageParser parser = new YourLanguageParser(new CommonTokenStream(new YourLanguageLexer(CharStreams.fromString(yourInput))));
ParseTree tree = parser.yourStartRule(); // yourStartRule은 시작 규칙에 따라 변경됩니다.
StringLiteralPrinterVisitor visitor = new StringLiteralPrinterVisitor();
visitor.visit(tree);
```

이 코드 스니펫은 ANTLR4를 사용하여 특정 노드(여기서는 문자열 리터럴)를 방문하고 출력하는 방법을 보여줍니다. 실제 사용을 위해서는 문법 파일에서 정의한 `STRING_LITERAL`과 같은 토큰 타입, 시작 규칙, 그리고 ANTLR4에 의해 생성된 클래스 이름에 맞게 코드를 수정해야 합니다.

