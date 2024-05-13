---
layout: post
title: "Copy Gradle Dependencies to Directory"
date: 2023-11-02
tags: [Dependencies, to, Directory, Copy, Gradle]
categories: blog
---

## 사용자 입력:
> gradle에서  dependency로 정의한 의존관계 jar를 특정디렉토리에 copy하려면?

## ChatGPT 응답:
> Gradle에서 의존관계(dependency)로 정의한 JAR 파일들을 특정 디렉토리로 복사하려면 `Copy` 태스크를 사용할 수 있습니다. 아래는 그 예시입니다:

```groovy
apply plugin: 'java' // or 'java-library'

task copyDependencies(type: Copy) {
    // 복사할 대상 디렉토리 설정
    into "$buildDir/libs"

    // 의존성에서 runtimeClasspath를 사용 (또는 다른 configuration)
    from configurations.runtimeClasspath
}

// 'build' 태스크가 실행될 때 'copyDependencies' 태스크도 같이 실행되도록 의존성 추가
build.dependsOn(copyDependencies)
```

위의 스크립트에서는 `runtimeClasspath` 설정을 사용하여 의존성들을 복사합니다. 이 설정은 런타임에 필요한 모든 의존성을 포함하고 있습니다. 만약 다른 의존성 설정(예: `compileClasspath`, `testRuntimeClasspath` 등)을 복사하려면 해당 설정으로 변경해주면 됩니다.

`$buildDir/libs`는 Gradle 빌드 디렉토리의 `libs` 하위 폴더를 나타내며, 여기에 의존성 JAR 파일들이 복사됩니다. 복사하고자 하는 다른 디렉토리 경로로 변경하실 수 있습니다.

`build.dependsOn(copyDependencies)`는 `build` 태스크가 실행될 때 `copyDependencies` 태스크도 함께 실행되도록 설정하는 부분입니다. 이 부분을 제거하면 `copyDependencies` 태스크를 명시적으로 실행해야만 JAR 파일들이 복사됩니다.

실제 사용 시에는 `build.gradle` 파일에 해당 스크립트를 추가하고 Gradle 태스크를 실행하면 됩니다. 예를 들어, 커맨드 라인에서 다음과 같이 실행할 수 있습니다:

```shell
gradle copyDependencies
```

또는 복사 태스크를 `build`에 의존하게 했다면 단순히:

```shell
gradle build
```

위의 예시는 Groovy 기반의 구문을 사용합니다. 만약 Kotlin DSL을 사용하는 `build.gradle.kts` 파일을 사용하고 있다면, 적절한 Kotlin 구문으로 변경해주어야 합니다.

## 사용자 입력:
> build.dependsOn(copyDependencies) 대신 특정 task에 dependon 하는 방법

## ChatGPT 응답:
> 특정 태스크가 실행되기 전에 의존성을 복사하고 싶다면 해당 태스크에 `dependsOn`을 추가할 수 있습니다. 예를 들어, `jar` 태스크가 실행되기 전에 의존성을 복사하고 싶다면 `build.gradle`에 다음과 같이 작성할 수 있습니다:

```groovy
task copyDependencies(type: Copy) {
    into "$buildDir/libs"
    from configurations.runtimeClasspath
}

// 'jar' 태스크가 실행되기 전에 'copyDependencies' 태스크가 실행되도록 설정
jar.dependsOn(copyDependencies)
```

위의 예시에서는 `jar` 태스크 실행 전에 `copyDependencies` 태스크가 실행되도록 설정하였습니다. `jar` 대신에 다른 태스크 이름을 넣어서, 다른 태스크에 대한 의존성을 설정할 수도 있습니다.

만약 여러 태스크에 대해 의존성을 설정하고 싶다면, 각 태스크에 대해 `dependsOn`을 추가하면 됩니다:

```groovy
// 'test' 태스크와 'jar' 태스크 둘 다 'copyDependencies'를 실행하도록 설정
test.dependsOn(copyDependencies)
jar.dependsOn(copyDependencies)
```

Gradle의 강력한 기능 중 하나는 특정 태스크가 수행되기 전에 여러 태스크들이 수행되도록 의존성을 설정할 수 있다는 점입니다. 위의 설정을 통해 빌드 프로세스를 세밀하게 제어할 수 있습니다.

또한, 특정 조건 하에서만 의존성 복사 태스크를 실행하도록 만드는 것도 가능합니다. 예를 들어, `copyDependencies` 태스크를 프로젝트의 다른 속성이나 조건에 따라 실행하도록 구성할 수 있습니다. 

이 모든 설정은 프로젝트의 필요에 맞게 조정할 수 있으며, Gradle의 유연성을 최대한 활용하여 개발 환경을 최적화할 수 있습니다.

## 사용자 입력:
> 커스텀 태스크일때야.

## ChatGPT 응답:
> 커스텀 태스크가 특정 다른 태스크에 의존하도록 설정하는 것도 매우 간단합니다. 아래는 `myCustomTask`라는 커스텀 태스크를 생성하고, 이 태스크가 실행되기 전에 `copyDependencies` 태스크가 먼저 실행되도록 하는 방법을 보여줍니다:

```groovy
task copyDependencies(type: Copy) {
    into "$buildDir/libs"
    from configurations.runtimeClasspath
}

task myCustomTask {
    // 커스텀 태스크의 로직을 여기에 작성합니다.
    doLast {
        println 'Custom task is running...'
    }
}

// myCustomTask가 실행되기 전에 copyDependencies가 먼저 실행되도록 설정
myCustomTask.dependsOn copyDependencies
```

위의 예시에서는 `myCustomTask`라는 커스텀 태스크를 정의했고, 이 태스크가 실행되기 전에 `copyDependencies` 태스크를 실행하도록 설정했습니다. 

태스크를 선언할 때 `dependsOn`을 사용하면, 해당 태스크가 실행되기 전에 먼저 `dependsOn`에 지정된 태스크가 실행됩니다. 이렇게 설정하면 빌드 프로세스를 좀 더 유연하게 제어할 수 있습니다.

커스텀 태스크를 실행하려면 커맨드 라인에서 다음과 같이 입력합니다:

```shell
gradle myCustomTask
```

이 커맨드를 실행하면, Gradle은 먼저 `copyDependencies` 태스크를 실행한 다음, `myCustomTask` 태스크를 실행할 것입니다.

## 사용자 입력:
> build.dependsOn(copyDependencies) 이 문법을 gradle 6.8이상에 맞게 바꿔봐.

## ChatGPT 응답:
> Gradle 6.8 이상에서 `build.dependsOn(copyDependencies)` 구문을 사용하지 않고 동일한 결과를 얻으려면, task 간의 의존성을 `Task` 객체의 `dependsOn` 메서드를 사용하여 설정할 수 있습니다. 아래는 이를 어떻게 수행하는지에 대한 예제입니다:

```groovy
task copyDependencies(type: Copy) {
    into "$buildDir/libs"
    from configurations.runtimeClasspath
}

// build 태스크를 구성하고 copyDependencies에 대한 의존성을 추가합니다.
tasks.named('build').configure {
    dependsOn(copyDependencies)
}
```

위 예제에서 `tasks.named('build')`는 `build` 태스크를 참조하고, `configure` 블록은 이 태스크의 구성을 변경합니다. `dependsOn` 메서드는 `build` 태스크가 실행되기 전에 `copyDependencies` 태스크를 실행하도록 설정합니다.

이러한 변경은 특히 더 큰 빌드 파일에서 태스크의 설정을 변경할 때 유용하며, 태스크의 구성이 실제로 필요할 때까지 지연시키기 때문에 성능 최적화에도 도움이 됩니다.

Gradle의 구버전과 신버전에서는 내부 API와 구현 방식이 다를 수 있으므로, 어떤 변경이 적용되었는지 확인하려면 공식 문서를 참조하는 것이 좋습니다. 위의 코드는 Gradle 6.8 이상 버전에서도 작동해야 합니다.

