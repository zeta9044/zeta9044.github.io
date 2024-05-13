---
layout: post
title: "Skip Liquid Errors in Jekyll"
date: 2023-09-27
tags: [Liquid, Errors, Jekyll, in, Skip]
categories: blog
---

## 사용자 입력:
> # This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

# Sample workflow for building and deploying a Jekyll site to GitHub Pages
name: Deploy Jekyll site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: write
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Ruby
        uses: ruby/setup-ruby@55283cc23133118229fd3f97f9336ee23a179fcf # v1.146.0
        with:
          ruby-version: '3.1' # Not needed with a .ruby-version file
          bundler-cache: true # runs 'bundle install' and caches installed gems automatically
          cache-version: 0 # Increment this number if you need to re-download cached gems
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Build with Jekyll
        # Outputs to the './_site' directory by default
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        # Automatically uploads an artifact from the './_site' directory by default
        uses: actions/upload-pages-artifact@v2

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2  이것은 github 페이지를 jekyll로 배포하는 건데, liquid엔진 문법에 오류나는 것은 skip하고 빌드하게 만들 수 없나?

## ChatGPT 응답:
> To skip files that might have Liquid syntax errors during the Jekyll build process, you can't directly ask Jekyll to skip files with errors. Instead, you have to approach this in a roundabout way.

Here are some steps you could follow:

1. **Use a Script to Validate Liquid Syntax**: Before the build process, run a script that checks all files with Liquid syntax for errors.
2. **Exclude Files with Errors**: If a file has a Liquid syntax error, you can either:
    - Move it to a different directory.
    - Add its path to the `exclude` array in `_config.yml`.

3. **Proceed with the Jekyll Build**: Once files with errors are excluded, proceed with the normal Jekyll build process.

Let's sketch this out in the GitHub Actions workflow:

1. Add a step that checks for Liquid syntax errors.
2. If errors are found, move those files or add them to the `exclude` list.

Here's a rough idea of how you might modify the workflow to achieve this:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # ... other steps ...

      - name: Validate Liquid and Exclude Files with Errors
        run: |
          # This is a pseudo-code. You'd have to create an actual script that does this.
          for file in $(find . -name "*.html" -or -name "*.md"); do
            # Check the file for Liquid syntax errors (using a tool or a script)
            # If there's an error, do one of the following:
            # - Move the file to a different directory
            # - Add the file to the exclude list in _config.yml
          done

      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production

      # ... other steps ...
```

The trickiest part here is actually validating the Liquid syntax. Jekyll doesn't provide a standalone validator for Liquid syntax. You might need to use the Liquid gem directly in a Ruby script to try and parse each file and see if any errors are thrown.

In summary, while this method is a bit roundabout, it does give you the desired outcome: files with Liquid errors are excluded from the build process.

## 사용자 입력:
> 한글로 설명해.

## ChatGPT 응답:
> 죄송합니다. 다시 한글로 설명 드리겠습니다.

Jekyll 빌드 프로세스 중에 Liquid 구문 오류가 있는 파일을 건너뛰는 기능은 Jekyll에 직접적으로 내장되어 있지 않습니다. 따라서 간접적인 방법을 사용해야 합니다.

다음과 같은 방법을 사용할 수 있습니다:

1. **Liquid 구문 검사 스크립트 사용**: 빌드 프로세스 전에 Liquid 구문이 있는 모든 파일을 검사하는 스크립트를 실행합니다.
2. **오류가 있는 파일 제외**: 파일에 Liquid 구문 오류가 있으면 다음 중 하나를 수행합니다:
    - 파일을 다른 디렉토리로 이동합니다.
    - `_config.yml`의 `exclude` 배열에 해당 파일의 경로를 추가합니다.
3. **Jekyll 빌드 진행**: 오류가 있는 파일을 제외한 후, 평소와 같은 Jekyll 빌드 프로세스를 진행합니다.

GitHub action 워크플로우를 이러한 방식으로 수정하는 방법:

1. Liquid 구문 오류를 검사하는 단계를 추가합니다.
2. 오류가 발견되면 해당 파일을 이동하거나 제외 목록에 추가합니다.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # ... 기타 단계 ...

      - name: Liquid 검증 및 오류 파일 제외
        run: |
          # 이것은 의사 코드입니다. 실제로 이 작업을 수행하는 스크립트를 만들어야 합니다.
          for file in $(find . -name "*.html" -or -name "*.md"); do
            # Liquid 구문 오류를 검사합니다 (툴이나 스크립트를 사용하여)
            # 오류가 있으면 다음 중 하나를 수행:
            # - 파일을 다른 디렉토리로 이동
            # - _config.yml의 exclude 목록에 파일 추가
          done

      - name: Jekyll로 빌드
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production

      # ... 기타 단계 ...
```

이 방법의 핵심은 Liquid 구문을 검증하는 것입니다. Jekyll은 Liquid 구문을 독립적으로 검증하는 기능을 제공하지 않기 때문에, 직접 Ruby 스크립트를 작성하여 Liquid gem을 사용하여 각 파일을 파싱하고 오류가 발생하는지 확인해야 할 수도 있습니다.

요약하면, 이 방법은 간접적이지만 원하는 결과를 얻을 수 있습니다: Liquid 오류가 있는 파일은 빌드 프로세스에서 제외됩니다.

