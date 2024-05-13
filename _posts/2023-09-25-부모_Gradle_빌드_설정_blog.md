---
layout: post
title: "부모 Gradle 빌드 설정"
date: 2023-09-25
tags: [부모, 빌드, 설정, Gradle]
categories: blog
---

## 사용자 입력:
> 멀티gradle java프로젝트에서, 자식프로젝트의 빌드를 진행할 수 있는 부모   build.gradle예시

## ChatGPT 응답:
> 멀티프로젝트 구성의 Gradle Java 프로젝트에서 부모 `build.gradle` 파일을 사용하여 자식 프로젝트의 빌드를 구성하려면 다음과 같은 방법을 사용할 수 있습니다:

1. 프로젝트 구조:

```
rootProject
|-- build.gradle (이 파일에 대한 예시를 제공합니다)
|-- settings.gradle
|-- childProject1
|   |-- build.gradle
|   |-- src
|-- childProject2
|   |-- build.gradle
|   |-- src
```

2. `settings.gradle`:

```groovy
rootProject.name = 'rootProject'
include 'childProject1', 'childProject2'
```

3. `rootProject/build.gradle` (부모 프로젝트의 `build.gradle`):

```groovy
allprojects {
    apply plugin: 'java'

    repositories {
        mavenCentral()
    }

    dependencies {
        testImplementation 'junit:junit:4.12'
    }

    tasks.withType(JavaCompile) {
        options.encoding = 'UTF-8'
    }
}

subprojects {
    version = '1.0.0'

    jar {
        manifest {
            attributes 'Implementation-Title': 'My Multi-Project',
                       'Implementation-Version': version
        }
    }
}

project(':childProject1') {
    dependencies {
        // 여기에 childProject1에 필요한 추가 종속성을 추가하십시오.
    }
}

project(':childProject2') {
    dependencies {
        // 여기에 childProject2에 필요한 추가 종속성을 추가하십시오.
    }
}
```

이 예시에서는 `allprojects` 블록을 사용하여 모든 프로젝트에 공통적으로 적용되는 설정을 정의하고 있습니다. 그런 다음 `subprojects` 블록을 사용하여 모든 자식 프로젝트에 공통적으로 적용되는 설정을 추가합니다.

마지막으로 `project(':childProject1')` 및 `project(':childProject2')` 블록을 사용하여 각 자식 프로젝트에 특정한 설정을 추가할 수 있습니다.

이 방식을 사용하면 부모 `build.gradle` 파일에서 여러 자식 프로젝트의 빌드를 중앙에서 관리할 수 있습니다.

## 사용자 입력:
> 부모 build.gradle에서 자식들의 각각 다른 task를 지정해서 실행하는 방법.

## ChatGPT 응답:
> 멀티프로젝트 구성에서 부모 `build.gradle` 파일에서 자식 프로젝트의 특정 태스크를 지정하여 실행하려면 다음과 같은 방법을 사용할 수 있습니다:

1. 프로젝트 구조:

```
rootProject
|-- build.gradle
|-- settings.gradle
|-- childProject1
|   |-- build.gradle
|   |-- src
|-- childProject2
|   |-- build.gradle
|   |-- src
```

2. 각 자식 프로젝트의 `build.gradle`에 특정 태스크를 정의합니다. 예를 들어, `childProject1`에 `customTask1`이라는 태스크를 추가하고, `childProject2`에 `customTask2`라는 태스크를 추가하겠습니다:

`childProject1/build.gradle`:

```groovy
task customTask1 {
    doLast {
        println "Executing customTask1 from childProject1"
    }
}
```

`childProject2/build.gradle`:

```groovy
task customTask2 {
    doLast {
        println "Executing customTask2 from childProject2"
    }
}
```

3. 부모 `build.gradle`에서 자식 프로젝트의 특정 태스크를 지정하여 실행합니다:

`rootProject/build.gradle`:

```groovy
task runChildTasks {
    dependsOn ':childProject1:customTask1', ':childProject2:customTask2'
}

// OR

task runChildTasks {
    doLast {
        ':childProject1:customTask1'.execute()
        ':childProject2:customTask2'.execute()
    }
}
```

이제 루트 프로젝트에서 `./gradlew runChildTasks` 명령을 실행하면 `childProject1`의 `customTask1`과 `childProject2`의 `customTask2` 태스크가 순차적으로 실행됩니다.

위의 예제는 간단한 방법을 보여주기 위한 것이며, 실제 프로젝트에서는 태스크간의 의존성, 실행 순서, 추가 로직 등을 고려하여 구성해야 할 수 있습니다.

## 사용자 입력:
> 다른 얘기인데,  java task중 jar를 다른 task로 override할 수 있나? 즉, 호출은 jar를 호출하는데, 실제 task는 다른 customtask를 실행하게 하는 것.

## ChatGPT 응답:
> 네, Gradle에서 기존의 태스크를 오버라이드하거나 치환하는 것은 가능합니다. 하지만 이는 주의해야 하는 방법입니다. 기존의 태스크를 완전히 대체하면 예상치 못한 문제가 발생할 수 있습니다.

기존의 `jar` 태스크를 오버라이드하고 싶다면, 먼저 해당 태스크를 삭제한 후 같은 이름의 새 태스크를 추가할 수 있습니다.

예를 들어:

```groovy
task replaceJar(type: Jar) {
    doLast {
        println "This is a custom task replacing the jar task."
        // 여기에 원하는 로직 추가
    }
}

tasks.remove('jar')
task jar(dependsOn: 'replaceJar') {
    doLast {
        println "Original jar task has been replaced!"
    }
}
```

위의 코드는 기존의 `jar` 태스크를 삭제하고, 그 자리에 새로운 `jar` 태스크를 추가합니다. 새로운 `jar` 태스크는 `replaceJar` 태스크에 의존하며, `replaceJar` 태스크는 원하는 로직을 포함하고 있습니다.

이렇게 하면 `./gradlew jar` 명령을 실행할 때 원래의 `jar` 태스크가 아닌 `replaceJar` 태스크가 실행됩니다.

그러나 이 방법은 권장되지 않습니다. 대신에 기존의 태스크를 확장하거나 수정하는 것이 더 안전하고 유지 관리하기 쉽습니다.

## 사용자 입력:
> 명령줄에서 각 자식 task를 실행하는 방법.

## ChatGPT 응답:
> 멀티프로젝트 구성에서 명령 줄을 사용하여 각 자식 프로젝트의 태스크를 실행하려면 `:` (콜론)을 사용하여 프로젝트 경로를 지정해야 합니다.

예를 들어, 위에서 언급한 프로젝트 구조를 사용한다면:

```
rootProject
|-- build.gradle
|-- settings.gradle
|-- childProject1
|   |-- build.gradle
|   |-- src
|-- childProject2
|   |-- build.gradle
|   |-- src
```

`childProject1`의 `customTask1` 태스크를 실행하려면:

```
./gradlew :childProject1:customTask1
```

`childProject2`의 `customTask2` 태스크를 실행하려면:

```
./gradlew :childProject2:customTask2
```

위의 예제에서 `:`는 프로젝트의 경로를 나타내며, 이를 통해 특정 서브 프로젝트의 태스크를 지정할 수 있습니다.

또한 여러 태스크를 동시에 실행하려면 각 태스크 이름을 공백으로 구분하여 지정하면 됩니다. 예를 들어, `childProject1`의 `customTask1`와 `childProject2`의 `customTask2`를 동시에 실행하려면:

```
./gradlew :childProject1:customTask1 :childProject2:customTask2
```

이렇게 하면 두 태스크가 순차적으로 실행됩니다.

## 사용자 입력:
> 위에는 자식의 task를 1개만 지정하는 거잖아. 여러개 지정은 어떻게 하나?

## ChatGPT 응답:
> 네, 여러 자식 프로젝트의 여러 태스크를 실행하려면 각 태스크를 공백으로 구분하여 명령 줄에 지정하면 됩니다.

예를 들어, `childProject1`에서 `taskA`, `taskB`를 실행하고 `childProject2`에서 `taskX`, `taskY`를 실행하려면 다음과 같이 지정할 수 있습니다:

```
./gradlew :childProject1:taskA :childProject1:taskB :childProject2:taskX :childProject2:taskY
```

이렇게 하면 지정된 순서대로 태스크가 실행됩니다. 

명령 줄에서 태스크 이름을 나열하면 Gradle은 해당 태스크를 지정된 순서대로 실행하며, 필요에 따라 각 태스크의 의존성도 고려하여 실행합니다.

