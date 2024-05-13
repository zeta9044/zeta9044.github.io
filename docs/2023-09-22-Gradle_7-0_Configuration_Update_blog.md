---
layout: post
title: "Gradle 7.0 Configuration Update"
date: 2023-09-22
tags: [Configuration, Gradle, Update]
categories: blog
---

## 사용자 입력:
> configurations {
    jar.archiveName = 'lia-ds-cosec.jar'
} 을 gradle 7.0이상에서 사용하는 걸로 바꿔봐.

## ChatGPT 응답:
> `jar.archiveName` 속성은 Gradle 7.0 이상에서는 사용되지 않습니다. 대신 `archiveFileName` 속성을 사용해야 합니다. 

다음은 Gradle 7.0 이상의 방식으로 설정을 수정한 것입니다:

```gradle
tasks.named('jar') {
    archiveFileName.set('lia-ds-cosec.jar')
}
```

이렇게 수정하면 Gradle 7.0 이상 버전에서 잘 동작할 것입니다.

## 사용자 입력:
> 위 작업은 jar파일명을 lia-ds-cosec.jar으로 만드는 작업인가?

## ChatGPT 응답:
> 네, 맞습니다. 

Gradle에서 `jar` 태스크는 프로젝트의 코드를 컴파일하고 JAR 파일로 패키징하는 작업을 수행합니다. 제시된 설정은 이 `jar` 태스크의 결과로 생성되는 JAR 파일의 이름을 `lia-ds-cosec.jar`로 설정하는 것입니다.

즉, Gradle 빌드를 실행하면 `build/libs` 디렉터리 안에 `lia-ds-cosec.jar`라는 이름의 JAR 파일이 생성됩니다.

## 사용자 입력:
> configurations {
    jar.archiveFileName= 'lia-ds-cosec.jar'
} 이렇게는 안돼?

## ChatGPT 응답:
> 아니요, 그 방식으로는 작동하지 않습니다.

`archiveFileName`는 `jar` 태스크의 속성입니다. 따라서 `configurations` 블록 내에서 직접 설정할 수 없습니다. 대신 `tasks.named('jar')` 블록을 사용하여 `jar` 태스크의 속성을 설정해야 합니다.

따라서 올바른 방법은:

```gradle
tasks.named('jar') {
    archiveFileName.set('lia-ds-cosec.jar')
}
```

이렇게 해야 `jar` 태스크가 생성하는 JAR 파일의 이름을 `lia-ds-cosec.jar`로 설정할 수 있습니다.

