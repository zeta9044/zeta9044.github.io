---
layout: post
title: "Java POJO 클래스 생성"
date: 2024-01-04
tags: [POJO, 클래스, Java, 생]
categories: blog
---

## 사용자 입력:
> java의 pojo 클래스를 만드는데, 메소드로 json문자열을 출력하는 함수를 추가하는 예제 보여줘.

## ChatGPT 응답:
> Java에서 POJO (Plain Old Java Object) 클래스를 만들고, 이 클래스에 JSON 문자열을 출력하는 메소드를 추가하는 예제를 보여드리겠습니다. 여기서는 `Gson` 라이브러리를 사용하여 객체를 JSON으로 변환합니다. `Gson`은 Google에서 제공하는 자바 라이브러리로, Java 객체를 JSON 표현으로 쉽게 변환할 수 있게 해줍니다.

먼저, POJO 클래스를 정의합니다. 예를 들어, 간단한 `Person` 클래스를 만들어 보겠습니다. 이 클래스는 이름(name)과 나이(age)라는 두 개의 필드를 가지고 있습니다.

그런 다음, 이 클래스에 `toJson`이라는 메소드를 추가하여, `Person` 객체를 JSON 문자열로 변환하는 기능을 구현합니다.

```java
import com.google.gson.Gson;

public class Person {
    private String name;
    private int age;

    // 생성자
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter와 Setter 메소드
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    // 객체를 JSON 문자열로 변환하는 메소드
    public String toJson() {
        Gson gson = new Gson();
        return gson.toJson(this);
    }
}

```

이제 `Person` 객체를 생성하고 `toJson` 메소드를 호출하여 JSON 문자열로 변환할 수 있습니다.

```java
public class Main {
    public static void main(String[] args) {
        Person person = new Person("John Doe", 30);
        String json = person.toJson();
        System.out.println(json);  // 출력 예: {"name":"John Doe","age":30}
    }
}
```

이 예제에서는 `Gson` 라이브러리를 사용했으므로, 프로젝트에 `Gson` 라이브러리를 추가해야 합니다. Maven이나 Gradle과 같은 빌드 도구를 사용하고 있다면, 의존성 관리 섹션에 `Gson` 라이브러리를 추가해야 합니다. 예를 들어, Maven을 사용하는 경우 `pom.xml` 파일에 다음과 같이 의존성을 추가할 수 있습니다.

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.8.6</version>
    </dependency>
</dependencies>
```

이렇게 하면 Java의 POJO 클래스에 JSON 문자열을 출력하는 메소드를 추가하는 방법에 대한 기본적인 예제를 확인할 수 있습니다.

## 사용자 입력:
> 보기 좋게 하는 기능도 추가해.,

## ChatGPT 응답:
> JSON 문자열을 보기 좋게 출력하려면 `Gson` 라이브러리의 `GsonBuilder` 클래스를 사용하여 `Gson` 인스턴스를 만들 때 포맷팅 옵션을 설정할 수 있습니다. `setPrettyPrinting()` 메소드를 사용하면 JSON 출력을 보기 좋게 들여쓰기와 줄바꿈을 적용하여 출력할 수 있습니다.

`Person` 클래스의 `toJson` 메소드를 수정하여 JSON 문자열을 보기 좋게 출력하는 기능을 추가해 보겠습니다.

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class Person {
    private String name;
    private int age;

    // 생성자, getter, setter 생략...

    // 객체를 보기 좋은 JSON 문자열로 변환하는 메소드
    public String toJsonPretty() {
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(this);
    }
}
```

이제 `toJsonPretty` 메소드를 사용하여 JSON 문자열을 보기 좋게 출력할 수 있습니다.

```java
public class Main {
    public static void main(String[] args) {
        Person person = new Person("John Doe", 30);
        String jsonPretty = person.toJsonPretty();
        System.out.println(jsonPretty);  // 보기 좋게 출력된 JSON
    }
}
```

이렇게 출력된 JSON은 들여쓰기와 줄바꿈이 적용되어 있어서 사람이 읽기에 더 편리합니다. 예를 들어, 위의 코드는 다음과 같은 형식으로 JSON을 출력할 것입니다.

```json
{
  "name": "John Doe",
  "age": 30
}
```

이 방법을 사용하면 JSON 문자열을 사용자에게 보다 친숙하고 읽기 쉬운 형태로 제공할 수 있습니다.

