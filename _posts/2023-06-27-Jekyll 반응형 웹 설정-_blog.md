---
layout: post
title: "Jekyll 반응형 웹 설정."
date: 2023-06-27
tags: [웹, 설정, 반응, Jekyll]
categories: blog
---

## 사용자 입력:
> zeta9044.github.io 페이지를 개설했는데,  jekyll로 관리하고 싶어. 테마는 휴대폰에서도 쓸 수 있는 반응형웹으로 관리하는 법 알려줘.

## ChatGPT 응답:
> Jekyll을 사용하여 GitHub Pages 사이트를 만드는 것은 아주 좋은 생각입니다. Jekyll은 정적 사이트 생성기로, 마크다운을 사용하여 포스트를 작성하고 HTML로 변환하여 사이트를 만듭니다. 

Jekyll을 사용하려면 우선 Ruby를 설치해야 합니다. 이 과정은 운영체제에 따라 다릅니다. macOS와 Windows에 Ruby를 설치하는 방법은 다음과 같습니다.

1. macOS에서는 터미널을 열고 다음 명령을 입력합니다.

   ```
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   brew install ruby
   ```

2. Windows에서는 RubyInstaller를 사용하여 설치합니다. RubyInstaller는 다음 주소에서 다운로드할 수 있습니다: https://rubyinstaller.org/

다음으로, Jekyll과 Bundler를 설치합니다. 터미널에 다음 명령을 입력합니다:

```
gem install jekyll bundler
```

이제 Jekyll을 사용하여 사이트를 만들 수 있습니다. 원하는 디렉터리에서 터미널을 열고 다음 명령을 입력하여 새 Jekyll 사이트를 생성합니다:

```
jekyll new zeta9044.github.io
cd zeta9044.github.io
```

이제 반응형 웹 테마를 적용할 차례입니다. Jekyll에서는 기본적으로 Minima라는 테마를 사용합니다. Minima는 반응형 디자인을 지원하기 때문에 모바일에서도 잘 보입니다. 하지만 다른 반응형 웹 테마를 사용하려면 `_config.yml` 파일을 수정하여 적용할 수 있습니다.

`_config.yml` 파일에는 다음과 같은 코드를 추가합니다.

```yaml
theme: YOUR_THEME
```

`YOUR_THEME`에 원하는 테마의 이름을 입력합니다. 테마는 RubyGems에서 찾을 수 있습니다. 예를 들어, "jekyll-theme-cayman"이라는 테마를 사용하려면 다음과 같이 입력합니다.

```yaml
theme: jekyll-theme-cayman
```

이제 적용한 테마에 따라 레이아웃과 스타일을 조정할 수 있습니다. 테마의 문서를 참고하여 사이트를 원하는 대로 커스터마이즈합니다.

마지막으로, 변경사항을 Git으로 커밋하고 GitHub에 푸시합니다.

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/zeta9044/zeta9044.github.io.git
git push -u origin master
```

이제 생성한 사이트는 `https://zeta9044.github.io`에서 볼 수 있습니다.

참고로, Jekyll 사이트를 로컬에서 미리보기 위해 `bundle exec jekyll serve`

