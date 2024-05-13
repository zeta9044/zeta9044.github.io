---
layout: post
title: "Gradle 작업 그룹 설정"
tags: [작업, Gradle, 설정, 그룹]
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
> Gradle에서 {% raw %}`build`{% endraw %} 그룹에 특정 작업을 추가하려면 {% raw %}`group`{% endraw %} 속성을 사용하여 해당 작업에 그룹을 지정하면 됩니다.

위의 {% raw %}`coreJar`{% endraw %} 작업을 {% raw %}`build`{% endraw %} 그룹에 추가하려면, 다음과 같이 {% raw %}`group`{% endraw %} 속성을 추가하면 됩니다:

{% raw %}```groovy
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
```{% endraw %}

이렇게 하면 {% raw %}`coreJar`{% endraw %} 작업이 {% raw %}`build`{% endraw %} 그룹에 포함되며, {% raw %}`gradle tasks`{% endraw %} 명령어를 실행할 때 {% raw %}`build`{% endraw %} 그룹 아래에 나타납니다.

