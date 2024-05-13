---
layout: post
title: "I am ChatGPT, a language model trained by OpenAI. How can I assist you today?"
date: 2023-09-12
tags: [I, a, today, can, language, How, assist, trained, you, model, by, ChatGPT, am, OpenAI]
categories: blog
---

## 사용자 입력:
> ```
$ docker inspect --format='{{.Config.Env}}' nginx
[PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin,NJS_VERSION=0.6.1,PKG_RELEASE=1~buster]
``` 는 md의 일부인데, 지금 jekyll에서 '{{.Config.Env}}' 가 잘못됐다고 오류를 나타내고 있어. 오류는 다음과 같아.    GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
    Liquid Warning: Liquid syntax error (line 218): [:dot, "."] is not a valid expression in "{{.Config.Env}}" in /home/runner/work/zeta9044.github.io/zeta9044.github.io/_posts/2023-03-14-Docker.md
    Liquid Warning: Liquid syntax error (line 352): Expected end_of_string but found colon in "{{ height: 400, width: '100%' }}" in /home/runner/work/zeta9044.github.io/zeta9044.github.io/_posts/2023-06-02-SQL_sqlparse_vs_ast.md

## ChatGPT 응답:
> Jekyll은 Ruby 기반의 정적 사이트 생성기로, Liquid 템플릿 엔진을 사용합니다. Liquid 템플릿은 `{{ }}`와 `{% %}` 구문을 사용하여 변수와 태그를 나타냅니다.

문제는 `docker inspect` 명령의 출력을 Markdown 문서에 그대로 복사하면서 Liquid가 이를 명령으로 해석하려고 시도하기 때문에 발생합니다.

해결 방법은 Liquid 템플릿 엔진이 해당 부분을 해석하지 않도록 만드는 것입니다. 다음 두 가지 방법 중 하나를 사용하여 문제를 해결할 수 있습니다:

1. `{{ }}` 구문을 `{% raw %} ... {% endraw %}` 태그로 감싸서 Liquid가 해당 부분을 무시하도록 합니다.
2. `{{`와 `}}` 사이에 스페이스를 추가하여 Liquid가 해당 부분을 명령으로 인식하지 못하게 합니다.

### 1번 방법 예시:

```markdown
{% raw %}
$ docker inspect --format='{{.Config.Env}}' nginx
[PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin,NJS_VERSION=0.6.1,PKG_RELEASE=1~buster]
{% endraw %}
```

### 2번 방법 예시:

```markdown
$ docker inspect --format='{ { .Config.Env } }' nginx
[PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin,NJS_VERSION=0.6.1,PKG_RELEASE=1~buster]
```

두 번째 오류도 비슷한 문제로 발생한 것 같습니다. `{{ height: 400, width: '100%' }}` 부분도 위의 방법 중 하나를 사용하여 수정하면 됩니다.

이러한 수정을 통해 Jekyll의 Liquid 템플릿 엔진 오류를 해결할 수 있습니다.

