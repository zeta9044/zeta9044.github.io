---
layout: post
title: "자바 @Override 효과"
tags: [자바, 효과, Override]
---

## 사용자 입력:
> 자바에서 부모클래스의 함수를 @overide하면 어떤 효과가 있나?

## ChatGPT 응답:
> Java에서 부모 클래스의 함수를 {% raw %}`@Override`{% endraw %} 어노테이션과 함께 오버라이드(재정의)하면 다음과 같은 효과가 있습니다:

1. 컴파일 에러 방지: {% raw %}`@Override`{% endraw %} 어노테이션을 사용하면 컴파일러가 오버라이드된 메서드가 실제로 부모 클래스의 메서드를 오버라이드하고 있는지 확인합니다. 만약 오버라이드하지 않고 있는 경우 컴파일 에러가 발생하여 이를 방지할 수 있습니다.

2. 가독성 향상: {% raw %}`@Override`{% endraw %} 어노테이션은 코드를 읽는 사람에게 오버라이드된 메서드임을 명시적으로 보여줍니다. 이는 코드의 가독성을 향상시키고 오버라이드가 의도적으로 이루어진 것인지를 명확하게 전달합니다.

3. 버그 방지: 부모 클래스의 메서드를 오버라이드할 때, 메서드 시그니처(이름, 매개변수 타입, 반환 타입)를 일치시켜야 합니다. {% raw %}`@Override`{% endraw %} 어노테이션을 사용하면 실수로 시그니처를 잘못 작성한 경우 컴파일러가 에러를 발생시켜 버그를 방지할 수 있습니다.

4. 상속 계층 구조의 일관성: 자식 클래스에서 부모 클래스의 메서드를 오버라이드하여 자신의 동작을 정의할 수 있습니다. 이로 인해 상속 계층 구조 내에서 메서드의 일관성을 유지하면서 특정 클래스에 맞는 동작을 구현할 수 있습니다.

5. 다형성 지원: 오버라이드된 메서드는 다형성을 지원합니다. 즉, 부모 클래스 타입으로 선언된 변수나 인터페이스 타입으로 참조된 경우에도 실제 객체의 오버라이드된 메서드가 호출되어 다양한 동작을 수행할 수 있습니다.

이러한 이유로 {% raw %}`@Override`{% endraw %} 어노테이션은 자바에서 부모 클래스의 함수를 오버라이드할 때 유용하게 사용됩니다.

## 사용자 입력:
> 부모클래스 함수를 override하면 부모클래스 함수기능을 수행하면서 추가 기능이 되는건가?

## ChatGPT 응답:
> 부모 클래스의 함수를 오버라이드하면, 자식 클래스에서 부모 클래스의 함수를 재정의할 수 있습니다. 이때 자식 클래스에서 오버라이드한 함수는 부모 클래스의 함수 기능을 수행하면서 추가적인 기능을 할 수 있습니다.

오버라이드된 함수는 부모 클래스의 함수와 동일한 이름, 매개변수 타입 및 반환 타입을 가져야 합니다. 자식 클래스에서 오버라이드된 함수는 부모 클래스의 함수를 완전히 대체하지 않고, 기존의 동작을 확장하거나 변경할 수 있습니다.

자식 클래스에서 오버라이드된 함수에서 추가적인 작업을 수행하는 방법은 다양합니다. 예를 들어, 오버라이드된 함수 내에서 부모 클래스의 함수를 먼저 호출한 후에 추가적인 로직을 실행할 수 있습니다. 또는 오버라이드된 함수에서 부모 클래스의 함수 결과를 가공하거나 수정할 수도 있습니다.

아래는 간단한 예제입니다. 부모 클래스인 {% raw %}`Animal`{% endraw %}에는 {% raw %}`makeSound()`{% endraw %} 함수가 있고, 이를 자식 클래스 {% raw %}`Cat`{% endraw %}에서 오버라이드하여 추가적인 기능을 수행합니다.

{% raw %}```java
class Animal {
    public void makeSound() {
        System.out.println("Animal is making a sound.");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        super.makeSound();  // 부모 클래스의 makeSound() 호출
        System.out.println("Cat is meowing.");  // 추가적인 동작
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Cat();
        animal.makeSound();  // 출력: "Animal is making a sound." "Cat is meowing."
    }
}
```{% endraw %}

위 예제에서 {% raw %}`Cat`{% endraw %} 클래스에서 {% raw %}`makeSound()`{% endraw %} 함수를 오버라이드하여 부모 클래스의 {% raw %}`makeSound()`{% endraw %}를 호출한 후에 "Cat is meowing."이라는 추가적인 메시지를 출력하도록 했습니다. 따라서 {% raw %}`Cat`{% endraw %} 객체의 {% raw %}`makeSound()`{% endraw %} 메서드를 호출하면 부모 클래스인 {% raw %}`Animal`{% endraw %}의 기능을 수행하면서 추가적인 기능이 실행됩니다.

## 사용자 입력:
> javaCC의 visitor에서 토큰명을 알려면 어떻게 해야 되나?

## ChatGPT 응답:
> JavaCC에서 Visitor를 사용하여 토큰명을 알아내려면 다음과 같은 절차를 따를 수 있습니다:

1. JavaCC 문법 파일(.jj 파일)에서 토큰 정의를 확인합니다. 토큰 정의는 {% raw %}`TOKEN`{% endraw %} 섹션에 위치하며, 정규표현식과 해당 토큰의 명칭을 포함합니다. 예를 들어, 다음과 같은 형식으로 토큰을 정의할 수 있습니다:

{% raw %}```java
TOKEN : {
  < IDENTIFIER: ["a"-"z" "A"-"Z"] (["a"-"z" "A"-"Z" "0"-"9" "_"])* >
  < INTEGER_LITERAL: (["0"-"9"])+ >
  // 추가 토큰 정의...
}
```{% endraw %}

2. Visitor 인터페이스를 구현하는 Visitor 클래스를 작성합니다. Visitor 클래스는 토큰들을 방문할 때 실행될 메서드를 정의합니다.

3. Visitor 클래스의 메서드를 구현하면서 토큰 명칭을 알아낼 수 있습니다. 각 토큰의 메서드는 해당 토큰을 방문할 때 실행되며, 토큰의 정보는 파라미터로 전달됩니다. JavaCC에서 토큰의 정보는 {% raw %}`Token`{% endraw %} 클래스를 통해 액세스할 수 있습니다.

{% raw %}```java
import MyGrammarParser.*; // 자신의 문법 파일에 맞는 패키지 및 클래스 이름으로 변경

public class MyVisitor implements MyGrammarVisitor {

    @Override
    public Object visit(Token token, Object data) {
        // 토큰 정보에 접근하여 토큰 명칭을 알아냄
        String tokenName = token.image;
        System.out.println("Visited token: " + tokenName);
        return null;
    }

    // 다른 메서드들 구현...

}
```{% endraw %}

위의 예제에서 {% raw %}`visit()`{% endraw %} 메서드는 모든 토큰에 대해 호출됩니다. {% raw %}`Token`{% endraw %} 객체의 {% raw %}`image`{% endraw %} 필드는 토큰의 텍스트 값을 포함하고 있습니다. 따라서 {% raw %}`token.image`{% endraw %}를 사용하여 토큰의 명칭을 얻을 수 있습니다.

참고로, 자신의 문법 파일과 패키지 구조에 따라 import 문과 Visitor 클래스의 이름을 조정해야 합니다. 또한 Visitor 클래스에서는 다른 메서드들을 구현하여 방문할 수 있는 다른 구문 요소들을 처리할 수도 있습니다.

## 사용자 입력:
> jjt에서 visitor에서 node를 방문할 때  node에 입력된 모든 텍스트를 찾으려면?

## ChatGPT 응답:
> JavaCC의 JJTree(JJTree 6)에서 Visitor를 사용하여 노드를 방문할 때 노드에 입력된 모든 텍스트를 찾으려면 다음과 같은 절차를 따를 수 있습니다:

1. JJTree 파일(.jjt 파일)에서 노드 정의를 확인합니다. 노드 정의는 {% raw %}`void`{% endraw %} 메서드로 시작하며, 해당 노드에 연결된 텍스트를 가져올 수 있습니다.

2. Visitor 인터페이스를 구현하는 Visitor 클래스를 작성합니다. Visitor 클래스는 노드를 방문할 때 실행될 메서드를 정의합니다.

3. Visitor 클래스의 메서드를 구현하면서 노드의 입력된 텍스트를 찾을 수 있습니다. 각 노드의 메서드는 해당 노드를 방문할 때 실행되며, 노드의 정보는 파라미터로 전달됩니다.

4. 노드에서 {% raw %}`jjtGetValue()`{% endraw %} 메서드를 호출하여 입력된 텍스트를 가져올 수 있습니다. 이 메서드는 {% raw %}`Token`{% endraw %} 타입을 반환하며, {% raw %}`Token`{% endraw %} 객체의 {% raw %}`image`{% endraw %} 필드를 사용하여 텍스트 값을 얻을 수 있습니다.

다음은 간단한 예제입니다. JJTree 파일에서 {% raw %}`Node`{% endraw %} 노드를 정의하고, Visitor 클래스에서 해당 노드를 방문하여 입력된 텍스트를 출력하는 방법을 보여줍니다:

{% raw %}```java
void Node() #void:
{}
{
  <IDENTIFIER> <INTEGER_LITERAL> <STRING_LITERAL>
  {
    // 노드에서 입력된 텍스트를 출력
    Token identifierToken = ((Token) jjtGetChild(0).jjtGetValue());
    Token integerToken = ((Token) jjtGetChild(1).jjtGetValue());
    Token stringToken = ((Token) jjtGetChild(2).jjtGetValue());

    System.out.println("Identifier: " + identifierToken.image);
    System.out.println("Integer Literal: " + integerToken.image);
    System.out.println("String Literal: " + stringToken.image);
  }
}

public class MyVisitor implements MyGrammarVisitor {

  @Override
  public Object visit(SimpleNode node, Object data) {
    // Node 노드를 방문하는 메서드
    if (node.getId() == MyGrammarTreeConstants.JJTNODE) {
      node.jjtAccept(this, data);
    }
    return null;
  }

  // 다른 메서드들 구현...

}
```{% endraw %}

위의 예제에서 {% raw %}`Node()`{% endraw %} 노드에서 {% raw %}`jjtGetValue()`{% endraw %} 메서드를 호출하여 {% raw %}`Token`{% endraw %} 객체를 가져오고, {% raw %}`Token`{% endraw %} 객체의 {% raw %}`image`{% endraw %} 필드를 사용하여 텍스트 값을 출력합니다. Visitor 클래스에서는 {% raw %}`visit()`{% endraw %} 메서드를 구현하여 다른 노드들을 방문할 수 있습니다.

주의사항: JJTree를 사용하여 구문 분석 트리를 생성하면 생성되는 노드 클래스와 메서드 이름은 각각의 문법 파일에 따라 다를 수 있습니다. 예제에서는 {% raw %}`Node()`{% endraw %} 노드를 사용했지만, 실제

## ChatGPT 응답:
> 로는 자신의 노드와 메서드를 확인하고 사용해야 합니다.

