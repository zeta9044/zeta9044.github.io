---
layout: post
title: "Java8 Nonnull 애노테이션."
date: 2024-02-13
tags: [애, Nonnull, 션]
categories: blog
---

## 사용자 입력:
> 자바 애노테이션 java8이상에서 Nonull은 어떻게 표시?

## ChatGPT 응답:
> 자바 8 이상에서 `@NonNull` (또는 `@NotNull` 등) 애노테이션은 주로 매개변수, 필드, 메서드 반환 값 등이 `null`이 아니어야 함을 나타내는 데 사용됩니다. 이 애노테이션은 자바 표준에 내장되어 있지 않으며, 주로 다음과 같은 외부 라이브러리를 통해 제공됩니다:

1. **JSR-305 (`javax.annotation.Nonnull`)**: 이것은 자바 커뮤니티 프로세스(JCP)의 일부로 제안된 공통 애노테이션에 대한 스펙입니다. 하지만, JSR-305는 자바 플랫폼의 공식 일부가 되지는 않았습니다.

2. **Spring Framework (`org.springframework.lang.NonNull`)**: Spring 프레임워크는 자체 `@NonNull` 애노테이션을 제공하며, Spring 코드 베이스 내에서 널이 아닌 필드, 메서드 반환 값, 매개변수를 나타내는 데 사용됩니다.

3. **Lombok (`lombok.NonNull`)**: Lombok 라이브러리는 `@NonNull` 애노테이션을 제공하여, 객체 생성 시 널이 아닌 매개변수를 검증하는 코드를 자동으로 생성합니다.

4. **JetBrains Annotations (`org.jetbrains.annotations.NotNull`)**: IntelliJ IDEA와 같은 JetBrains 툴을 사용하는 개발자들을 위한 애노테이션 패키지입니다. 이 애노테이션은 매개변수, 필드, 메서드 반환 값이 널이 아님을 나타냅니다.

자바에서 `@NonNull` 또는 유사한 애노테이션을 사용하는 예는 다음과 같습니다:

```java
import org.jetbrains.annotations.NotNull;

public class MyClass {
    public void myMethod(@NotNull String myParam) {
        // myParam은 null이 아님을 가정합니다.
    }
}
```

애노테이션을 사용할 때는 해당 애노테이션이 포함된 라이브러리를 프로젝트의 의존성으로 추가해야 합니다. 예를 들어, Maven이나 Gradle을 사용하는 경우 `pom.xml`이나 `build.gradle` 파일에 필요한 의존성을 추가합니다.

## 사용자 입력:
> JSR-305 (javax.annotation.Nonnull)를 사용하려면?

## ChatGPT 응답:
> JSR-305 (`javax.annotation.Nonnull`) 애노테이션을 사용하기 위해서는 먼저 JSR-305 라이브러리를 프로젝트의 의존성으로 추가해야 합니다. Maven이나 Gradle과 같은 빌드 도구를 사용하는 경우, 프로젝트의 `pom.xml` 또는 `build.gradle` 파일에 해당 의존성을 추가함으로써 이를 수행할 수 있습니다.

### Maven을 사용하는 경우:

`pom.xml` 파일에 다음 의존성을 추가합니다:

```xml
<dependency>
    <groupId>com.google.code.findbugs</groupId>
    <artifactId>jsr305</artifactId>
    <version>3.0.2</version>
</dependency>
```

### Gradle을 사용하는 경우:

`build.gradle` 파일에 다음 의존성을 추가합니다:

```gradle
implementation 'com.google.code.findbugs:jsr305:3.0.2'
```

의존성을 추가한 후, `@Nonnull` 애노테이션을 사용하여 널이 아닌 매개변수, 필드, 메서드 반환 값을 명시적으로 표시할 수 있습니다. 예를 들어:

```java
import javax.annotation.Nonnull;

public class Example {
    public void printString(@Nonnull String str) {
        System.out.println(str);
    }
}
```

이 코드에서 `@Nonnull` 애노테이션은 `printString` 메서드의 `str` 매개변수가 널이 아니어야 함을 나타냅니다. 이를 통해 개발자와 도구들이 코드를 더 정확하게 이해하고, 널 관련 오류를 예방할 수 있습니다.

JSR-305는 널성 애노테이션에 대한 표준 제안이었지만, 자바 플랫폼의 공식 일부가 되지는 않았습니다. 그럼에도 불구하고, 많은 프로젝트와 도구에서 널성 계약을 명시하는 데 널리 사용됩니다.

