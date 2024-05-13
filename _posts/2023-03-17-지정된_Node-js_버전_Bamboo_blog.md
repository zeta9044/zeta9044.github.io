---
layout: post
title: "지정된 Node.js 버전 Bamboo"
tags: [js, 버전, Node, 지정, Bamboo]
---

## 사용자 입력:
> bamboo에서 빌드할때 nodejs버전을 지정할 수 있나?

## ChatGPT 응답:
> 네, Bamboo에서 Node.js 버전을 지정할 수 있습니다. 

Bamboo에서 Node.js를 사용하는 경우, Node.js 실행 파일을 사용하여 실행할 수 있습니다. 이를 위해 Node.js 실행 파일의 경로를 Bamboo에 지정해야 합니다. 

Node.js 실행 파일의 경로를 지정하려면, 다음과 같은 단계를 수행할 수 있습니다:

1. Bamboo 대시보드에서 빌드 계획을 선택합니다.
2. "Task Configuration"에서 "Add Task"를 선택합니다.
3. "Script" 탭에서 "Node.js"를 선택합니다.
4. "Node.js Executable" 필드에 사용할 Node.js 실행 파일의 경로를 입력합니다.

예를 들어, Node.js 14.17.0을 사용하려면 "Node.js Executable" 필드에 다음과 같은 경로를 입력할 수 있습니다:

{% raw %}```
/opt/nodejs/14.17.0/bin/node
```{% endraw %}

이제 빌드가 실행될 때, 지정된 Node.js 실행 파일을 사용하여 빌드가 실행됩니다. 이 방법으로 빌드할 때 항상 지정된 버전의 Node.js를 사용할 수 있습니다.

