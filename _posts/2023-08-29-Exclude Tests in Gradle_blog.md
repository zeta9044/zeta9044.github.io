---
layout: post
title: "Exclude Tests in Gradle"
date: 2023-08-29
tags: [Tests, in, Exclude, Gradle]
categories: blog
---

## 사용자 입력:
> gradle 빌드에서 테스트를 제외하는 명령은

## ChatGPT 응답:
> Gradle 빌드 시에 테스트를 제외하려면, 테스트 작업을 건너뛰도록 명령을 내려야 합니다. 이를 위해서는 `test` 작업을 건너뛰도록 명령을 내려야 합니다. 아래는 그 방법입니다.

터미널 또는 명령 프롬프트에서:

```bash
./gradlew build -x test
```

위 명령에서 `-x` 뒤에 오는 작업 이름(`test`)은 실행에서 제외되게 됩니다.

또는 Gradle 빌드 파일(`build.gradle` 또는 `build.gradle.kts`)에 테스트를 항상 건너뛰게 설정하려면 다음과 같이 설정할 수 있습니다.

`build.gradle`에서:

```gradle
test {
    enabled = false
}
```

`build.gradle.kts`에서 (Kotlin 스크립트를 사용하는 경우):

```kotlin
tasks.test {
    enabled = false
}
```

하지만 이 설정은 저장하고 난 뒤에 모든 빌드에서 테스트를 건너뛰게 되므로, 임시적으로 테스트를 건너뛰고 싶을 때만 사용하는 것이 좋습니다.

