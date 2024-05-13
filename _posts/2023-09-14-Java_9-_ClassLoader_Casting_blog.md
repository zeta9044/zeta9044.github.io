---
layout: post
title: "Java 9+ ClassLoader Casting"
date: 2023-09-14
tags: [ClassLoader, Java, Casting]
categories: blog
---

## 사용자 입력:
> Exception in thread "main" java.lang.ClassCastException: class jdk.internal.loader.ClassLoaders$AppClassLoader cannot be cast to class java.net.URLClassLoader (jdk.internal.loader.ClassLoaders$AppClassLoader and java.net.URLClassLoader are in module java.base of loader 'bootstrap')

## ChatGPT 응답:
> The exception you've encountered is typically seen when someone tries to cast the system class loader to a `URLClassLoader` in Java 9 or later versions. The class loader hierarchy and implementation changed starting from Java 9 with the introduction of the module system (JPMS - Java Platform Module System).

Before Java 9, the application class loader was an instance of `URLClassLoader`. Thus, a common pattern in older code was:

```java
URLClassLoader classLoader = (URLClassLoader) ClassLoader.getSystemClassLoader();
```

However, in Java 9 and later, the system class loader is not an instance of `URLClassLoader`. Instead, it's an instance of `jdk.internal.loader.ClassLoaders.AppClassLoader`. This change was made to support the module system, and as a result, directly casting the system class loader to `URLClassLoader` will throw a `ClassCastException` as you've experienced.

To resolve this, you'll need to refactor your code. The exact solution will depend on what you're trying to achieve. If you can provide more details about your specific use case, I can guide you further. If you are only trying to get the URLs from the class loader, note that in many cases, this might not be possible directly with the system class loader in Java 9+. 

If you are developing a library or an application that needs to be compatible with both pre-Java 9 and Java 9+ versions, you will need to implement a conditional approach based on the Java version.

## 사용자 입력:
> 한글로 설명해 줄래?

## ChatGPT 응답:
> 물론이죠.

Java 9 이후 버전에서 보게 되는 해당 예외는 시스템 클래스 로더를 `URLClassLoader`로 캐스팅하려고 할 때 주로 발생합니다. Java 9에서 모듈 시스템(JPMS - Java Platform Module System)의 도입으로 클래스 로더의 계층 구조와 구현이 변경되었습니다.

Java 9 이전에는 응용 프로그램 클래스 로더가 `URLClassLoader`의 인스턴스였습니다. 따라서 이전 코드에서는 다음과 같은 패턴이 자주 사용되었습니다:

```java
URLClassLoader classLoader = (URLClassLoader) ClassLoader.getSystemClassLoader();
```

그러나 Java 9 이후 시스템 클래스 로더는 `URLClassLoader`의 인스턴스가 아닙니다. 대신 `jdk.internal.loader.ClassLoaders.AppClassLoader`의 인스턴스입니다. 이러한 변경은 모듈 시스템을 지원하기 위해 이루어졌으므로, Java 9 이후 버전에서 시스템 클래스 로더를 직접 `URLClassLoader`로 캐스팅하면 `ClassCastException`이 발생하게 됩니다.

이 문제를 해결하려면 코드를 리팩토링해야 합니다. 구체적인 해결 방법은 당신이 달성하려는 목표에 따라 다를 것입니다. 클래스 로더에서 URL을 가져오려고 하는 경우, Java 9+에서 시스템 클래스 로더로 직접 URL을 가져올 수 없다는 점을 유의해야 합니다.

Java 9 이전 버전과 Java 9+ 버전 모두와 호환되는 라이브러리나 응용 프로그램을 개발하려는 경우, Java 버전에 따라 조건부 접근 방식을 구현해야 합니다.

## 사용자 입력:
> Springboot를 gradle bootRun을 실행할 때 발생되는 에러인데...실행하는 jdk는 java11이야.

## ChatGPT 응답:
> Spring Boot와 Gradle을 사용하면서 Java 11에서 해당 에러가 발생한 경우, 일반적으로 두 가지 주요 원인이 있습니다:

1. **의존성 문제**: 프로젝트의 의존성 중 하나가 Java 9 이전의 클래스 로더 구조에 의존하는 코드를 포함하고 있을 수 있습니다. 이러한 의존성을 업데이트하거나 필요한 경우 적절한 대체 의존성을 찾아야 합니다.
2. **플러그인 문제**: Gradle 프로젝트에서 사용하는 플러그인 중 하나가 Java 9 이전의 클래스 로더 구조를 기대하고 있을 수 있습니다. 이런 경우 해당 플러그인의 최신 버전을 사용하는지 확인하고, 필요한 경우 업데이트 해야 합니다.

해결 방법:

1. **의존성 업데이트**: `build.gradle` 파일에서 모든 의존성이 최신 버전인지 확인하세요. 이를 위해 Maven Repository나 해당 의존성의 공식 문서에서 최신 버전 정보를 확인할 수 있습니다.
2. **플러그인 업데이트**: 사용 중인 모든 Gradle 플러그인이 최신 버전인지 확인하세요.
3. **스택 트레이스 확인**: 위의 예외 외에도 다른 스택 트레이스 또는 로그 메시지가 있는지 확인하십시오. 이 정보는 문제의 원인을 더 정확하게 파악하는 데 도움이 될 수 있습니다.
4. **Java 버전 확인**: `gradle.properties` 파일에 `sourceCompatibility` 및 `targetCompatibility` 속성이 설정되어 있는지 확인하세요. Java 11로 설정되어야 합니다.

만약 위의 단계들로 문제를 해결할 수 없다면, `build.gradle` 파일의 주요 섹션(의존성, 플러그인, 기타 설정)을 공유해주시면 더 구체적인 도움을 드릴 수 있습니다.

## 사용자 입력:
> 해당 코딩은 java8기준으로 되어 있어.

## ChatGPT 응답:
> Java 8 기준으로 작성된 코드를 Java 11에서 실행하려고 할 때 위와 같은 문제가 발생할 수 있습니다. Java 9 이후의 버전에서 클래스 로더의 구조와 동작 방식이 바뀌었기 때문입니다.

Java 11에서 Java 8 기준의 코드를 실행하려면 다음 단계를 시도해 보세요:

1. **의존성 업데이트**: `build.gradle` 파일의 의존성들이 Java 11과 호환되는 버전인지 확인하세요. 호환되지 않는 라이브러리가 있을 경우, 해당 라이브러리의 Java 11 호환 버전을 찾아 업데이트해 주세요.

2. **플러그인 업데이트**: 사용 중인 Gradle 플러그인들 중 Java 9 이후 버전과 호환되지 않는 것이 있을 수 있습니다. 이런 플러그인들을 최신 버전으로 업데이트하세요.

3. **소스 및 대상 호환성**: `build.gradle` 파일에 다음과 같이 Java 8 호환성을 설정하세요:
    ```gradle
    sourceCompatibility = '1.8'
    targetCompatibility = '1.8'
    ```

4. **JVM 옵션**: Java 8과 호환성을 유지하면서 Java 11에서 실행하려면, JVM 옵션을 조정할 필요가 있을 수 있습니다. 예를 들어, 모듈 시스템과 관련된 경고나 에러를 피하기 위해 `--add-opens`, `--add-modules` 등의 JVM 플래그를 설정할 수 있습니다.

5. **에러 로그 확인**: 에러 로그나 스택 트레이스에서 문제의 원인이 될 수 있는 구체적인 클래스나 라이브러리를 확인하십시오. 이 정보는 문제의 원인을 좁히는 데 도움이 될 수 있습니다.

만약 위의 조치들로도 문제가 해결되지 않는다면, `build.gradle` 파일의 일부 또는 관련 에러 로그를 공유해 주시면 더 구체적인 해결 방법을 제시해 드릴 수 있습니다.

## 사용자 입력:
> JVM옵션 조정 방법 알려줄래.

## ChatGPT 응답:
> 물론입니다. Java 11에서 Java 8로 작성된 코드를 실행할 때 JVM 옵션을 조정하는 것은 종종 필요한 작업입니다. 아래는 JVM 옵션을 조정하는 몇 가지 일반적인 방법과 그 예시입니다:

1. **모듈 접근성 허용**: Java 9 이후의 모듈화 시스템에서 일부 내부 API는 기본적으로 숨겨져 있습니다. 이런 API에 접근하려면 `--add-opens` 옵션을 사용해야 합니다.
   ```
   --add-opens java.base/java.lang=ALL-UNNAMED
   ```

2. **특정 모듈 추가**: 필요한 모듈이 기본적으로 포함되어 있지 않은 경우 `--add-modules` 옵션을 사용하여 추가할 수 있습니다.
   ```
   --add-modules module.name
   ```

3. **경고 무시**: `--add-opens`나 `--add-exports`와 같은 옵션을 사용하면 경고 메시지가 출력될 수 있습니다. 이러한 경고를 무시하려면 `--no-module-warnings` 옵션을 사용하세요.

4. **기타**: Java 9 이후로 변경된 기본 설정이나 동작을 무시하려면 다양한 옵션들을 사용할 수 있습니다. 예를 들면, `--permit-illegal-access` 옵션은 초기 버전의 Java 9에서 사용되었으나 이후 버전에서는 사용되지 않습니다.

Gradle의 `bootRun` 작업에 JVM 옵션을 추가하려면, `build.gradle` 파일에 다음과 같이 추가합니다:

```gradle
bootRun {
    jvmArgs(
        '--add-opens', 'java.base/java.lang=ALL-UNNAMED',
        '--add-modules', 'module.name'
        // ... 기타 JVM 옵션들
    )
}
```

필요한 JVM 옵션을 정확하게 알려주시면, 좀 더 구체적으로 도와드릴 수 있습니다. 또한, 스택 트레이스나 에러 로그에서 어떤 내부 API나 모듈이 문제를 일으키는지 알 수 있습니다. 이 정보를 통해 필요한 JVM 옵션을 결정할 수 있습니다.

## 사용자 입력:
> 위의 옵션이 java8로 실행할 때도 문제 없나?

## ChatGPT 응답:
> Java 8에서는 Java 9 이후에 도입된 모듈 시스템 (JPMS)이 없기 때문에, `--add-opens`, `--add-modules`와 같은 옵션들은 인식되지 않습니다. 따라서 Java 8의 JVM에서 이러한 옵션을 사용하면 에러가 발생합니다.

Java 8과 Java 11 둘 다에서 코드를 실행할 계획이라면, JVM 버전에 따라 조건부로 옵션을 설정하는 것이 좋습니다. Gradle에서는 이를 위해 Groovy 스크립트를 사용하여 조건부 로직을 작성할 수 있습니다.

예를 들면:

```gradle
bootRun {
    if (JavaVersion.current().isJava9Compatible()) {
        jvmArgs(
            '--add-opens', 'java.base/java.lang=ALL-UNNAMED',
            '--add-modules', 'module.name'
            // ... 기타 Java 9+ 전용 JVM 옵션들
        )
    }
    // Java 8에 대한 기타 설정들
}
```

위의 스크립트는 현재 JVM 버전이 Java 9 이상인 경우에만 해당 JVM 옵션을 설정하도록 합니다. 이렇게 하면 Java 8에서도 문제 없이 실행됩니다.

