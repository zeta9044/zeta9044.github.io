---
layout: post
title: "상속 공통 의존성 Gradle"
tags: [공통, Gradle, 상속, 의존]
---

## 사용자 입력:
> gradle부모에서 자식프로젝트에 공통 dependency를 상속하는 법

## ChatGPT 응답:
> Gradle에서 부모 프로젝트 (root project)에서 자식 프로젝트 (sub-projects)로 공통의 의존성 (dependency)을 상속하려면 다음과 같은 방법을 사용할 수 있습니다.

1. **Root Project의 {% raw %}`build.gradle`{% endraw %} 파일에 공통 의존성 정의**:
   부모 프로젝트의 {% raw %}`build.gradle`{% endraw %} 파일에서 {% raw %}`subprojects`{% endraw %} 블록을 사용하여 자식 프로젝트에 적용할 공통 의존성을 정의합니다.

   예를 들어, 모든 자식 프로젝트에 {% raw %}`spring-boot-starter-web`{% endraw %} 의존성을 추가하려면 아래와 같이 설정합니다:

   {% raw %}```groovy
   subprojects {
       apply plugin: 'java'

       repositories {
           mavenCentral()
       }

       dependencies {
           implementation 'org.springframework.boot:spring-boot-starter-web'
       }
   }
   ```{% endraw %}

2. **자식 프로젝트에서 추가 의존성 사용**:
   자식 프로젝트의 {% raw %}`build.gradle`{% endraw %} 파일에서는 부모 프로젝트에서 정의된 공통 의존성 외에 추가적인 의존성을 선언할 수 있습니다. 이미 정의된 공통 의존성은 자동으로 상속됩니다.

3. **필요한 경우 예외 처리**:
   특정 자식 프로젝트에서 공통 의존성을 사용하지 않아야 하는 경우, 해당 의존성을 제외하도록 설정할 수 있습니다.

   예를 들어, 공통으로 추가된 {% raw %}`spring-boot-starter-web`{% endraw %} 의존성을 특정 자식 프로젝트에서 제외하려면 아래와 같이 설정합니다:

   {% raw %}```groovy
   configurations {
       all*.exclude group: 'org.springframework.boot', module: 'spring-boot-starter-web'
   }
   ```{% endraw %}

이러한 방식으로 Gradle에서 부모 프로젝트에서 자식 프로젝트로 공통 의존성을 쉽게 상속할 수 있습니다.

## 사용자 입력:
> allproject와 subproject의 차이가 뭐야?

## ChatGPT 응답:
> {% raw %}`allprojects`{% endraw %}와 {% raw %}`subprojects`{% endraw %}는 Gradle 빌드 스크립트에서 프로젝트 계층 구조에 설정을 적용하기 위한 블록입니다. 이 두 블록의 주요 차이점은 적용 대상에 있습니다.

1. **allprojects**:
   - {% raw %}`allprojects`{% endraw %} 블록 내에서 정의된 설정은 현재 프로젝트와 그 모든 하위 프로젝트 (자식 프로젝트들)에 적용됩니다.
   - 예를 들어, 루트 프로젝트의 {% raw %}`build.gradle`{% endraw %} 파일에서 {% raw %}`allprojects`{% endraw %} 블록을 사용하면 해당 설정은 루트 프로젝트와 모든 자식 프로젝트에 적용됩니다.

   {% raw %}```groovy
   allprojects {
       repositories {
           mavenCentral()
       }
   }
   ```{% endraw %}
   위 설정은 모든 프로젝트 (루트 및 자식 프로젝트 모두)에서 Maven Central 저장소를 사용하도록 설정합니다.

2. **subprojects**:
   - {% raw %}`subprojects`{% endraw %} 블록 내에서 정의된 설정은 현재 프로젝트의 자식 프로젝트들에만 적용됩니다. 현재 프로젝트에는 적용되지 않습니다.
   - 예를 들어, 루트 프로젝트의 {% raw %}`build.gradle`{% endraw %} 파일에서 {% raw %}`subprojects`{% endraw %} 블록을 사용하면 해당 설정은 루트 프로젝트를 제외한 모든 자식 프로젝트에만 적용됩니다.

   {% raw %}```groovy
   subprojects {
       apply plugin: 'java'
   }
   ```{% endraw %}
   위 설정은 모든 자식 프로젝트에만 Java 플러그인을 적용하며, 루트 프로젝트에는 적용되지 않습니다.

요약하면, {% raw %}`allprojects`{% endraw %}는 현재 프로젝트와 그 하위 모든 프로젝트에 설정을 적용하는 반면, {% raw %}`subprojects`{% endraw %}는 현재 프로젝트를 제외한 하위 프로젝트들에만 설정을 적용합니다.

