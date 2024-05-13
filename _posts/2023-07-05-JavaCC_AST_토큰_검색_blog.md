---
layout: post
title: "JavaCC AST 토큰 검색"
date: 2023-07-05
tags: [JavaCC, 토큰, 검색, AST]
categories: blog
---

## 사용자 입력:
> javaCC에서 jjt를 사용할 때, 정의된  구문노드( AST)를 방문할 때, 해당 구문노드에서 정의된 모든 토큰을 찾는 방법은 없나?

## ChatGPT 응답:
> JavaCC에서 jjt를 사용하여 정의된 구문 노드(AST)를 방문할 때, 해당 노드에서 정의된 모든 토큰을 찾는 것은 상대적으로 간단한 작업입니다. JavaCC는 AST를 생성하는 과정에서 토큰 정보를 포함하므로, AST 노드를 방문할 때 해당 노드에 대한 토큰 정보를 쉽게 얻을 수 있습니다.

AST 노드를 방문하는 방법은 대부분 JavaCC 구문 분석기에 따라 다를 수 있으므로, 여기에서는 가장 일반적인 방법을 설명하겠습니다. 일반적으로 jjtVisitor 클래스를 확장하여 AST 노드를 방문하고, 방문할 때마다 해당 노드에 대한 작업을 수행할 수 있습니다.

아래는 jjtVisitor 클래스를 확장하여 AST 노드를 방문하고, 해당 노드에서 정의된 모든 토큰을 출력하는 예시 코드입니다:

```java
import mypackage.MyParserVisitor;
import mypackage.SimpleNode;

public class MyVisitor implements MyParserVisitor {
    public Object visit(SimpleNode node, Object data) {
        // 현재 노드에서 정의된 토큰에 접근
        Token token = node.jjtGetFirstToken();
        while (token != null) {
            // 토큰 정보 출력 또는 원하는 작업 수행
            System.out.println("Token: " + token.image);
            token = token.next;
        }

        // 자식 노드 방문
        if (node.children != null) {
            for (int i = 0; i < node.children.length; i++) {
                node.children[i].jjtAccept(this, data);
            }
        }

        return null;
    }
}
```

위의 코드에서 `SimpleNode`는 JavaCC 구문 분석기에서 생성된 AST 노드 클래스를 나타냅니다. `visit` 메서드는 각 노드를 방문하고, 해당 노드에서 정의된 토큰에 접근하여 작업을 수행합니다. 또한, `visit` 메서드는 자식 노드를 방문하기 위해 재귀적으로 호출됩니다.

이제 `MyVisitor` 클래스를 사용하여 구문 노드를 방문하고 토큰을 출력할 수 있습니다. 다음은 예시 사용법입니다:

```java
MyParser parser = new MyParser(new StringReader("your input string"));
SimpleNode ast = parser.Start(); // 구문 분석 수행

MyVisitor visitor = new MyVisitor();
ast.jjtAccept(visitor, null); // 구문 노드 방문
```

위의 코드에서 `MyParser`는 JavaCC 구문 분석기에서 생성된 파서 클래스를 나타냅니다. `Start` 메서드를 호출하여 입력 문자열을 구문 분석하고, 생성된 AST를 얻은 후, `MyVisitor`를 사용하여 노드

## ChatGPT 응답:
> 를 방문합니다. 방문 과정에서 토큰 정보가 출력됩니다.

이 방법을 사용하여 AST 노드에서 정의된 모든 토큰에 접근할 수 있습니다. 자세한 내용은 JavaCC 사용자 설명서를 참조하십시오.

## 사용자 입력:
> 위에 코드를 좀 더 발전시켜서,  토큰명도 출력하는 거 알려줘.

## ChatGPT 응답:
> 자바CC(JaavCC)에서 AST 노드를 방문하고 해당 노드에서 정의된 모든 토큰과 토큰명을 출력하는 방법을 보여드리겠습니다. 이를 위해 토큰의 심볼명을 사용할 것입니다.

아래 예시 코드에서는 토큰 심볼명을 얻기 위해 JavaCC의 `jjtGetFirstToken().kind`를 사용합니다. `kind`는 토큰의 심볼 식별자입니다. 이 식별자를 사용하여 토큰 심볼명을 찾을 수 있습니다.

```java
import mypackage.MyParserVisitor;
import mypackage.SimpleNode;
import mypackage.TokenMgrError;

public class MyVisitor implements MyParserVisitor {
    private String[] tokenImage;

    public MyVisitor(String[] tokenImage) {
        this.tokenImage = tokenImage;
    }

    public Object visit(SimpleNode node, Object data) {
        // 현재 노드에서 정의된 토큰에 접근
        Token token = node.jjtGetFirstToken();
        while (token != null) {
            // 토큰 정보 및 토큰명 출력
            int tokenKind = token.kind;
            String tokenName = tokenImage[tokenKind];
            String tokenImage = token.image;
            System.out.println("Token: " + tokenImage + " (" + tokenName + ")");

            token = token.next;
        }

        // 자식 노드 방문
        if (node.children != null) {
            for (int i = 0; i < node.children.length; i++) {
                node.children[i].jjtAccept(this, data);
            }
        }

        return null;
    }
}
```

위의 코드에서 `MyVisitor` 클래스는 생성자에서 토큰 심볼명 배열 `tokenImage`을 받습니다. `visit` 메서드에서 토큰의 `kind`를 사용하여 심볼명을 찾고, 출력에 사용됩니다.

사용 예시는 이전과 동일합니다:

```java
MyParser parser = new MyParser(new StringReader("your input string"));
SimpleNode ast = parser.Start(); // 구문 분석 수행

String[] tokenImage = parser.tokenImage; // 토큰 심볼명 배열 가져오기

MyVisitor visitor = new MyVisitor(tokenImage);
ast.jjtAccept(visitor, null); // 구문 노드 방문
```

위의 코드를 실행하면 구문 노드를 방문하여 해당 노드에서 정의된 모든 토큰과 토큰명이 출력됩니다. 토큰 심볼명은 `tokenImage` 배열에서 찾아지며, 토큰 이미지는 `token.image`에서 얻어집니다.

## 사용자 입력:
> void attribute() : {} {
    <NAME> <EQULS> <STRING>
} 이런 JavaCC 구문이 있는데,  attribute노드에서 child는 구체적으로 어떤 것인가?

## ChatGPT 응답:
> 주어진 JavaCC 구문에서 `attribute` 노드는 `<NAME>`, `<EQULS>`, `<STRING>` 세 개의 자식 노드를 가지게 됩니다. 이 세 자식 노드는 `<NAME>`, `<EQULS>`, `<STRING>` 토큰에 해당하는 토큰 노드입니다.

아래는 `attribute` 노드를 나타내는 AST의 구조를 시각적으로 표현한 것입니다:

```
attribute
├── <NAME>
├── <EQULS>
└── <STRING>
```

따라서, `attribute` 노드의 자식 노드는 순서대로 `<NAME>`, `<EQULS>`, `<STRING>` 토큰 노드입니다.

