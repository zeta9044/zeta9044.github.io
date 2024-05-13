---
layout: post
title: "Dependency Issue in Gradle"
tags: [Dependency, in, Gradle, Issue]
---

## 사용자 입력:
> // 자식프로젝트에만 적용
subprojects {

    apply plugin: 'java-library'

    repositories {
        mavenCentral()
        maven {
            url "https://plugins.gradle.org/m2/"
        }
    }

    java {
        toolchain {
            languageVersion = JavaLanguageVersion.of(11)
        }
    }

    dependencies {
        implementation fileTree(dir: "${rootProject.rootDir}/aiworks/lib/system", include: ['*.jar'], exclude: ['lia-ds-*.jar'])
    }
}
부모 build.gradle인데, 자식프로젝트에 dependency가 제대로 안 먹히는 것 같은데.

## ChatGPT 응답:
> {% raw %}`build.gradle`{% endraw %} 파일에서 부모 프로젝트의 설정이 자식 프로젝트에 올바르게 적용되지 않을 경우 여러 가지 원인이 있을 수 있습니다. 

제시된 설정만으로는 완전한 판단이 어렵지만, 몇 가지 일반적인 문제점과 확인해야 할 사항들을 제시하겠습니다:

1. **자식 프로젝트의 구조**: Gradle의 {% raw %}`subprojects`{% endraw %} 블록은 정말로 자식 프로젝트에만 적용됩니다. 따라서 프로젝트 구조를 검토하여 자식 프로젝트가 정확하게 정의되어 있는지 확인해 보세요. {% raw %}`settings.gradle`{% endraw %} 또는 {% raw %}`settings.gradle.kts`{% endraw %} 파일에 자식 프로젝트가 올바르게 명시되어 있는지 확인하십시오.

2. **자식 프로젝트의 build.gradle**: 자식 프로젝트의 {% raw %}`build.gradle`{% endraw %} 파일이 부모 프로젝트의 설정을 오버라이드 하지 않는지 확인하십시오.

3. **Dependency 확인**: 부모 {% raw %}`build.gradle`{% endraw %}에서 정의한 {% raw %}`dependencies`{% endraw %}가 실제로 파일 시스템에 존재하는지 확인하십시오. 예를 들어, {% raw %}`"${rootProject.rootDir}/aiworks/lib/system"`{% endraw %} 디렉터리 안에 올바른 JAR 파일들이 있는지 확인하세요.

4. **자식 프로젝트의 동작**: 자식 프로젝트를 직접 빌드하여 문제가 발생하는지 확인해보십시오. 예를 들면, {% raw %}`gradlew :subprojectName:build`{% endraw %} 처럼 명령을 실행하여 특정 자식 프로젝트를 빌드할 수 있습니다.

5. **Sync 문제**: IDE(Integrated Development Environment) 문제로 인해 종종 Gradle 동기화 문제가 발생할 수 있습니다. IDE에서 Gradle 프로젝트를 다시 동기화하거나 (예: IntelliJ IDEA의 경우 'Refresh' 버튼을 사용하여) 동기화 문제가 해결되는지 확인해보십시오.

6. **Gradle 버전 및 플러그인**: 사용 중인 Gradle 버전 또는 플러그인과 관련된 알려진 문제가 없는지 확인하세요.

이러한 사항들을 검토한 후에도 문제가 지속되면, 추가 정보나 자식 프로젝트의 {% raw %}`build.gradle`{% endraw %} 설정, 에러 메시지 등을 제공하시면 더 구체적으로 도움을 드릴 수 있을 것입니다.

## 사용자 입력:
> gradle에서 프로젝트 dir  변수 이름은?

## ChatGPT 응답:
> Gradle에서 프로젝트 디렉토리의 경로를 참조할 때 사용하는 변수는 {% raw %}`projectDir`{% endraw %}입니다.

예를 들어, 현재 프로젝트의 디렉토리에 있는 {% raw %}`libs`{% endraw %} 폴더 내의 모든 JAR 파일을 dependencies에 추가하려면 다음과 같이 할 수 있습니다:

{% raw %}```groovy
dependencies {
    implementation fileTree(dir: "${projectDir}/libs", include: ['*.jar'])
}
```{% endraw %}

이와 유사하게, 전체 프로젝트(즉, 루트 프로젝트)의 기본 디렉토리를 참조하려면 {% raw %}`rootProject.rootDir`{% endraw %}를 사용할 수 있습니다.

