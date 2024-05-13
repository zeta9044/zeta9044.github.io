---
layout: post
title: "GitHub Pages "minimal mistakes" 포스트작성"
date: 2023-09-06
tags: [작성, Pages, mistakes, 포스트, minimal, GitHub]
categories: blog
---

## 사용자 입력:
> github pages 의 테마를 minimal takes로 했는데, 포스트팅은 어떻게 해야해?

## ChatGPT 응답:
> GitHub Pages를 사용하여 Jekyll 기반의 웹사이트를 호스팅할 때 "minimal mistakes" 테마를 사용하려면 다음과 같은 단계를 따라야 합니다:

1. **테마 설치하기**: 
   먼저 Gemfile에 `gem "minimal-mistakes-jekyll"`를 추가합니다. 그런 다음, 터미널에서 `bundle install`을 실행하여 필요한 gem들을 설치합니다.

2. **_config.yml 수정하기**: 
   기본 설정 파일인 `_config.yml` 파일을 열어서 `theme: minimal-mistakes-jekyll`를 추가하거나 변경합니다.

3. **포스트 작성하기**:
   `_posts` 디렉터리에 새로운 포스트를 작성하려면 해당 디렉터리에 YYYY-MM-DD-title-of-post.md 형식의 파일을 만듭니다. 각 포스트 파일의 상단에는 Jekyll front matter가 필요합니다. 예를 들면:
   
   ```
   ---
   layout: single
   title: "제목"
   date: YYYY-MM-DD
   ---
   
   포스트의 내용을 여기에 작성합니다.
   ```

4. **로컬에서 사이트 실행하기**:
   터미널에서 `bundle exec jekyll serve`를 실행하여 로컬에서 사이트를 미리 볼 수 있습니다. 이를 통해 변경 사항을 실시간으로 확인할 수 있습니다.

5. **GitHub에 푸시하기**:
   모든 변경 사항을 commit하고 GitHub repository에 push합니다. GitHub Pages가 자동으로 사이트를 빌드하고 호스팅합니다.

6. **테마의 추가 기능 활용하기**:
   "minimal mistakes" 테마는 여러 가지 추가 기능과 설정 옵션을 제공합니다. 테마의 공식 문서를 참조하여 다양한 기능을 활용하는 방법을 알아보세요.

이러한 단계를 통해 GitHub Pages에 "minimal mistakes" 테마를 적용하고 포스트를 게시할 수 있습니다.

