---
layout: post
title: "IntelliJ 설정 초기화"
tags: [IntelliJ, 설정, 초기]
---

## 사용자 입력:
> intellj 설정을 초기화하는 방법

## ChatGPT 응답:
> IntelliJ를 초기화하는 방법은 다음과 같습니다.

1. IntelliJ를 닫습니다.
2. IntelliJ의 설정 디렉토리를 찾습니다. 설정 디렉토리는 다음과 같은 위치에 있습니다.
   - Windows: C:\Users\<사용자 이름>\.IntelliJIdea<버전>
   - macOS: /Users/<사용자 이름>/Library/Preferences/IntelliJIdea<버전>
   - Linux: /home/<사용자 이름>/.IntelliJIdea<버전>
3. 설정 디렉토리를 삭제합니다.
4. IntelliJ를 다시 실행합니다. IntelliJ는 새로운 설정 파일을 생성하여 초기 상태로 다시 시작됩니다.

참고: IntelliJ의 설정 초기화는 모든 사용자 지정 설정과 플러그인, 테마 등을 삭제합니다. 이를 복구하려면 백업 파일에서 다시 가져와야합니다.

## 사용자 입력:
> groovy에서  @ComponentScan  예제 보여줘

## ChatGPT 응답:
> Sure, here's an example of using {% raw %}`@ComponentScan`{% endraw %} in a Groovy application:

Assuming you have a package {% raw %}`com.example`{% endraw %} that contains all of your Spring components, you can use the following code in your Groovy application:

{% raw %}```
package com.example

import org.springframework.boot.SpringApplication
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.context.annotation.ComponentScan

@SpringBootApplication
@ComponentScan("com.example")
class MyApplication {
    static void main(String[] args) {
        SpringApplication.run(MyApplication, args)
    }
}
```{% endraw %}

In the code above, {% raw %}`@SpringBootApplication`{% endraw %} is used to enable Spring Boot features, while {% raw %}`@ComponentScan`{% endraw %} is used to scan the {% raw %}`com.example`{% endraw %} package and all of its sub-packages for Spring components to be managed by the Spring container.

Note that you can also specify multiple package names to scan by separating them with commas, like this:

{% raw %}```
@ComponentScan({"com.example.package1", "com.example.package2"})
```{% endraw %}

I hope this helps! Let me know if you have any more questions.

## 사용자 입력:
> 위 코드에서 basePackages로 설정해봐

## ChatGPT 응답:
> Sure! Here's an example of using {% raw %}`basePackages`{% endraw %} instead of {% raw %}`value`{% endraw %} in the {% raw %}`@ComponentScan`{% endraw %} annotation:

{% raw %}```
package com.example

import org.springframework.boot.SpringApplication
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.context.annotation.ComponentScan

@SpringBootApplication
@ComponentScan(basePackages = ["com.example"])
class MyApplication {
    static void main(String[] args) {
        SpringApplication.run(MyApplication, args)
    }
}
```{% endraw %}

In this code, we're using {% raw %}`basePackages`{% endraw %} to specify the base package to scan for Spring components. The value of {% raw %}`basePackages`{% endraw %} is an array of strings, so you can specify multiple package names like this:

{% raw %}```
@ComponentScan(basePackages = ["com.example.package1", "com.example.package2"])
```{% endraw %}

Note that {% raw %}`basePackages`{% endraw %} and {% raw %}`value`{% endraw %} are interchangeable, so you can use either one depending on your preference.

I hope this helps! Let me know if you have any more questions.

## 사용자 입력:
> WARNING: An illegal reflective access operation has occurred , illeal access를 허용하는 jvm옵션

## ChatGPT 응답:
> Java 9 이상의 버전에서는 액세스 규칙이 변경되어 일부 리플렉션 코드가 이전 버전과 호환되지 않을 수 있습니다. 이러한 변경으로 인해 애플리케이션에서 다음과 같은 경고 메시지가 표시될 수 있습니다.

{% raw %}```
WARNING: An illegal reflective access operation has occurred
```{% endraw %}

이 경고 메시지는 애플리케이션이 Java 9 이상의 버전에서 실행될 때 리플렉션 코드가 내부적으로 사용하는 API에 대한 비공개 액세스를 시도했음을 나타냅니다.

이러한 경고 메시지를 제거하려면, 다음과 같은 JVM 옵션을 사용하여 액세스 규칙을 완화할 수 있습니다.

{% raw %}```
--add-opens <module>/<package>=<module>(,<module>/<package>=<module>,...)
```{% endraw %}

위의 옵션에서 {% raw %}`<module>/<package>`{% endraw %}는 리플렉션 액세스가 필요한 모듈과 패키지를 지정하며, {% raw %}`<module>`{% endraw %}은 해당 모듈의 이름을 지정합니다. 쉼표로 구분하여 여러 개의 모듈과 패키지를 지정할 수 있습니다.

예를 들어, {% raw %}`com.example`{% endraw %} 패키지에 대한 리플렉션 액세스를 허용하려면 다음과 같이 옵션을 사용할 수 있습니다.

{% raw %}```
--add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.base/java.util=ALL-UNNAMED --add-opens java.base/java.io=ALL-UNNAMED --add-opens java.base/java.net=ALL-UNNAMED --add-opens java.base/java.security=ALL-UNNAMED --add-opens java.base/java.util.concurrent=ALL-UNNAMED --add-opens java.base/sun.nio.ch=ALL-UNNAMED --add-opens java.management/sun.management=ALL-UNNAMED --add-opens jdk.management/com.sun.management.internal=ALL-UNNAMED --add-opens jdk.management/com.sun.management.jmx=ALL-UNNAMED --add-opens jdk.management/com.sun.management.snmp=ALL-UNNAMED --add-opens jdk.management/com.sun.management=ALL-UNNAMED --add-opens java.xml/com.sun.org.apache.xerces.internal.jaxp=ALL-UNNAMED --add-opens jdk.xml.dom/com.sun.org.apache.xerces.internal.dom=ALL-UNNAMED --add-opens jdk.zipfs/jdk.nio.zipfs=ALL-UNNAMED --add-opens jdk.compiler/com.sun.tools.javac.comp=ALL-UNNAMED --add-opens jdk.javadoc/jdk.javadoc.internal.tool=ALL-UNNAMED --add-opens jdk.jfr/jdk.jfr=ALL-UNNAMED
```{% endraw %}

위의 옵션은 {% raw %}`java.base`{% endraw %}, {% raw %}`java.management`{% endraw %}, {% raw %}`jdk.management`{% endraw %}, {% raw %}`java.xml`{% endraw %}, {% raw %}`jdk.xml.dom`{% endraw %}, {% raw %}`jdk.zipfs`{% endraw %}, {% raw %}`jdk.compiler`{% endraw %}, {% raw %}`jdk.javadoc`{% endraw %} 및 {% raw %}`jdk.jfr`{% endraw %} 모듈에 대한 모든 패키지에 대한 액세스

## 사용자 입력:
> intellj에서 Execution failed for task ':qtrack-web:bootRun'.
> Process 'command 'C:\Program Files\Java\jdk-11.0.9\bin\java.exe'' finished with non-zero exit value 1 이 메세지가 뜨는데, jdk18로 변경하고 싶으면 어떻게 해야돼?

## ChatGPT 응답:
> 만약 IntelliJ에서 Gradle을 사용하여 실행 중인 Spring Boot 애플리케이션에서 JDK 11 대신 JDK 8을 사용하고자 한다면, 다음과 같은 단계를 수행할 수 있습니다.

1. 프로젝트 구성 파일 ({% raw %}`build.gradle`{% endraw %} 또는 {% raw %}`build.gradle.kts`{% endraw %})을 엽니다.
2. {% raw %}`java`{% endraw %} 플러그인을 찾습니다. 일반적으로 다음과 같이 정의됩니다.

   {% raw %}```groovy
   plugins {
       id 'java'
   }
   ```{% endraw %}

3. JDK 버전을 1.8로 변경하려면 {% raw %}`sourceCompatibility`{% endraw %} 및 {% raw %}`targetCompatibility`{% endraw %} 설정을 다음과 같이 변경합니다.

   {% raw %}```groovy
   plugins {
       id 'java'
   }

   java {
       sourceCompatibility = JavaVersion.VERSION_1_8
       targetCompatibility = JavaVersion.VERSION_1_8
   }
   ```{% endraw %}

   위의 코드에서 {% raw %}`JavaVersion.VERSION_1_8`{% endraw %}은 JDK 8을 나타냅니다.

4. IntelliJ에서 Gradle 뷰를 열고 "Refresh Gradle Project"를 클릭하여 프로젝트를 다시로드합니다.
5. 이제 JDK 8로 Spring Boot 애플리케이션을 실행할 수 있어야합니다.

참고: 이 방법은 IntelliJ에서 Gradle을 사용하여 Spring Boot 애플리케이션을 실행할 때만 작동합니다. 만약 IntelliJ에서 직접 Spring Boot 애플리케이션을 실행하는 경우, IntelliJ의 프로젝트 설정에서 JDK 버전을 변경해야합니다. 이 경우에는 "Project Structure" 다이얼로그에서 JDK 버전을 변경할 수 있습니다.

