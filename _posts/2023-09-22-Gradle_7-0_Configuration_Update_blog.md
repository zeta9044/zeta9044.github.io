---
layout: post
title: "Gradle 7.0 Configuration Update"
tags: [Configuration, Gradle, Update]
---

## 사용자 입력:
> configurations {
    jar.archiveName = 'lia-ds-cosec.jar'
} 을 gradle 7.0이상에서 사용하는 걸로 바꿔봐.

## ChatGPT 응답:
> {% raw %}`jar.archiveName`{% endraw %} 속성은 Gradle 7.0 이상에서는 사용되지 않습니다. 대신 {% raw %}`archiveFileName`{% endraw %} 속성을 사용해야 합니다. 

다음은 Gradle 7.0 이상의 방식으로 설정을 수정한 것입니다:

{% raw %}```gradle
tasks.named('jar') {
    archiveFileName.set('lia-ds-cosec.jar')
}
```{% endraw %}

이렇게 수정하면 Gradle 7.0 이상 버전에서 잘 동작할 것입니다.

## 사용자 입력:
> 위 작업은 jar파일명을 lia-ds-cosec.jar으로 만드는 작업인가?

## ChatGPT 응답:
> 네, 맞습니다. 

Gradle에서 {% raw %}`jar`{% endraw %} 태스크는 프로젝트의 코드를 컴파일하고 JAR 파일로 패키징하는 작업을 수행합니다. 제시된 설정은 이 {% raw %}`jar`{% endraw %} 태스크의 결과로 생성되는 JAR 파일의 이름을 {% raw %}`lia-ds-cosec.jar`{% endraw %}로 설정하는 것입니다.

즉, Gradle 빌드를 실행하면 {% raw %}`build/libs`{% endraw %} 디렉터리 안에 {% raw %}`lia-ds-cosec.jar`{% endraw %}라는 이름의 JAR 파일이 생성됩니다.

## 사용자 입력:
> configurations {
    jar.archiveFileName= 'lia-ds-cosec.jar'
} 이렇게는 안돼?

## ChatGPT 응답:
> 아니요, 그 방식으로는 작동하지 않습니다.

{% raw %}`archiveFileName`{% endraw %}는 {% raw %}`jar`{% endraw %} 태스크의 속성입니다. 따라서 {% raw %}`configurations`{% endraw %} 블록 내에서 직접 설정할 수 없습니다. 대신 {% raw %}`tasks.named('jar')`{% endraw %} 블록을 사용하여 {% raw %}`jar`{% endraw %} 태스크의 속성을 설정해야 합니다.

따라서 올바른 방법은:

{% raw %}```gradle
tasks.named('jar') {
    archiveFileName.set('lia-ds-cosec.jar')
}
```{% endraw %}

이렇게 해야 {% raw %}`jar`{% endraw %} 태스크가 생성하는 JAR 파일의 이름을 {% raw %}`lia-ds-cosec.jar`{% endraw %}로 설정할 수 있습니다.

