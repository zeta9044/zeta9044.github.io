---
layout: post
title: "Java 8에서 instanceof와 switch"
date: 2023-11-10
tags: [switch, Java]
categories: blog
---

## 사용자 입력:
> 자바8에서     instanceof 를 switch case문에 쓸 방법 없을까?

## ChatGPT 응답:
> Java 8에서 `instanceof`를 `switch`문과 함께 사용하는 직접적인 방법은 없습니다. `switch`문은 주로 열거형(enum), 문자열(String), 또는 기본 데이터 타입들과 함께 사용됩니다. 그러나 객체 타입을 확인하고 분기 처리하기 위해 `instanceof`와 `switch`문을 함께 사용하는 것은 지원되지 않습니다.

하지만, Java 8에서 이를 대체할 수 있는 몇 가지 방법이 있습니다:

1. **If-Else 문 사용**: `instanceof`를 사용하여 객체의 타입을 확인한 다음, 각 경우에 대해 if-else 블록을 사용하여 로직을 구현할 수 있습니다.

2. **디자인 패턴 사용**: 예를 들어, 방문자(Visitor) 패턴을 사용하여 다양한 타입에 대한 로직을 구현할 수 있습니다. 이 방법은 더 복잡하지만, 확장 가능하고 유지 관리가 용이합니다.

3. **Map을 이용한 전략 패턴**: 객체 타입을 키로 하고 해당 타입에 대한 로직을 구현한 함수를 값으로 하는 Map을 사용할 수 있습니다. 이 방법은 `switch`문과 유사한 구조를 제공하며, 유연하고 확장 가능합니다.

Java 14부터는 `instanceof`와 `switch`문을 함께 사용할 수 있는 패턴 매칭이 도입되었지만, 이는 Java 8에서는 사용할 수 없습니다. Java 8을 사용 중이라면 위의 방법들 중 하나를 고려해 볼 수 있습니다.

## 사용자 입력:
> 디자인 패턴 사용하는 걸로 보여줘.

## ChatGPT 응답:
> Java에서 객체 타입에 따라 다른 동작을 수행해야 할 때, 디자인 패턴 중 하나인 방문자(Visitor) 패턴을 사용할 수 있습니다. 이 패턴은 객체 구조와 처리 로직을 분리하여, 새로운 연산을 쉽게 추가할 수 있게 해줍니다. 예제로 간단한 방문자 패턴을 구현해 보겠습니다.

### 예제 시나리오

다음과 같은 시나리오를 가정해 봅시다: 여러 종류의 동물 객체가 있고, 이들에 대해 특정 행동을 수행하고 싶습니다. 예를 들어, `Dog`, `Cat`, `Bird` 클래스가 있으며, 각각에 대해 `speak`라는 행동을 수행하고자 합니다.

### 방문자 패턴 구현

1. **Element 인터페이스**: 처리될 객체들의 인터페이스입니다.

    ```java
    public interface Animal {
        void accept(AnimalVisitor visitor);
    }
    ```

2. **Concrete Element 클래스들**: 실제 객체들입니다.

    ```java
    public class Dog implements Animal {
        public void accept(AnimalVisitor visitor) {
            visitor.visitDog(this);
        }
    }

    public class Cat implements Animal {
        public void accept(AnimalVisitor visitor) {
            visitor.visitCat(this);
        }
    }

    public class Bird implements Animal {
        public void accept(AnimalVisitor visitor) {
            visitor.visitBird(this);
        }
    }
    ```

3. **Visitor 인터페이스**: 각 Element에 대한 방문자 메소드들을 정의합니다.

    ```java
    public interface AnimalVisitor {
        void visitDog(Dog dog);
        void visitCat(Cat cat);
        void visitBird(Bird bird);
    }
    ```

4. **Concrete Visitor 클래스**: 실제 로직을 구현합니다.

    ```java
    public class SpeakVisitor implements AnimalVisitor {
        public void visitDog(Dog dog) {
            System.out.println("Dog says: Woof!");
        }

        public void visitCat(Cat cat) {
            System.out.println("Cat says: Meow!");
        }

        public void visitBird(Bird bird) {
            System.out.println("Bird says: Tweet!");
        }
    }
    ```

5. **사용 예제**:

    ```java
    public class Main {
        public static void main(String[] args) {
            Animal dog = new Dog();
            Animal cat = new Cat();
            Animal bird = new Bird();

            AnimalVisitor speakVisitor = new SpeakVisitor();

            dog.accept(speakVisitor);
            cat.accept(speakVisitor);
            bird.accept(speakVisitor);
        }
    }
    ```

이 예제에서 `Animal` 객체들은 `accept` 메소드를 통해 `SpeakVisitor` 객체를 받습니다. `SpeakVisitor`는 `visitDog`, `visitCat`, `visitBird` 메소드를 통해 각 동물 타입에 대한 구체적인 로직을 수행합니다. 이 방식으로, 새로운 동물 타입이 추가되거나 새로운 행동(예를 들어 `EatVisitor`)이 필요한 경우, 기존 코드를 변경하지 않고 확장할 수 있습니다.

