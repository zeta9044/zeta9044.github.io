---
layout: post
title: "Gradle 작업 그룹 설정"
date: 2023-09-21
tags: [작업, 설정, Gradle, 그룹]
categories: blog
---

## 사용자 입력:
> task coreJar(type: Jar) {
    dependsOn compileJava
    manifest {
        attributes('Implementation-Title': 'com.ids.core',
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
    archiveName = "lia-ds-core.jar"
    destinationDir = file("${buildDir}/libs")
    
    from(sourceSets.main.output) {  
        include "com/ids/core/**/*.class", "com/ids/sql/**/*.class", "com/ids/startup/**/*.class", "com/ids/exception/**/*.class", "com/ids/xml/**/*.class", "com/ids/license/**/*.class", "com/ids/common/**/*.class"
    }  
} 이런 gradle 스크립트가 있는데, gradle의 build그룹에 넣으려면?

## ChatGPT 응답:
> Gradle에서 `build` 그룹에 특정 작업을 추가하려면 `group` 속성을 사용하여 해당 작업에 그룹을 지정하면 됩니다.

위의 `coreJar` 작업을 `build` 그룹에 추가하려면, 다음과 같이 `group` 속성을 추가하면 됩니다:

```groovy
task coreJar(type: Jar) {
    group = 'build'  // 이 줄을 추가하세요.
    
    dependsOn compileJava
    manifest {
        attributes('Implementation-Title': 'com.ids.core',
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
    archiveName = "lia-ds-core.jar"
    destinationDir = file("${buildDir}/libs")
    
    from(sourceSets.main.output) {  
        include "com/ids/core/**/*.class", "com/ids/sql/**/*.class", "com/ids/startup/**/*.class", "com/ids/exception/**/*.class", "com/ids/xml/**/*.class", "com/ids/license/**/*.class", "com/ids/common/**/*.class"
    }  
}
```

이렇게 하면 `coreJar` 작업이 `build` 그룹에 포함되며, `gradle tasks` 명령어를 실행할 때 `build` 그룹 아래에 나타납니다.

