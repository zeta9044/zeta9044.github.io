---
layout: post
title: "Copy Gradle Dependencies to Directory"
date: 2023-11-03
tags: [Dependencies, to, Directory, Copy, Gradle]
categories: blog
---

## 사용자 입력:
> gradle에서 실제 compile에 사용된 depency만 따로 별도의 디렉토리에 넣을 수는 없나?

## ChatGPT 응답:
> Gradle에서 실제 컴파일에 사용된 의존성(dependency)을 별도의 디렉토리로 복사하는 것은 가능합니다. 이를 수행하기 위해서는 Gradle 빌드 스크립트에서 작업(task)을 정의해야 합니다.

`dependencies` 블록에서 정의된 의존성 중 `implementation`, `api`, `compileOnly` 등 실제 컴파일에 필요한 의존성들을 대상으로 하여 `Copy` 타입의 작업을 만들어 실행할 수 있습니다.

아래는 컴파일에 사용된 의존성을 `libs/compile` 디렉토리에 복사하는 예시 작업입니다:

```groovy
task copyCompileDependencies(type: Copy) {
    // 컴파일에 사용된 의존성을 가져오기 위해 configurations.runtimeClasspath를 사용
    from configurations.compileClasspath
    into 'libs/compile'
}

// copyCompileDependencies 작업이 build 작업 이후에 실행되도록 함
build.finalizedBy(copyCompileDependencies)
```

이 스크립트 조각은 `build.gradle` 파일에 추가해야 합니다. `configurations.compileClasspath`를 사용하면 컴파일에 사용된 클래스패스에 있는 모든 아티팩트를 가져올 수 있습니다.

Gradle 7 이상에서는 `compile` 대신 `implementation` 또는 `api` 구성을 사용할 수 있으므로 위의 `from` 부분에서 `configurations.compileClasspath` 대신 `configurations.runtimeClasspath` 또는 적절한 구성 이름을 사용해야 할 수 있습니다.

빌드 스크립트를 수정한 후, 다음과 같이 Gradle 작업을 실행하면 복사 작업이 수행됩니다:

```sh
./gradlew build
```

이 커맨드는 `build` 작업을 실행한 후에 `copyCompileDependencies` 작업을 실행하여 컴파일에 사용된 의존성을 지정된 디렉토리에 복사합니다.

다만, 주의해야 할 점은 이 방법이 프로젝트의 실제 컴파일에 사용된 의존성만을 복사하는 것이 아니라, `compileClasspath` 구성에 포함된 모든 의존성을 복사한다는 것입니다. 따라서 컴파일 시점에 필요하지 않은 의존성까지 포함될 수 있습니다. 프로젝트의 실제 컴파일에만 사용된 의존성을 정확히 필터링하기 위해서는 추가적인 작업이 필요할 수 있습니다.

## 사용자 입력:
> 실제 의존성만 추출하고 싶어.

## ChatGPT 응답:
> 실제 컴파일에 사용된 의존성만 추출하고 싶다면, Gradle의 `configuration`을 사용하여 compile-time에 필요한 의존성을 가져올 수 있습니다. 하지만 Gradle 5.x 버전부터 `compile`과 `runtime` configuration이 deprecated 되었고, Gradle 6.x 이후부터는 `implementation`, `api`, `compileOnly`, `runtimeOnly` 등으로 변경되었습니다. 여기서 `compileOnly`는 컴파일 시간에만 필요하고 배포 시에는 필요하지 않은 의존성에 사용됩니다.

따라서 실제 컴파일에 사용된 의존성만을 추출하려면 `compileClasspath` (compileOnly를 포함) 또는 `runtimeClasspath` (컴파일과 런타임 의존성을 모두 포함) configuration 중 하나를 사용할 수 있습니다. `compileClasspath`는 특정 소스 세트에 대해 컴파일에 필요한 모든 요소를 포함합니다.

아래의 예시는 `compileClasspath`를 사용하여 컴파일에 사용된 의존성만 추출하고, 그 의존성들을 별도의 디렉토리(`libs/compile`)로 복사하는 작업입니다.

```groovy
task copyCompileDependencies(type: Copy) {
    from configurations.compileClasspath
    into 'libs/compile'
}
```

그리고 이 작업을 실행하려면, 다음과 같이 커맨드를 실행합니다:

```sh
./gradlew copyCompileDependencies
```

이렇게 하면 `libs/compile` 디렉토리에 컴파일에 실제로 사용된 의존성들이 복사됩니다.

만약 `api`나 `implementation`으로 선언된 의존성만을 추출하고 싶다면, 해당하는 구성(configuration)에 맞춰 `from`을 설정해야 합니다. 예를 들어, `api` 의존성만을 복사하고 싶다면 `configurations.api`를 사용할 수 있습니다. 하지만 이는 Gradle에서 직접적으로 제공하는 기능은 아니기 때문에, `api`와 `implementation`으로 구분하여 추출하는 것은 추가적인 스크립트 작성이 필요할 수 있습니다.

`compileOnly`와 `runtimeOnly`의 구분 없이 실제로 컴파일에 참여한 의존성만을 원한다면, 이러한 의존성을 추적하기 위한 별도의 로직을 작성해야 할 수도 있습니다. 이는 Gradle의 기본 기능으로는 제공되지 않으며, 의존성 해석 로직을 추가로 구현해야 하는 복잡한 작업입니다.

마지막으로, 어떤 의존성이 실제로 컴파일에 사용되었는지 정확히 알기 위해서는 Gradle의 의존성 해석 로그를 확인하거나, 빌드 스크립트 내에서 의존성 해석 과정을 더 세밀하게 제어해야 할 수도 있습니다.

## 사용자 입력:
> compileOnly와 runtimeOnly의 구분 없이 실제로 컴파일에 참여한 의존성만을 원해.

## ChatGPT 응답:
> 실제로 컴파일에 참여한 의존성만을 추출하고자 한다면, `compileClasspath` configuration을 사용해야 합니다. 이 configuration은 실제로 컴파일 시에 클래스 패스에 포함되는 의존성들을 포함하고 있습니다.

이러한 작업을 수행하기 위해서는 Gradle의 Task를 커스터마이징해야 합니다. 다음은 컴파일 시 클래스패스에 포함된 의존성들만을 특정 디렉토리로 복사하는 작업의 예시입니다:

```groovy
task copyCompileOnlyDependencies(type: Copy) {
    from configurations.compileClasspath
    into 'libs/compileOnly'
    
    // Do not include dependencies that are also part of runtime
    exclude { dependency ->
        configurations.runtimeClasspath.contains(dependency.file)
    }
}
```

`configurations.compileClasspath`를 사용하면 컴파일 시에 필요한 모든 의존성을 가져올 수 있으며, `exclude` 블록을 사용하여 런타임 클래스패스에도 포함되는 의존성을 제외하도록 설정합니다.

그러나 이 방법은 여전히 완벽하지 않을 수 있습니다. 왜냐하면 `compileClasspath`와 `runtimeClasspath`에는 각각의 configuration에 지정된 의존성 뿐만 아니라, 그 의존성들이 의존하는 하위 의존성들도 포함되기 때문입니다. 즉, 컴파일 시에만 사용되고 런타임 시에는 제외되어야 할 `compileOnly` 의존성이 `runtimeClasspath`에 포함될 가능성이 있습니다.

이 작업은 실제 빌드 과정에서 어떤 의존성들이 컴파일 시에 참여했는지를 정확히 추적하기 위한 것이며, `compileOnly`와 `runtimeOnly`를 구분하지 않고 실제 컴파일에 사용된 의존성만을 대상으로 합니다.

이 작업을 실행하려면, Gradle 작업을 다음과 같이 호출합니다:

```sh
./gradlew copyCompileOnlyDependencies
```

이렇게 하면 `libs/compileOnly` 디렉토리에 실제로 컴파일 시에 참여한 의존성들이 복사됩니다.

