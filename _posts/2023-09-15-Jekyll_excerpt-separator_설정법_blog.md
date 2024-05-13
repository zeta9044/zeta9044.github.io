---
layout: post
title: "Jekyll excerpt_separator 설정법"
date: 2023-09-15
tags: [설정, Jekyll, 법]
categories: blog
---

## 사용자 입력:
> jekyll에서 excerpt_separater 사용법 예시 알려줘.

## ChatGPT 응답:
> Jekyll에서 `excerpt_separator`는 포스트의 본문에서 발췌문을 어떻게 구분할지 정의하는 설정값입니다. 이 설정값을 사용하면 포스트의 일부분만을 발췌하여 홈페이지, 아카이브 페이지, 카테고리 페이지 등에서 표시할 수 있습니다.

`excerpt_separator`를 사용하는 방법은 다음과 같습니다:

1. **`_config.yml` 파일에서 `excerpt_separator` 설정하기**
   
    `_config.yml` 파일에 `excerpt_separator` 값을 설정합니다. 예를 들어, 포스트에서 `<!--more-->` 태그를 발췌문의 구분자로 사용하려면 아래와 같이 설정할 수 있습니다:

    ```yml
    excerpt_separator: "<!--more-->"
    ```

2. **포스트에서 `excerpt_separator` 사용하기**

    포스트에서 발췌문을 구분하고 싶은 위치에 앞서 설정한 `excerpt_separator` 값을 추가합니다. 

    예를 들어:

    ```markdown
    ---
    layout: post
    title: "Jekyll에서 excerpt_separator 사용하기"
    ---

    Jekyll은 정적 사이트 생성기입니다. 이 도구를 사용하면 Markdown, Liquid 등을 활용하여 동적인 웹사이트를 만들 수 있습니다.

    <!--more-->

    이 포스트에서는 `excerpt_separator`의 사용법에 대해 자세히 설명합니다...
    ```

    위의 예에서 `<!--more-->` 태그 앞의 내용만 발췌문으로 인식됩니다.

3. **발췌문 표시하기**

    Jekyll 템플릿에서는 `post.excerpt` 변수를 사용하여 발췌문을 표시할 수 있습니다.

    ```html
    <div class="post-excerpt">
      {{ post.excerpt }}
    </div>
    ```

이렇게 설정하면 Jekyll은 `<!--more-->` 태그 앞의 내용만을 발췌문으로 인식하고, 해당 발췌문을 원하는 위치에 표시할 수 있게 됩니다.

