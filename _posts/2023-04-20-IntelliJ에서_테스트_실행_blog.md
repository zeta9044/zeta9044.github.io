---
layout: post
title: "IntelliJ에서 테스트 실행"
date: 2023-04-20
tags: [실행, 테스트]
categories: blog
---

## 사용자 입력:
> intellij에서 test를 실행하는데, gradle로 실행하는 거 말고 설정하는 거 알려줘

## ChatGPT 응답:
> IntelliJ에서 테스트를 실행하는 두 가지 방법이 있습니다. Gradle로 실행하는 방법 외에도 IntelliJ 내장 테스트 실행기를 사용할 수 있습니다.

1. IntelliJ 내장 테스트 실행기 사용

- IntelliJ를 열고 테스트할 프로젝트를 선택합니다.
- 왼쪽 탭에서 "Project"를 선택하고, "src/test" 디렉토리를 마우스 오른쪽 버튼으로 클릭합니다.
- "Run 'All Tests'"를 선택합니다.
- 테스트 결과를 확인할 수 있는 "Run" 창이 열립니다.

2. Gradle을 사용하지 않고 IntelliJ에서 테스트 실행

- IntelliJ를 열고 테스트할 프로젝트를 선택합니다.
- 왼쪽 탭에서 "Project"를 선택하고, "src/test" 디렉토리를 마우스 오른쪽 버튼으로 클릭합니다.
- "Mark Directory As"를 선택하고, "Test Sources Root"를 선택합니다.
- 왼쪽 탭에서 "Run"을 선택하고, "+" 버튼을 클릭하여 새 테스트 구성을 만듭니다.
- "Test Kind"에서 "All in Package" 또는 "All in Directory"를 선택하고, 테스트할 패키지 또는 디렉토리를 선택합니다.
- "Run" 버튼을 클릭하여 테스트를 실행합니다.

위의 방법으로 IntelliJ에서 테스트를 실행할 수 있습니다. 다른 질문이 있으면 언제든지 물어보세요.

