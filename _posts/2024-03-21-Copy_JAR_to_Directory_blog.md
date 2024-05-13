---
layout: post
title: "Copy JAR to Directory"
date: 2024-03-21
tags: [Directory, to, Copy, JAR]
categories: blog
---

## 사용자 입력:
> plugins {
    id 'java-library'
    id 'antlr'
}


group = 'com.datastreams.antlr4'
version = 'unspecified'

repositories {
    mavenCentral()
}


dependencies {
    implementation project(':aiworks')

    antlr 'org.antlr:antlr4:4.13.1'  // ANTLR4 버전

    //  javax.annotation.Nonnull
    implementation 'com.google.code.findbugs:jsr305:3.0.2'

    // https://mvnrepository.com/artifact/net.sf.jopt-simple/jopt-simple
    implementation 'net.sf.jopt-simple:jopt-simple:6.0-alpha-3'

    // Java Semantic Versioning Library
    implementation 'com.github.zafarkhaja:java-semver:0.9.0'

    // ant
    implementation 'org.apache.ant:ant:1.10.13'

    // json
    testImplementation 'com.google.guava:guava:32.0.1-jre'

    // char detect
    implementation 'com.github.albfernandez:juniversalchardet:2.4.0'
}

test {
    useJUnitPlatform()
}  이 스크립트를 실행하면, 생기는 jar를 aiworks/lib/system 디렉토리로 copy하는 거 추가해줘.

## ChatGPT 응답:
> 아래의 Gradle 스크립트에는 `jar` 태스크를 확장하여 빌드된 JAR 파일을 `aiworks/lib/system` 디렉토리로 복사하는 코드가 추가되었습니다. 이것은 `copyJar` 태스크를 생성하여 빌드되는 JAR 파일을 해당 디렉토리로 복사합니다.

```groovy
plugins {
    id 'java-library'
    id 'antlr'
}

group = 'com.datastreams.antlr4'
version = 'unspecified'

repositories {
    mavenCentral()
}

dependencies {
    implementation project(':aiworks')

    antlr 'org.antlr:antlr4:4.13.1'  // ANTLR4 버전

    //  javax.annotation.Nonnull
    implementation 'com.google.code.findbugs:jsr305:3.0.2'

    // https://mvnrepository.com/artifact/net.sf.jopt-simple/jopt-simple
    implementation 'net.sf.jopt-simple:jopt-simple:6.0-alpha-3'

    // Java Semantic Versioning Library
    implementation 'com.github.zafarkhaja:java-semver:0.9.0'

    // ant
    implementation 'org.apache.ant:ant:1.10.13'

    // json
    testImplementation 'com.google.guava:guava:32.0.1-jre'

    // char detect
    implementation 'com.github.albfernandez:juniversalchardet:2.4.0'
}

test {
    useJUnitPlatform()
}

// 빌드된 JAR 파일을 aiworks/lib/system 디렉토리로 복사하는 태스크 추가
task copyJar(type: Copy) {
    from jar
    into 'aiworks/lib/system'
}

// 빌드 직전에 copyJar 태스크 실행
build.dependsOn(copyJar)
```

위의 스크립트를 실행하면, `./gradlew build` 명령을 실행했을 때 빌드된 JAR 파일이 `aiworks/lib/system` 디렉토리로 복사됩니다.

## 사용자 입력:
> // 빌드된 JAR 파일을 aiworks/lib/system 디렉토리로 복사하는 태스크 추가
task copyJar(type: Copy) {
    from jar
    into '../aiworks/lib/system'
}

// 빌드 직전에 copyJar 태스크 실행
build.dependsOn(copyJar)
빌드가 다 된 뒤에 생성된 jar를 옮기는 거야.

## ChatGPT 응답:
> 알겠습니다. 아래의 코드는 빌드가 완료된 후에 JAR 파일을 `aiworks/lib/system` 디렉토리로 복사하는 방식으로 수정되었습니다.

```groovy
// 빌드된 JAR 파일을 aiworks/lib/system 디렉토리로 복사하는 태스크 추가
task copyJar(type: Copy) {
    dependsOn jar // 빌드된 jar 파일에 의존
    from jar // 빌드된 jar 파일을 소스로 설정
    into '../aiworks/lib/system' // 복사할 디렉토리 설정
}

// 빌드가 완료된 후에 copyJar 태스크 실행
gradle.buildFinished {
    tasks.copyJar.execute()
}
```

위의 코드를 실행하면, 빌드가 완료된 후에 JAR 파일이 `aiworks/lib/system` 디렉토리로 복사됩니다.

## 사용자 입력:
> gradle 6.8.3에 맞는 걸로

## ChatGPT 응답:
> Gradle 6.8.3에 맞게 코드를 수정하겠습니다. 아래는 Gradle 6.8.3에 맞는 코드입니다.

```groovy
// 빌드된 JAR 파일을 aiworks/lib/system 디렉토리로 복사하는 태스크 추가
task copyJar(type: Copy) {
    dependsOn jar // 빌드된 jar 파일에 의존
    from jar // 빌드된 jar 파일을 소스로 설정
    into '../aiworks/lib/system' // 복사할 디렉토리 설정
}

// 빌드가 완료된 후에 copyJar 태스크 실행
gradle.buildFinished {
    tasks.copyJar.execute()
}
```

이렇게 하면 Gradle 6.8.3에서도 원하는 기능을 수행할 수 있습니다.

