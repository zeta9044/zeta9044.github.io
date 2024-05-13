---
layout: post
title: "Qodana 구성 방법 (Qodana Configuration)"
date: 2023-11-27
tags: [Configuration, Qodana, 방법, 구성]
categories: blog
---

## 사용자 입력:
> gradle 자바 멀티 프로젝트를 qodana로 분석하려고해. 그런데,  하위 특정프로젝트의 특정디렉토리만 취약점 inspection할려면 qodana.yaml을 어떻게 구성해야해 ? 

## ChatGPT 응답:
> Gradle을 사용하는 자바 멀티 프로젝트에서 Qodana를 이용해 특정 하위 프로젝트의 특정 디렉토리만 분석하기 위해서는 `qodana.yaml` 파일을 적절하게 구성해야 합니다. 이를 위해 다음과 같은 단계를 따를 수 있습니다:

1. **Qodana 설정 파일 생성**: 프로젝트의 루트 디렉토리에 `qodana.yaml` 파일을 생성합니다.

2. **분석할 디렉토리 지정**: `qodana.yaml` 파일 내에서 `include`와 `exclude` 섹션을 사용하여 분석할 특정 디렉토리를 지정합니다. 예를 들어, 특정 하위 프로젝트의 `src/main/java` 디렉토리만 분석하려면 해당 경로를 `include` 섹션에 추가합니다.

3. **분석 설정 조정**: 필요한 경우, 다양한 분석 옵션을 `qodana.yaml`에 추가하여 분석을 더 세밀하게 조정할 수 있습니다. 예를 들어, 특정 검사 규칙을 활성화하거나 비활성화할 수 있습니다.

4. **Qodana 실행**: 설정을 완료한 후 Qodana를 실행하여 분석을 시작합니다.

다음은 `qodana.yaml`의 예시 구성입니다:

```yaml
profile:
  name: "Java"

paths:
  - name: "include"
    path: "subproject/src/main/java"
  - name: "exclude"
    path: "*/test"

# 필요한 경우 추가 분석 옵션 설정
```

이 구성에서는 `subproject/src/main/java` 디렉토리만 분석하고, 모든 `test` 디렉토리는 제외합니다. 프로젝트의 구조와 필요에 따라 경로를 조정해야 합니다.

Qodana의 자세한 설정 방법과 옵션에 대해서는 Qodana의 공식 문서를 참고하는 것이 좋습니다.

