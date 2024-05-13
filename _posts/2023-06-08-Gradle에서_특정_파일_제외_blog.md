---
layout: post
title: "Gradle에서 특정 파일 제외"
tags: [특정, 제외, 파일]
---

## 사용자 입력:
> gradle에서  jar를 생성하는 태스크를 실행할 때, 특정파일을 제외하는 방법은?

## ChatGPT 응답:
> Gradle에서 JAR를 생성하는 태스크를 실행할 때 특정 파일을 제외하려면 {% raw %}`exclude`{% endraw %} 메서드를 사용하여 제외 패턴을 지정해야 합니다. 다음은 Gradle에서 특정 파일을 제외하는 방법의 예시입니다.

{% raw %}```groovy
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
```{% endraw %}

위 예시에서 {% raw %}`from`{% endraw %} 메서드를 사용하여 JAR 파일에 포함할 디렉토리 또는 파일을 지정합니다. {% raw %}`exclude`{% endraw %} 메서드를 사용하여 제외할 디렉토리 또는 파일을 패턴으로 지정할 수 있습니다. 위 예시에서는 {% raw %}`**/excluded_file.txt`{% endraw %} 패턴으로 {% raw %}`src/main/java`{% endraw %} 디렉토리 내부의 {% raw %}`excluded_file.txt`{% endraw %} 파일을 제외합니다.

마지막으로, {% raw %}`archiveName`{% endraw %} 속성을 사용하여 생성된 JAR 파일의 이름을 설정하고, {% raw %}`destinationDirectory`{% endraw %} 속성을 사용하여 JAR 파일이 저장될 경로를 설정합니다. 위 예시에서는 {% raw %}`myjar.jar`{% endraw %} 파일이 {% raw %}`build/libs`{% endraw %} 디렉토리에 생성됩니다.

위와 같이 {% raw %}`exclude`{% endraw %} 메서드를 사용하여 JAR 생성 태스크에서 특정 파일을 제외할 수 있습니다.

## 사용자 입력:
> tasks.named('jar') {
    manifest {
        attributes('Implementation-Title': project.name,
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
}  이 그래들 태스크에서 **/*.jjt를 제외하도록 해줘.

## ChatGPT 응답:
> 아래와 같이 {% raw %}`exclude`{% endraw %} 메서드를 사용하여 {% raw %}`**/*.jjt`{% endraw %} 파일을 제외할 수 있습니다.

{% raw %}```groovy
tasks.named('jar') {
    exclude('**/*.jjt')
    manifest {
        attributes('Implementation-Title': project.name,
                   'Implementation-Vendor': 'DataStreams, Inc.',
                   'Implementation-Version': project.version)
    }
}
```{% endraw %}

위 예시에서 {% raw %}`exclude('**/*.jjt')`{% endraw %}를 추가하여 {% raw %}`jar`{% endraw %} 태스크에서 {% raw %}`**/*.jjt`{% endraw %} 파일을 제외하도록 설정합니다. 이렇게 하면 JAR 생성 시 해당 파일이 포함되지 않습니다. {% raw %}`exclude`{% endraw %} 메서드는 해당 태스크에 적용되며, 특정 패턴에 해당하는 파일을 제외하는 데 사용됩니다.

