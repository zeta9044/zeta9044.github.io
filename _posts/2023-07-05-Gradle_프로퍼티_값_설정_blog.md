---
layout: post
title: "Gradle 프로퍼티 값 설정"
tags: [설정, 퍼티, 값, 프로, Gradle]
---

## 사용자 입력:
> gradle task를 cmd에서 실행시킬때, roject.getProperty("debug_parser")의 debug_parser는 어떻게 지정하나?

## ChatGPT 응답:
> Gradle에서 {% raw %}`project.getProperty("debug_parser")`{% endraw %}를 사용하여 프로퍼티 값을 가져올 때는 다음과 같은 방법으로 {% raw %}`debug_parser`{% endraw %} 값을 지정할 수 있습니다.

1. 명령 줄에서 {% raw %}`-P`{% endraw %} 옵션을 사용하여 프로퍼티 값을 지정합니다.

   {% raw %}```shell
   gradle <taskName> -Pdebug_parser=<value>
   ```{% endraw %}

   예를 들어, {% raw %}`build`{% endraw %} 태스크를 실행하고 {% raw %}`debug_parser`{% endraw %} 프로퍼티 값을 "true"로 지정하려면 다음과 같이 입력합니다.

   {% raw %}```shell
   gradle build -Pdebug_parser=true
   ```{% endraw %}

2. {% raw %}`gradle.properties`{% endraw %} 파일에 프로퍼티를 정의합니다.

   프로젝트 디렉토리에 {% raw %}`gradle.properties`{% endraw %} 파일을 만들고 다음과 같이 {% raw %}`debug_parser`{% endraw %} 프로퍼티 값을 설정합니다.

   {% raw %}```
   debug_parser=true
   ```{% endraw %}

   그런 다음, 명령 줄에서 Gradle 태스크를 실행하면 해당 프로퍼티 값이 사용됩니다.

   {% raw %}```shell
   gradle <taskName>
   ```{% endraw %}

   프로퍼티 값을 변경하려면 {% raw %}`gradle.properties`{% endraw %} 파일을 수정하면 됩니다.

3. 빌드 스크립트에서 직접 프로퍼티를 설정합니다.

   {% raw %}`build.gradle`{% endraw %} 파일에 다음과 같이 {% raw %}`ext`{% endraw %} 블록을 사용하여 프로퍼티를 정의하고 값을 설정합니다.

   {% raw %}```groovy
   ext {
       debug_parser = true
   }
   ```{% endraw %}

   이렇게 설정한 프로퍼티는 빌드 스크립트 내에서 사용할 수 있습니다.

   {% raw %}```groovy
   task myTask {
       doLast {
           if (debug_parser) {
               // debug_parser 프로퍼티가 true인 경우에만 실행되는 코드
           }
       }
   }
   ```{% endraw %}

위의 방법 중 하나를 선택하여 {% raw %}`debug_parser`{% endraw %} 프로퍼티 값을 설정하고 Gradle 태스크를 실행할 수 있습니다.

