---
layout: post
title: "해결책 제안: Bouncy Castle 오류"
date: 2023-12-11
tags: [오류, 제안, 책, 해결, Bouncy, Castle]
categories: blog
---

## 사용자 입력:
> lia-ds-cosec.jar를 빌드할 때, gradle에서 implementation으로 BouncyCastleProvider를 적용했더니, 파이썬에서 jar로딩할때 자바 패키지 또는 클래스 로딩 중 오류 발생: java.lang.NoClassDefFoundError: org/bouncycastle/jce/provider/BouncyCastleProvider .   

## ChatGPT 응답:
> `java.lang.NoClassDefFoundError` 오류는 Java에서 특정 클래스를 찾을 수 없을 때 발생합니다. 이 경우 `org.bouncycastle.jce.provider.BouncyCastleProvider` 클래스를 찾을 수 없다는 것을 의미합니다. 이 문제를 해결하기 위한 몇 가지 접근 방법은 다음과 같습니다:

1. **의존성 확인**: Gradle 빌드 파일에서 Bouncy Castle 라이브러리가 `implementation`으로 올바르게 추가되었는지 확인하세요. 예를 들어, `build.gradle` 파일에 다음과 같은 라인이 포함되어 있어야 합니다:

   ```gradle
   implementation 'org.bouncycastle:bcprov-jdk15on:[버전]'
   ```

   여기서 `[버전]`은 사용하려는 Bouncy Castle 라이브러리의 버전을 나타냅니다.

2. **라이브러리 버전 확인**: 사용 중인 Bouncy Castle 라이브러리의 버전이 프로젝트와 호환되는지 확인하세요. 때때로 특정 버전의 라이브러리가 프로젝트의 JDK 버전과 호환되지 않을 수 있습니다.

3. **JAR 파일 검사**: 빌드 프로세스가 완료된 후 생성된 JAR 파일 안에 `BouncyCastleProvider` 클래스가 포함되어 있는지 확인하세요. 이를 위해 JAR 파일을 압축 해제하고 해당 클래스가 포함되어 있는지 확인할 수 있습니다.

4. **Classpath 설정**: Python에서 JAR를 로드할 때, 해당 JAR 파일이 올바른 클래스 경로에 있어야 합니다. Python 스크립트에서 JAR 파일의 위치를 올바르게 지정했는지 확인하세요.

5. **JDK 버전**: 사용 중인 JDK 버전과 Bouncy Castle 라이브러리가 호환되는지 확인하세요. JDK 버전이 너무 낮거나 높으면 호환성 문제가 발생할 수 있습니다.

6. **Fat JAR 생성**: 의존성을 포함하는 Fat JAR(모든 필요한 클래스와 리소스를 하나의 JAR 파일에 패키징하는 것)를 생성하여 의존성 문제를 해결할 수 있습니다. Gradle에서는 `shadowJar` 플러그인을 사용하여 이를 수행할 수 있습니다.

이러한 접근 방법 중 하나를 시도해 보고 문제가 해결되는지 확인하세요. 문제가 지속되면 추가적인 정보(예: 사용 중인 JDK 버전, Gradle 버전, Bouncy Castle 버전 등)를 제공해 주시면 더 구체적인 해결책을 제공할 수 있습니다.

## 사용자 입력:
> $ jar tf lia-ds-cosec.jar
META-INF/
META-INF/MANIFEST.MF
com/
com/lia/
com/lia/security/
com/lia/security/DecryptFile.class
com/lia/security/LIALicense.class
LIAPWRun.class

## ChatGPT 응답:
> `jar tf` 명령어의 출력을 보면, `lia-ds-cosec.jar` 파일 내에는 `org.bouncycastle.jce.provider.BouncyCastleProvider` 클래스가 포함되어 있지 않습니다. 이것이 `java.lang.NoClassDefFoundError` 오류의 원인일 가능성이 높습니다. 이 문제를 해결하기 위해 다음 단계를 따라야 합니다:

1. **Fat JAR 생성**: Bouncy Castle 라이브러리를 포함하는 Fat JAR를 생성하십시오. 이렇게 하면 Bouncy Castle 라이브러리와 프로젝트의 모든 클래스가 하나의 JAR 파일에 포함됩니다. Gradle에서 `shadowJar` 플러그인을 사용하여 이를 수행할 수 있습니다. `build.gradle` 파일에 다음과 같은 구성을 추가하십시오:

   ```gradle
   plugins {
       id 'com.github.johnrengelman.shadow' version '버전'
   }

   shadowJar {
       archiveBaseName.set('lia-ds-cosec')
       archiveClassifier.set('')
       archiveVersion.set('')
   }
   ```

   여기서 `'버전'`은 사용 중인 shadowJar 플러그인의 버전입니다.

2. **의존성 추가**: Bouncy Castle 라이브러리가 Gradle 의존성에 올바르게 추가되었는지 다시 한 번 확인하십시오. 예:

   ```gradle
   implementation 'org.bouncycastle:bcprov-jdk15on:[버전]'
   ```

3. **재빌드**: 위의 변경 사항을 적용한 후, 프로젝트를 재빌드하십시오. Gradle에서 `gradlew shadowJar` 명령어를 사용하여 Fat JAR를 생성할 수 있습니다.

4. **JAR 파일 검사**: Fat JAR이 성공적으로 생성되었는지 확인하고, `jar tf` 명령어를 사용하여 `org.bouncycastle.jce.provider.BouncyCastleProvider` 클래스가 포함되어 있는지 다시 확인하십시오.

이러한 단계를 거치면 `java.lang.NoClassDefFoundError` 오류를 해결할 수 있을 것입니다. 오류가 지속되면, 추가적인 정보를 제공해주시면 더 구체적인 도움을 드릴 수 있습니다.

## 사용자 입력:
> gradle jar를 하면, shadowJar를 사용못하는 건가?

## ChatGPT 응답:
> Gradle에서 `jar` 태스크와 `shadowJar` 태스크는 서로 다른 목적으로 사용됩니다. 일반적으로 `jar` 태스크는 표준 JAR 파일을 생성하는 데 사용되며, 이는 프로젝트의 컴파일된 클래스와 리소스만 포함합니다. 반면에 `shadowJar` 태스크는 Fat JAR 또는 Uber JAR를 생성하는 데 사용되며, 이는 프로젝트의 클래스와 리소스 뿐만 아니라 모든 종속성(의존성 라이브러리)을 하나의 JAR 파일에 포함합니다.

두 태스크를 동시에 사용할 수는 있지만, 일반적으로 다음과 같은 방식으로 사용됩니다:

- `jar` 태스크: 프로젝트의 클래스와 리소스만 포함하는 기본 JAR 파일을 생성합니다.
- `shadowJar` 태스크: 모든 의존성을 포함하는 Fat JAR을 생성합니다.

`shadowJar` 태스크를 사용하려면, 먼저 `build.gradle` 파일에 `shadowJar` 플러그인을 추가해야 합니다. 그 후에 `shadowJar` 태스크를 실행하여 Fat JAR을 생성할 수 있습니다.

간단한 예시로, `build.gradle` 파일에 다음과 같이 추가할 수 있습니다:

```gradle
plugins {
    id 'com.github.johnrengelman.shadow' version '버전'
}

shadowJar {
    archiveBaseName.set('lia-ds-cosec')
    archiveClassifier.set('')
    archiveVersion.set('')
}
```

이 설정을 추가한 후, 커맨드 라인에서 `./gradlew shadowJar` 명령을 실행하여 Fat JAR을 생성합니다. 이렇게 하면, 의존성 문제를 해결할 수 있으며, `org.bouncycastle.jce.provider.BouncyCastleProvider` 클래스를 포함하는 JAR 파일이 생성됩니다.

## 사용자 입력:
> shadowJar {
    archiveBaseName.set('lia-ds-cosec')
    archiveClassifier.set('')
    archiveVersion.set('')
}  이거 해석.

## ChatGPT 응답:
> `shadowJar` 블록 내의 설정들은 Gradle의 `shadowJar` 태스크를 구성하기 위해 사용됩니다. 이 태스크는 Fat JAR(또는 Uber JAR)를 생성하는데, 이는 프로젝트의 모든 클래스와 리소스, 그리고 그 의존성들을 하나의 JAR 파일에 포함시킵니다. 각 설정의 의미는 다음과 같습니다:

1. **archiveBaseName.set('lia-ds-cosec')**: 생성될 JAR 파일의 기본 이름을 `lia-ds-cosec`으로 설정합니다. 즉, 파일 이름은 `lia-ds-cosec.jar`가 됩니다.

2. **archiveClassifier.set('')**: JAR 파일 이름에 추가되는 분류자(classifier)를 설정합니다. 여기서 빈 문자열(`''`)을 설정함으로써, 분류자가 사용되지 않도록 합니다. 분류자는 일반적으로 JAR 파일의 다양한 버전을 구분하는 데 사용됩니다.

3. **archiveVersion.set('')**: JAR 파일 이름에 포함될 버전을 설정합니다. 여기서도 빈 문자열(`''`)을 사용하여, 파일 이름에 버전 번호를 포함시키지 않도록 합니다.

결과적으로, 이 설정을 사용하면 `shadowJar` 태스크는 `lia-ds-cosec.jar`라는 이름의 Fat JAR 파일을 생성합니다. 이 파일에는 프로젝트의 모든 클래스와 리소스, 그리고 그 의존성들이 포함되어 있어, 의존성 문제를 해결하는 데 도움이 됩니다.

