---
layout: post
title: "Bamboo 빌드 - 브랜치 지정"
tags: [Bamboo, 지정, 브랜치, 빌드]
---

## 사용자 입력:
> bamboo에서 빌드할때  Create plan branch하는 방법을 When new branch in repository is created and matches expression 으로 선택하면, 여러 개 지정하는 예를 보여줘

## ChatGPT 응답:
> "Create plan branch" 작업을 수행할 때, "When new branch in repository is created and matches expression" 옵션을 선택하면 여러 개의 브랜치를 지정할 수 있습니다. 

다음은 예시입니다. 

1. 브랜치 이름이 feature/* 인 경우:
{% raw %}```
feature/*
```{% endraw %}

2. 브랜치 이름이 release/* 또는 hotfix/* 인 경우:
{% raw %}```
(release/*)|(hotfix/*)
```{% endraw %}

3. 브랜치 이름이 develop 또는 staging 인 경우:
{% raw %}```
develop|staging
```{% endraw %}

위와 같이 여러 개의 브랜치 이름을 지정할 때는 | (or) 연산자를 사용하면 됩니다. 괄호는 그룹을 지정하기 위해 사용되며, 연산자 우선순위를 나타냅니다. 

이렇게 지정된 브랜치는 Bamboo에서 빌드할 때 자동으로 트리거됩니다.

