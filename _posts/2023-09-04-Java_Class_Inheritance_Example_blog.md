---
layout: post
title: "Java Class Inheritance Example"
tags: [Example, Inheritance, Class, Java]
---

## 사용자 입력:
> 자바에서 Class A extends B {} , Class B implement C {} 이런 관계가 있는데, Class A에서 B의 메소드를 오버라이드하는 예시 보여줘.

## ChatGPT 응답:
> 먼저 주어진 관계를 통해 간단한 예제를 만들어 보겠습니다.

1. 인터페이스 {% raw %}`C`{% endraw %}를 정의합니다. 이 인터페이스는 {% raw %}`showMessage`{% endraw %}라는 메소드를 가지고 있습니다.
2. 클래스 {% raw %}`B`{% endraw %}는 인터페이스 {% raw %}`C`{% endraw %}를 구현합니다. 따라서 {% raw %}`showMessage`{% endraw %} 메소드를 오버라이드해야 합니다.
3. 클래스 {% raw %}`A`{% endraw %}는 클래스 {% raw %}`B`{% endraw %}를 상속받습니다. 클래스 {% raw %}`A`{% endraw %}에서는 {% raw %}`B`{% endraw %}의 {% raw %}`showMessage`{% endraw %} 메소드를 다시 오버라이드합니다.

{% raw %}```java
// Step 1: Interface C
interface C {
    void showMessage();
}

// Step 2: Class B implements C
class B implements C {
    @Override
    public void showMessage() {
        System.out.println("Message from class B");
    }
}

// Step 3: Class A extends B
class A extends B {
    @Override
    public void showMessage() {
        System.out.println("Message from class A");
    }
}

public class Main {
    public static void main(String[] args) {
        A objA = new A();
        objA.showMessage(); // This should print "Message from class A"
    }
}
```{% endraw %}

위 예제에서 {% raw %}`Main`{% endraw %} 클래스의 {% raw %}`main`{% endraw %} 메소드를 실행하면, "Message from class A"가 출력됩니다. 클래스 {% raw %}`A`{% endraw %}에서 {% raw %}`showMessage`{% endraw %} 메소드를 오버라이드했기 때문에, 클래스 {% raw %}`B`{% endraw %}의 {% raw %}`showMessage`{% endraw %} 메소드 대신 클래스 {% raw %}`A`{% endraw %}의 {% raw %}`showMessage`{% endraw %} 메소드가 호출됩니다.

## 사용자 입력:
> B는 C를 implement했는데, 로직이 없다면, A에서 B의 메소드를 호출할 때, super를 쓰지 않아도 되지?

## ChatGPT 응답:
> 맞습니다. 클래스 {% raw %}`B`{% endraw %}가 인터페이스 {% raw %}`C`{% endraw %}의 메소드를 구현할 때 로직을 제공하지 않는다면 (즉, {% raw %}`B`{% endraw %}에서 해당 메소드가 추상 메소드로 남아 있다면), 클래스 {% raw %}`A`{% endraw %}에서 그 메소드를 오버라이드할 때 {% raw %}`super`{% endraw %}를 사용하여 상위 클래스의 메소드를 호출할 필요가 없습니다. 왜냐하면 실제로 호출할 로직이 존재하지 않기 때문입니다.

다음은 해당 시나리오에 대한 예시입니다:

{% raw %}```java
// Step 1: Interface C
interface C {
    void showMessage();
}

// Step 2: Class B implements C but does not provide a concrete implementation for showMessage
abstract class B implements C {
    // Note: We didn't provide an implementation for showMessage here
}

// Step 3: Class A extends B and provides a concrete implementation for showMessage
class A extends B {
    @Override
    public void showMessage() {
        System.out.println("Message from class A");
        // No need to call super.showMessage() here since B's showMessage is abstract
    }
}

public class Main {
    public static void main(String[] args) {
        A objA = new A();
        objA.showMessage(); // This should print "Message from class A"
    }
}
```{% endraw %}

이 예제에서는 {% raw %}`B`{% endraw %}가 {% raw %}`showMessage`{% endraw %}를 구현하지 않았기 때문에 {% raw %}`B`{% endraw %}는 추상 클래스로 선언되었습니다. 따라서 {% raw %}`A`{% endraw %}에서 {% raw %}`showMessage`{% endraw %}를 오버라이드할 때 {% raw %}`super.showMessage()`{% endraw %}를 호출할 필요가 없습니다.

