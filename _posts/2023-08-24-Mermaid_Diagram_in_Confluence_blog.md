---
layout: post
title: "Mermaid Diagram in Confluence"
tags: [in, Diagram, Confluence, Mermaid]
---

## 사용자 입력:
> conflence에서 무료로 제공하는 mermaid 플러그인으로 간단한 데이터 흐름도를 작성하는 예제 알려줘.

## ChatGPT 응답:
> Confluence에는 다양한 플러그인들이 있지만, 제 지식 업데이트 기준(2021년 9월)까지 Confluence에서 무료로 제공하는 공식 Mermaid 플러그인은 없습니다. 그렇지만, 만약 Confluence에서 Mermaid로 다이어그램을 그리고 싶다면 다음과 같은 방법으로 진행할 수 있습니다.

1. **사용자 정의 매크로 만들기**: Confluence 관리자가 사용자 정의 매크로를 만들어 Mermaid 스크립트를 직접 포함시킬 수 있습니다.

2. **제3자 플러그인 사용**: Atlassian Marketplace에서 Mermaid와 호환되는 제3자 플러그인을 검색하고 설치할 수 있습니다. 설치 후, 해당 플러그인의 문서나 가이드를 참조하여 사용하면 됩니다.

아래는 Mermaid를 사용하여 간단한 데이터 흐름도(Data Flow Diagram)를 작성하는 예제입니다:

{% raw %}```mermaid
graph TD;
    A[시작] --> B[데이터 입력];
    B --> C[데이터 처리];
    C --> D[데이터 저장];
    C --> E[데이터 출력];
```{% endraw %}

이 코드는 다음과 같은 흐름을 나타냅니다:

- "시작"에서 시작하여
- "데이터 입력"으로 이동,
- 그 후 "데이터 처리"를 거쳐,
- "데이터 저장" 또는 "데이터 출력"으로 이동합니다.

만약 Confluence에 Mermaid 플러그인 또는 매크로가 설치되어 있다면, 위의 Mermaid 코드를 적절한 위치에 넣으면 흐름도가 그려질 것입니다.

마지막으로, Confluence나 사용하고 있는 플러그인의 업데이트 상황에 따라 방법이나 절차가 바뀔 수 있으니, 항상 최신의 공식 문서나 가이드를 참고하는 것이 좋습니다.

