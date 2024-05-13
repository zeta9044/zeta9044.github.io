---
layout: post
title: "Gradle Shadow 플러그인 사용"
date: 2024-01-05
tags: [Shadow, 사용, Gradle, 플러그인]
categories: blog
---

## 사용자 입력:
> gradle에서 dependency를 어떤 라이브러리에 대해서 implement 했는데, jar로 만들때 라이브리를 포함해서 만들려면? 그래들 6.8버전이야

## ChatGPT 응답:
> Gradle에서 의존성을 추가하고 JAR 파일을 생성할 때, 해당 의존성을 포함하려면 Gradle의 Shadow 플러그인을 사용할 수 있습니다. Gradle 6.8에서는 다음 단계를 따라야 합니다:

1. **Shadow 플러그인 추가**: `build.gradle` 파일에 Shadow 플러그인을 추가합니다.

   ```gradle
   plugins {
       id 'java'
       id 'com.github.johnrengelman.shadow' version '6.1.0'
   }
   ```

2. **의존성 설정**: 필요한 의존성을 `dependencies` 블록에 추가합니다.

   ```gradle
   dependencies {
       implementation 'org.some.library:library-name:1.0.0'
       // 다른 의존성들...
   }
   ```

3. **ShadowJar 작업 설정**: `shadowJar` 작업을 설정하여 필요한 구성을 추가합니다.

   ```gradle
   shadowJar {
       archiveBaseName.set('your-application')
       archiveVersion.set('0.1.0')
       archiveClassifier.set('')
   }
   ```

4. **JAR 생성**: Gradle Shadow 플러그인을 사용하여 JAR 파일을 생성합니다. 명령 프롬프트나 터미널에서 다음 명령어를 실행합니다.

   ```
   gradle shadowJar
   ```

이 과정을 완료하면, `build/libs` 디렉토리에 모든 의존성이 포함된 JAR 파일이 생성됩니다. Shadow 플러그인은 의존성을 JAR 파일에 포함시키고, 필요한 경우 중복을 제거하거나 다른 설정을 수행할 수 있습니다.

