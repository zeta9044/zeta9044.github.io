---
layout: post
title: "Jekyll 요약 설정 설명"
date: 2023-09-15
tags: [설명, 설정, 요약, Jekyll]
categories: blog
---

## 사용자 입력:
> jekyll에서 excerpt_separator: "\n\n" 일 때, 사용법

## ChatGPT 응답:
> `Jekyll`은 정적 사이트 생성기로, 웹사이트를 빌드하기 위해 사용되는 도구입니다. `excerpt_separator`는 Jekyll에서 게시물의 요약 (excerpt)을 어디까지 포함할 것인지 결정하는 설정 값입니다.

`excerpt_separator: "\n\n"`로 설정하면, 게시물의 본문에서 첫 번째 두 개의 줄바꿈 (`\n\n`)을 만날 때까지의 텍스트가 요약으로 사용됩니다.

예를 들어, 아래와 같은 게시물이 있다고 해보겠습니다:

```
---
layout: post
title: "Jekyll Excerpt Separator"
---

안녕하세요, Jekyll에 대해 알아보겠습니다.

Jekyll은 정적 사이트 생성기로 매우 유용합니다.
이번 포스팅에서는 `excerpt_separator`에 대해 알아보겠습니다.

...
```

`excerpt_separator: "\n\n"` 설정이 있다면, 위 게시물의 요약은 아래와 같이 됩니다:

```
안녕하세요, Jekyll에 대해 알아보겠습니다.
```

이 요약은 웹사이트의 메인 페이지나 카테고리 페이지 등에서 전체 게시물 내용 대신 표시될 수 있습니다.

Jekyll 템플릿에서 요약을 표시하려면, `post.excerpt` 변수를 사용합니다. 예를 들면:

```liquid
{{ post.excerpt }}
```

요약을 사용하는 것은 페이지 로딩 속도를 향상시키거나, 사용자에게 짧은 내용을 빠르게 보여주기 위한 방법으로 유용합니다.

