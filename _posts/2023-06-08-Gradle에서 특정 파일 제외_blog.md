---
layout: post
title: "Gradle에서 특정 파일 제외"
date: 2023-06-08
tags: [파일, 특정, 제외]
categories: blog
---

## 사용자 입력:
> gradle에서  jar를 생성하는 태스크를 실행할 때, 특정파일을 제외하는 방법은?

## ChatGPT 응답:
> Gradle에서 JAR를 생성하는 태스크를 실행할 때 특정 파일을 제외하려면 `exclude` 메서드를 사용하여 제외 패턴을 지정해야 합니다. 다음은 Gradle에서 특정 파일을 제외하는 방법의 예시입니다.

```groovy
task createJar(type: Jar) {
    // JAR 파일에 포함할 디렉토리 또는 파일을 설정합니다.
    from 'src/main/java'
    
    // 제외할 디렉토리 또는 파일을 설정합니다.
    exclude '**/excluded_file.txt'
    
    // 생성된 JAR 파일의 이름을 설정합니다.
    archiveName = 'myjar.jar'
    
    // JAR 파일이 저장될 경로를 설정합니다.
    destinationDirectory = file('build/libs')
}
```

위 예시에서 `from` 메서드를 사용하여 JAR 파일에 포함할 디렉토리 또는 파일을 지정합니다. `exclude` 메서드를 사용하여 제외할 디렉토리 또는 파일을 패턴으로 지정할 수 있습니다. 위 예시에서는 `**/excluded_file.txt` 패턴으로 `src/main/java` 디렉토리 내부의 `excluded_file.txt` 파일을 제외합니다.

마지막으로, `archiveName` 속성을 사용하여 생성된 JAR 파일의 이름을 설정하고, `destinationDirectory` 속성을 사용하여 JAR 파일이 저장될 경로를 설정합니다. 위 예시에서는 `myjar.jar` 파일이 `build/libs` 디렉토리에 생성됩니다.

위와 같이 `exclude` 메서드를 사용하여 JAR 생성 태스크에서 특정 파일을 제외할 수 있습니다.

## 사용자 입력:
> tasks.named('jar') {
    manifest {
        attributes('Implementation-Title': project.name,
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
}  이 그래들 태스크에서 **/*.jjt를 제외하도록 해줘.

## ChatGPT 응답:
> 아래와 같이 `exclude` 메서드를 사용하여 `**/*.jjt` 파일을 제외할 수 있습니다.

```groovy
tasks.named('jar') {
    exclude('**/*.jjt')
    manifest {
        attributes('Implementation-Title': project.name,
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
}
```

위 예시에서 `exclude('**/*.jjt')`를 추가하여 `jar` 태스크에서 `**/*.jjt` 파일을 제외하도록 설정합니다. 이렇게 하면 JAR 생성 시 해당 파일이 포함되지 않습니다. `exclude` 메서드는 해당 태스크에 적용되며, 특정 패턴에 해당하는 파일을 제외하는 데 사용됩니다.

