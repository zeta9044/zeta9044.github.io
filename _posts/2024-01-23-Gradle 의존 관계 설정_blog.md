---
layout: post
title: "Gradle 의존 관계 설정"
date: 2024-01-23
tags: [설정, 관계, Gradle, 의존]
categories: blog
---

## 사용자 입력:
> gradle task X  안에 맨 나중에 다른 Task Y를 실행하려면?

## ChatGPT 응답:
> Gradle에서 특정 Task X가 실행된 후에 다른 Task Y를 실행하려면, 다음과 같이 `dependsOn` 키워드를 사용하여 의존 관계를 정의할 수 있습니다. 아래는 간단한 예제입니다:

```groovy
task X {
    doLast {
        // Task X의 실행 내용
        println "Task X 실행됨"
    }
}

task Y {
    doLast {
        // Task Y의 실행 내용
        println "Task Y 실행됨"
    }
}

// Task Y를 Task X의 종속성으로 설정
X.dependsOn Y
```

위의 예제에서는 `task X`와 `task Y`가 정의되어 있습니다. 그리고 `X.dependsOn Y`를 사용하여 Task X가 실행되기 전에 Task Y가 먼저 실행되도록 설정했습니다. 이렇게 하면 Task X가 실행되면 자동으로 Task Y도 실행됩니다.

또는, 다음과 같이 `dependsOn` 메서드를 사용하여 동적으로 의존 관계를 추가할 수도 있습니다:

```groovy
task X {
    doLast {
        // Task X의 실행 내용
        println "Task X 실행됨"
    }
}

task Y {
    doLast {
        // Task Y의 실행 내용
        println "Task Y 실행됨"
    }
}

// Task X가 실행될 때마다 Task Y가 실행되도록 설정
X.doLast {
    Y.execute()
}
```

이렇게 하면 Task X가 실행될 때마다 Task Y가 실행되도록 설정됩니다.

## 사용자 입력:
> gradle 6.9이상의 문법으로 만들어.

## ChatGPT 응답:
> Gradle 6.9 이상에서는 Kotlin DSL이 기본 스크립트 언어로 사용되고 있습니다. Kotlin DSL을 사용하여 위의 예제를 작성하면 다음과 같습니다:

```kotlin
tasks.register("X") {
    doLast {
        // Task X의 실행 내용
        println("Task X 실행됨")
    }
}

tasks.register("Y") {
    doLast {
        // Task Y의 실행 내용
        println("Task Y 실행됨")
    }
}

// Task X가 실행될 때마다 Task Y가 실행되도록 설정
tasks.named("X").configure {
    finalizedBy("Y")
}
```

위의 예제에서는 `tasks.register`를 사용하여 Task X와 Task Y를 등록하고, `finalizedBy` 메서드를 사용하여 Task X가 종료될 때 Task Y가 실행되도록 설정했습니다.

또는 다음과 같이 의존 관계를 설정할 수도 있습니다:

```kotlin
tasks.register("X") {
    doLast {
        // Task X의 실행 내용
        println("Task X 실행됨")
    }
}

tasks.register("Y") {
    dependsOn("X")
    doLast {
        // Task Y의 실행 내용
        println("Task Y 실행됨")
    }
}
```

이 예제에서는 `dependsOn` 메서드를 사용하여 Task Y가 Task X에 의존하도록 설정했습니다. Task X가 실행되면 자동으로 Task Y도 실행됩니다.

