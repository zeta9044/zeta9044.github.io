---
layout: post
title: "Java Enum 활용 예제"
date: 2024-01-31
tags: [예제, 활용, Java, Enum]
categories: blog
---

## 사용자 입력:
> 자바에서 enum 활용 예제 여러가지.

## ChatGPT 응답:
> 자바(Java)에서 `enum` (열거형)을 사용하는 다양한 예제를 소개하겠습니다. `enum`은 고정된 상수들의 집합을 표현할 때 사용되며, 유지 보수와 코드 가독성 향상에 유용합니다.

### 기본 Enum 사용

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}

public class EnumTest {
    Day day;

    public EnumTest(Day day) {
        this.day = day;
    }

    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("Mondays are bad.");
                break;
            case FRIDAY:
                System.out.println("Fridays are better.");
                break;
            case SATURDAY: case SUNDAY:
                System.out.println("Weekends are best.");
                break;
            default:
                System.out.println("Midweek days are so-so.");
                break;
        }
    }

    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs();
        EnumTest fifthDay = new EnumTest(Day.FRIDAY);
        fifthDay.tellItLikeItIs();
        EnumTest sixthDay = new EnumTest(Day.SATURDAY);
        sixthDay.tellItLikeItIs();
        EnumTest seventhDay = new EnumTest(Day.SUNDAY);
        seventhDay.tellItLikeItIs();
    }
}
```

### Enum에 메소드 추가

```java
public enum Planet {
    MERCURY (3.303e+23, 2.4397e6),
    VENUS   (4.869e+24, 6.0518e6),
    EARTH   (5.976e+24, 6.37814e6),
    MARS    (6.421e+23, 3.3972e6),
    JUPITER (1.9e+27,   7.1492e7),
    SATURN  (5.688e+26, 6.0268e7),
    URANUS  (8.686e+25, 2.5559e7),
    NEPTUNE (1.024e+26, 2.4746e7);

    private final double mass;   // in kilograms
    private final double radius; // in meters

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    private double mass() { return mass; }
    private double radius() { return radius; }

    // Universal gravitational constant  (m3 kg-1 s-2)
    public static final double G = 6.67300E-11;

    double surfaceGravity() {
        return G * mass / (radius * radius);
    }
    double surfaceWeight(double otherMass) {
        return otherMass * surfaceGravity();
    }
}
```

### Enum과 Singleton 패턴

```java
public enum Singleton {
    INSTANCE;

    private Singleton() {
        // 초기화 코드
    }

    public void doSomething() {
        // 동작 코드
    }
}

public class SingletonTest {
    public static void main(String[] args) {
        Singleton singleton = Singleton.INSTANCE;
        singleton.doSomething();
    }
}
```

### Enum에서 값 가져오기

```java
public enum Level {
    LOW, MEDIUM, HIGH;

    public static Level getLevelFromString(String levelStr) {
        for (Level level : Level.values()) {
            if (level.name().equalsIgnoreCase(levelStr)) {
                return level;
            }
        }
        return null;
    }
}

public class LevelTest {
    public static void main(String[] args) {
        Level level = Level.getLevelFromString("HIGH");
        System.out.println(level); // 출력: HIGH
    }
}
```

이러한 예제들은 `enum`을 다양한 방법으로 활용하는 방법을 보여줍니다. `enum`은 코드를 더욱 체계적이고 관리하기 쉽게 만들어 줍니다.

