---
layout: post
title: "JavaCC Visitor 구현"
date: 2023-07-05
tags: [구현, JavaCC, Visitor]
categories: blog
---

## 사용자 입력:
> options {
  MULTI = true;
  VISITOR = true;
  TRACK_TOKENS = true;
  STATIC = false;
}

PARSER_BEGIN(PentahoParser)

package com.ids.parser.pentaho.parser;

public class PentahoParser {
      public static void main(String args[]) throws Exception {
          PentahoParser p = new PentahoParser(System.in);
      }
}

PARSER_END(PentahoParser)

SKIP : {
  <DTD: "<!" (~[">"])* ">" >
| <S: (" " | "\t" | "\r" | "\n")+ >
}

TOKEN : {
  <COMMENT: "<!--" (~["<"])* ("-->")? > : IN_COMMENT
}

<IN_COMMENT> MORE : {
  "-->" : DEFAULT
}

TOKEN : {
  <CDATA: "<![CDATA[" (~[">"])* ("]]>")? > : IN_CDATA
}

<IN_CDATA> MORE : {
  "]]>" : DEFAULT
}

TOKEN: {
  < ENTITY_REF: "&" (<NAME>)+ ";" >
| < CHAR_REF: ("&#" (<DIGIT>)+ ";" | "&#x" (<HEXDIGIT>)+ ";" ) >
| < SEA_WS: ( " " | "\t" | ("\r")? "\n" )+ >
| < OPEN: "<" > : INSIDE
| < XML_DECL_OPEN: "<?xml" > : INSIDE
| < SPECIAL_OPEN: "<?" <NAME>> : PROC_INSTR
| < TEXT: (~["<","&"])+ >
}

<INSIDE> TOKEN : {
  < CLOSE: ">" > : DEFAULT
| < SPECIAL_CLOSE: "?>" > : DEFAULT
| < SLASH_CLOSE: "/>" > : DEFAULT
| < SLASH: "/" >
| < EQULS: "=" >
| < STRING: "\"" (~["<", "\""])* "\"" | "\'" (~["<", "\'"])* "\'" >
| < NAME: <NAME_START_CHAR> (<NAME_CHAR>)* >
| < #HEXDIGIT: ["a"-"f", "A"-"F", "0"-"9"] >
| < #DIGIT: ["0"-"9"] >
| < NAME_CHAR: <NAME_START_CHAR> | ["-", "_", ".", "0"-"9", "\u00B7", "\u0300"-"\u036F", "\u203F"-"\u2040"] >
| < NAME_START_CHAR: [":","a"-"z", "A"-"Z"] | ["\u2070"-"\u218F", "\u2C00"-"\u2FEF", "\u3001"-"\uD7FF", "\uF900"-"\uFDCF", "\uFDF0"-"\uFFFD"] >
}

<INSIDE> SKIP : {
    < BLANK: (" " | "\t" | "\r" | "\n")+ >
}

<PROC_INSTR> TOKEN : {
  < PI : "?>" > : DEFAULT
}

<PROC_INSTR> SKIP : {
  < IGNORE : ~[] >
}

void document() : {} {
    [prolog()] (misc())* element() (misc())* <EOF>
}

void prolog() : {} {
     <XML_DECL_OPEN> (attribute())* <SPECIAL_CLOSE>
}

void attribute() : {} {
    <NAME> <EQULS> <STRING>
}

void misc() : {} {
    <COMMENT> | <PI> | <SEA_WS>
}

void element() : {} {
    LOOKAHEAD(3)  <OPEN> <NAME> (attribute())* <CLOSE> content() <OPEN> <SLASH> <NAME> <CLOSE>
    |
    LOOKAHEAD(3)  <OPEN> <NAME> (attribute())* <SLASH_CLOSE>
}

void reference() : {} {
    <ENTITY_REF> | <CHAR_REF>
}

void chardata() : {} {
    <TEXT> | <SEA_WS>
}

void content() : {} {
    [chardata()]
    (

    LOOKAHEAD(2)    (
            element() | reference() | <CDATA> | <PI> | <COMMENT>
        )
        [chardata()]
    )*
}



이것은 javaCC에서 사용하는 jjt파일인데, visitor를 이용해서 구문노드를 순회하면서 어떤 처리를 하고 싶어.  이를 위해서 이 코드에서 빠진 부분이 있으면 변경해줘.

## ChatGPT 응답:
> JavaCC 파일은 파서의 작동 방식을 정의하는데 사용됩니다. 파일 내의 각 부분은 토큰 정의, 문법 규칙 등을 나타냅니다. 그러나, visitor pattern을 사용하려면 노드를 방문하고 처리할 수 있는 방법을 제공해야 합니다.

JavaCC를 사용하여 Visitor 패턴을 적용하려면, 각 노드를 방문할 수 있는 방법을 제공하는 인터페이스와 이를 구현하는 클래스를 추가해야 합니다. 

파서를 생성할 때 "VISITOR=true" 옵션을 사용하면 각 구문노드에 대한 visit() 메소드를 포함하는 Visitor 인터페이스를 생성합니다. 그러나 이 코드에서는 이 옵션을 이미 사용하였으므로, 이 부분은 변경할 필요가 없습니다.

하지만, 생성된 구문 트리를 실제로 순회하고 필요한 작업을 수행하는 Visitor 클래스를 정의해야 합니다. 이 클래스는 방문한 각 노드에 대한 작업을 정의하는 visit() 메소드를 포함해야 합니다.

다음은 문서 노드를 순회하는 간단한 Visitor 클래스의 예시입니다:

```java
public class DocumentVisitor implements PentahoParserVisitor {

  public Object visit(SimpleNode node, Object data) {
    // Default visit method
  }

  public Object visit(ASTdocument node, Object data) {
    // Visit the document node
    return node.childrenAccept(this, data);
  }

  public Object visit(ASTprolog node, Object data) {
    // Visit the prolog node
    return node.childrenAccept(this, data);
  }

  // ... include visit methods for other nodes ...
}
```

이 클래스는 PentahoParserVisitor 인터페이스를 구현하며, 각 visit() 메소드에서는 해당 노드를 방문할 때 수행할 작업을 정의합니다. 일반적으로, 각 visit() 메소드에서는 먼저 노드의 자식을 방문한 다음 (이를 위해 `childrenAccept(this, data)`를 호출), 노드에 대한 특정 작업을 수행합니다.

다른 노드에 대해서도 동일한 방식으로 visit 메소드를 추가해야 합니다. 이러한 방식으로 구문 트리의 각 노드를 방문하고 필요한 작업을 수행할 수 있습니다.

## 사용자 입력:
> Visitor는 이미 만들었어. 실행하는 자바코드 알려줘.

## ChatGPT 응답:
> 만약 당신이 이미 `PentahoParserVisitor`를 구현한 클래스를 만들었다면, 당신의 Visitor를 사용하여 파서가 생성한 구문 트리를 순회할 수 있는 메인 실행 코드는 다음과 같이 작성될 수 있습니다:

```java
public class Main {
  public static void main(String[] args) throws ParseException {
    // 파일 또는 스트림으로부터 입력을 읽습니다.
    // 이 예제에서는 문자열에서 입력을 읽습니다.
    String sourceCode = "<your xml code here>";
    InputStream is = new ByteArrayInputStream(sourceCode.getBytes());

    // 파서를 생성하고 소스 코드를 파싱합니다.
    PentahoParser parser = new PentahoParser(is);
    SimpleNode root = parser.document(); // 'document' is the root rule in your grammar

    // Visitor를 생성하고 구문 트리를 순회합니다.
    PentahoParserVisitor visitor = new YourVisitorImplementation();
    root.jjtAccept(visitor, null);
  }
}
```

위 코드에서는 먼저 소스 코드를 읽고, 이를 파싱하여 구문 트리를 생성합니다. 그 다음, 당신의 `PentahoParserVisitor` 구현을 사용하여 구문 트리를 순회합니다. `YourVisitorImplementation`은 당신이 작성한 `PentahoParserVisitor` 구현의 이름이어야 합니다.

`jjtAccept` 메소드는 구문 트리의 노드를 방문하는 데 사용되며, 첫 번째 인자로 Visitor 객체를 받고, 두 번째 인자로는 노드를 방문할 때 전달할 데이터를 받습니다. 여기서는 데이터로 `null`을 사용했습니다.

참고로, 이 코드를 실행하려면 JavaCC로 생성한 모든 클래스가 클래스패스에 포함되어 있어야 합니다. 또한, 입력은 당신의 문법에 따라 유효한 XML 코드를 포함하는 문자열이어야 합니다.

## 사용자 입력:
> 내가 제공한 소스에서 document가 void로 정의 되어 있는데, 위와 같이 실행 못하지 않나?

## ChatGPT 응답:
> 맞습니다, 제가 그 부분을 간과했습니다. JavaCC에서, `void` 메소드들은 일반적으로 아무런 값을 반환하지 않습니다. 따라서 구문 트리의 루트 노드에 접근하려면 몇 가지 수정이 필요합니다.

일반적으로 JavaCC로 생성한 파서에서는 `void` 대신 `SimpleNode` 또는 파서의 특정 노드 타입을 반환하는 메소드를 사용하여 구문 트리의 루트 노드에 접근합니다. 

이 경우, `document()` 메소드를 `SimpleNode`를 반환하는 메소드로 변경해야 합니다. 이렇게 하려면, 파서 규칙에 반환 타입을 지정하고, 메소드 내에서 적절한 노드 객체를 반환하도록 코드를 추가해야 합니다. 다음은 `document()` 메소드를 수정하는 예시입니다:

```java
SimpleNode document() : {} {
    [prolog()] (misc())* element() (misc())* <EOF>
    {
        // Return the root node of the syntax tree
        return jjtThis;
    }
}
```

그런 다음, 메인 메소드에서는 이제 `document()` 메소드를 호출하여 구문 트리의 루트 노드에 접근할 수 있습니다:

```java
public class Main {
    public static void main(String[] args) throws ParseException {
        // ...same as before...

        // Parser creates the syntax tree and returns the root node
        SimpleNode root = parser.document();

        // ...same as before...
    }
}
```

이렇게 하면, `document()` 메소드는 이제 구문 트리의 루트 노드를 반환하며, 이 노드는 메인 메소드에서 Visitor를 사용하여 구문 트리를 순회하는 데 사용됩니다.

## 사용자 입력:
> 그러면, root노드만 반환하면 구문트리를 순회할 수 있는 건가?

## ChatGPT 응답:
> 네, 맞습니다. 구문 트리(rooted tree)에서 루트 노드(root node)는 트리의 시작점입니다. 따라서 루트 노드를 가지고 있다면, 그 노드부터 시작해서 하위의 모든 노드를 순회(traverse)할 수 있습니다.

Visitor 패턴은 이러한 트리 순회를 쉽게 해줍니다. `jjtAccept` 함수를 호출하면, 해당 노드와 그 하위 노드(sub-nodes)들이 차례로 visitor의 `visit` 메소드를 호출합니다. 이 과정을 통해 루트 노드에서 시작해 트리의 모든 노드를 방문하게 됩니다.

아래의 코드는 Visitor 패턴을 이용해 구문 트리를 순회하는 코드입니다:

```java
public class Main {
  public static void main(String[] args) throws ParseException {
    // ... (소스 코드를 읽고 파싱하는 부분)

    // Parser creates the syntax tree and returns the root node
    SimpleNode root = parser.document();

    // Create your visitor
    PentahoParserVisitor visitor = new YourVisitorImplementation();

    // Start traversing from the root node
    root.jjtAccept(visitor, null);
  }
}
```

이 코드에서, `jjtAccept` 메소드를 호출하면서 visitor를 인자로 전달합니다. 이렇게 하면, visitor의 `visit` 메소드가 각 노드를 방문하면서 호출되며, 이를 통해 구문 트리의 각 노드에 대한 작업을 수행할 수 있습니다. 이 방법을 이용하면, 루트 노드만 가지고 있어도 전체 구문 트리를 효과적으로 순회할 수 있습니다.

## 사용자 입력:
> document() 반환 타입을 자동생성되는 Node 인터페이스로 지정해도 되나?

## ChatGPT 응답:
> JavaCC를 사용하면, `Node`라는 인터페이스와 `SimpleNode`라는 기본 클래스가 자동으로 생성됩니다. 이들은 구문 트리의 노드를 표현하는데 사용됩니다. `Node` 인터페이스는 구문 트리를 순회하거나 조작하는데 필요한 기본 메소드를 정의하며, `SimpleNode` 클래스는 이 인터페이스를 기본적으로 구현한 것입니다.

따라서, `document()` 메소드의 반환 타입을 `Node`로 지정해도 문제 없습니다. 이 경우, 이 메소드는 여전히 구문 트리의 루트 노드를 반환하게 됩니다. 그러나 반환 타입이 `Node`인 경우, `SimpleNode`에서 제공하는 추가 메소드를 사용할 수 없게 됩니다.

아래는 반환 타입을 `Node`로 지정한 `document()` 메소드의 예시입니다:

```java
Node document() : {} {
    [prolog()] (misc())* element() (misc())* <EOF>
    {
        // Return the root node of the syntax tree
        return jjtThis;
    }
}
```

그러나, 이렇게 `Node`를 반환 타입으로 사용하면, 구문 트리를 순회하거나 조작하는 코드에서는 이 노드를 `SimpleNode`로 캐스팅해야 `SimpleNode`에서 제공하는 추가 메소드를 사용할 수 있습니다. 따라서, 가능하다면 `SimpleNode`를 반환 타입으로 사용하는 것이 좋습니다. 이렇게 하면 `SimpleNode`에서 제공하는 모든 메소드를 사용할 수 있으므로, 구문 트리를 더 쉽게 조작할 수 있습니다.

