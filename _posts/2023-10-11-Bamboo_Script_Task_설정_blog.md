---
layout: post
title: "Bamboo Script Task 설정"
date: 2023-10-11
tags: [설정, Task, Bamboo, Script]
categories: blog
---

## 사용자 입력:
> bamboo task에서 script task를 추가하여, gradle명령어를 실행하는데 순차적으로 진행하다가 fail이 나면 중단하도록 하는 방법은?

## ChatGPT 응답:
> Bamboo에서 Script Task를 사용하여 Gradle 명령어를 실행하고 해당 명령어가 실패하면 빌드를 중단하게 만들고 싶다면, 기본적으로 Bamboo가 제공하는 기능을 사용하여 쉽게 구성할 수 있습니다.

다음은 그 과정을 단계별로 설명합니다:

1. **Bamboo Plan 생성 또는 편집**:
   - Bamboo에서 Plan을 생성하거나 편집합니다.

2. **Script Task 추가**:
   - 해당 Plan에서 Stage와 Job을 설정하고, Job 내에 Task를 추가합니다.
   - 'Add Task'를 클릭하여 'Script'를 선택합니다.

3. **Gradle 명령어 설정**:
   - Script Task를 추가한 후, 'Script' 섹션에 Gradle 명령어를 입력합니다. 예를 들어, `./gradlew build`라는 명령어를 실행하고자 한다면 해당 명령어를 입력합니다.

4. **실패 시 빌드 중단 설정**:
   - Bamboo는 기본적으로 Script Task의 명령어가 실패하면 (즉, 0이 아닌 exit code를 반환하면) 그 이후의 Task들을 실행하지 않고 빌드를 실패로 표시합니다. 따라서 별도의 설정 없이도 Script Task의 명령어가 실패하면 빌드는 중단됩니다.

5. **Bamboo Plan 저장 및 실행**:
   - 설정을 모두 완료한 후 Plan을 저장하고 실행합니다.
   - 만약 Script Task에서 실행된 Gradle 명령어가 실패한다면, Bamboo는 해당 Plan의 실행을 중단하고 빌드 실패로 표시합니다.

위의 방법을 통해 Bamboo에서 Gradle 명령어를 실행하면서 해당 명령어가 실패할 경우 빌드를 중단하게 설정할 수 있습니다.

## 사용자 입력:
> 나도 그런줄 알았는데, 멀티줄일때, 중간에 에러나도 멈추지 않던데.

## ChatGPT 응답:
> 멀티 라인 스크립트에서 중간에 에러가 발생할 경우도 Bamboo가 작업을 중단하도록 하려면, 각 명령어가 실패했을 때 즉시 스크립트 실행을 중단하도록 설정해야 합니다.

Bash 스크립트를 사용하는 경우, `set -e` 명령어를 스크립트의 상단에 추가하여 명령어가 실패했을 때 스크립트 실행을 중단하도록 설정할 수 있습니다.

예시:

```bash
#!/bin/bash
set -e

./gradlew clean
./gradlew build
./gradlew test
```

`set -e`는 스크립트에서 명령어가 실패하면 즉시 스크립트 실행을 중단하게 됩니다. 따라서 `./gradlew clean`이 실패한다면 `./gradlew build`와 `./gradlew test`는 실행되지 않습니다.

Bamboo의 Script Task에 위와 같은 형식의 스크립트를 사용하면 중간에 명령어가 실패하더라도 Bamboo가 작업을 중단하도록 설정할 수 있습니다.

