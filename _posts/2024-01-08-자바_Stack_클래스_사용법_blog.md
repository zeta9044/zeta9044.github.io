---
layout: post
title: "자바 Stack 클래스 사용법"
tags: [자바, 클래스, 용법, Stack]
---

## 사용자 입력:
> 자바의 Stack클래스 사용법에 대해서 알려줘.

## ChatGPT 응답:
> 자바에서 {% raw %}`Stack`{% endraw %} 클래스는 후입선출(LIFO, Last In First Out) 구조의 데이터를 저장하는 데 사용됩니다. {% raw %}`Stack`{% endraw %} 클래스는 {% raw %}`java.util`{% endraw %} 패키지에 포함되어 있습니다. 아래는 {% raw %}`Stack`{% endraw %} 클래스를 사용하는 간단한 예제입니다.

1. **Import 문 추가:**
   {% raw %}`Stack`{% endraw %} 클래스를 사용하기 위해 다음 import 문을 추가합니다.
   {% raw %}```java
   import java.util.Stack;
   ```{% endraw %}

2. **Stack 생성:**
   {% raw %}`Stack`{% endraw %} 객체를 생성합니다.
   {% raw %}```java
   Stack<Integer> stack = new Stack<>();
   ```{% endraw %}

3. **요소 추가 (push):**
   {% raw %}`push`{% endraw %} 메서드를 사용하여 요소를 스택에 추가합니다.
   {% raw %}```java
   stack.push(1);
   stack.push(2);
   stack.push(3);
   ```{% endraw %}

4. **요소 제거 및 반환 (pop):**
   {% raw %}`pop`{% endraw %} 메서드를 사용하여 스택의 맨 위 요소를 제거하고 반환합니다.
   {% raw %}```java
   int poppedElement = stack.pop();
   System.out.println("Popped element: " + poppedElement);
   ```{% endraw %}

5. **맨 위 요소 확인 (peek):**
   {% raw %}`peek`{% endraw %} 메서드를 사용하여 스택의 맨 위 요소를 확인할 수 있습니다.
   {% raw %}```java
   int topElement = stack.peek();
   System.out.println("Top element: " + topElement);
   ```{% endraw %}

6. **스택이 비어 있는지 확인:**
   {% raw %}`isEmpty`{% endraw %} 메서드를 사용하여 스택이 비어 있는지 확인할 수 있습니다.
   {% raw %}```java
   if (stack.isEmpty()) {
       System.out.println("Stack is empty");
   } else {
       System.out.println("Stack is not empty");
   }
   ```{% endraw %}

7. **스택 크기 확인:**
   {% raw %}`size`{% endraw %} 메서드를 사용하여 스택의 크기를 확인할 수 있습니다.
   {% raw %}```java
   int stackSize = stack.size();
   System.out.println("Stack size: " + stackSize);
   ```{% endraw %}

이것은 {% raw %}`Stack`{% endraw %} 클래스의 간단한 사용 예제입니다. 스택은 주로 임시 데이터를 저장하거나 역순으로 데이터를 처리할 때 유용합니다.

