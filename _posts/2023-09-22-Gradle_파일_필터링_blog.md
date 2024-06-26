---
layout: post
title: "Gradle 파일 필터링"
tags: [필터, 파일, 링, Gradle]
---

## 사용자 입력:
> gradle에서 outputDirectory에 들어갈 파일을 필터링하는 방법은?

## ChatGPT 응답:
> Gradle에서 {% raw %}`outputDirectory`{% endraw %}에 들어갈 파일을 필터링하려면, 일반적으로 {% raw %}`Copy`{% endraw %} 작업을 사용하여 원하는 파일 또는 디렉토리의 내용을 복사하고 필터링할 수 있습니다. 

다음은 {% raw %}`Copy`{% endraw %} 작업을 사용하여 특정 패턴에 일치하는 파일만 복사하는 예제입니다:

{% raw %}```groovy
task copyFilteredFiles(type: Copy) {
    from 'src/main/resources' // 복사할 원본 디렉토리
    into 'build/outputDirectory' // 복사할 대상 디렉토리
    include '**/*.txt' // .txt 확장자를 가진 파일만 포함
    exclude '**/exclude-me.txt' // exclude-me.txt 파일을 제외
}
```{% endraw %}

위의 예제에서는 {% raw %}`src/main/resources`{% endraw %} 디렉토리의 내용 중 {% raw %}`.txt`{% endraw %} 확장자를 가진 파일만 {% raw %}`build/outputDirectory`{% endraw %} 디렉토리로 복사합니다. 그러나 {% raw %}`exclude-me.txt`{% endraw %}는 제외됩니다.

또한, {% raw %}`filter`{% endraw %} 메서드를 사용하여 파일의 내용을 변형할 수도 있습니다. 예를 들면:

{% raw %}```groovy
task copyAndFilterFiles(type: Copy) {
    from 'src/main/resources'
    into 'build/outputDirectory'
    include '**/*.properties'
    filter { String line -> 
        // 주석 제거
        line.startsWith('#') ? null : line 
    }
}
```{% endraw %}

위의 예제에서는 {% raw %}`.properties`{% endraw %} 확장자를 가진 파일만 복사하면서, 각 파일의 내용 중 주석(시작이 {% raw %}`#`{% endraw %}로 되어 있는 줄)을 제거합니다.

이와 같은 방법으로 원하는 방식으로 파일을 필터링하고 복사할 수 있습니다.

