---
layout: post
title: "정규식으로 declarationseq 검색"
tags: [검색, 정규식, declarationseq]
---

## 사용자 입력:
> declarationseq: declaration+;  문자열은 declaration이 declarationseq에 포함되어 있는데, 단독으로 사용되는 걸 찾기 위한 정규식 알려줘.

## ChatGPT 응답:
> 문자열 {% raw %}`declaration`{% endraw %}이 {% raw %}`declarationseq`{% endraw %}에 포함되어 있지만, {% raw %}`declarationseq`{% endraw %} 안에서 {% raw %}`declaration`{% endraw %}이 단독으로 사용되는 경우를 찾기 위한 정규식을 작성하는 것은 꽤 흥미로운 문제입니다. {% raw %}`declarationseq`{% endraw %}는 하나 이상의 {% raw %}`declaration`{% endraw %}을 포함하므로, 우리는 {% raw %}`declaration`{% endraw %}이 다른 {% raw %}`declaration`{% endraw %}들과 연결되지 않은 상태로 존재하는 경우를 찾아야 합니다.

정규식으로 이를 표현하기 위해 다음과 같은 조건을 고려해야 합니다:

1. {% raw %}`declaration`{% endraw %} 앞뒤로 다른 문자가 오지 않는 경우를 찾아야 합니다.
2. {% raw %}`declaration`{% endraw %}은 {% raw %}`declarationseq`{% endraw %}의 시작이나 끝에 올 수 있으며, 또는 다른 {% raw %}`declaration`{% endraw %}들 사이에 있을 수도 있습니다.

이를 고려하여 정규식을 작성하면 다음과 같습니다:

{% raw %}```
\bdeclaration\b
```{% endraw %}

여기서 {% raw %}`\b`{% endraw %}는 단어 경계(word boundary)를 나타냅니다. 이는 {% raw %}`declaration`{% endraw %}이 다른 단어와 붙어있지 않고 독립적으로 존재할 때 일치하게 됩니다. 예를 들어, {% raw %}`declarationseq`{% endraw %}에서 {% raw %}`declaration`{% endraw %}이 다른 {% raw %}`declaration`{% endraw %}이나 다른 문자들과 붙어있지 않은 경우에만 이 정규식에 일치하게 됩니다.

## 사용자 입력:
> 뒤에 +는 올수 있어.

## ChatGPT 응답:
> {% raw %}`declaration`{% endraw %} 뒤에 {% raw %}`+`{% endraw %} 기호가 올 수 있다는 조건을 추가하면, 정규식은 다음과 같이 조금 수정됩니다:

{% raw %}```
\bdeclaration\+?\b
```{% endraw %}

이 정규식에서:

- {% raw %}`\b`{% endraw %}: 단어의 시작과 끝을 나타내는 경계를 의미합니다.
- {% raw %}`declaration`{% endraw %}: 찾고자 하는 문자열입니다.
- {% raw %}`\+?`{% endraw %}: {% raw %}`+`{% endraw %} 기호가 0회 또는 1회 나타날 수 있음을 나타냅니다. {% raw %}`\`{% endraw %}는 {% raw %}`+`{% endraw %}가 특수 문자로 해석되는 것을 방지하기 위해 사용됩니다.

이 정규식은 {% raw %}`declaration`{% endraw %}이 단독으로 나타나거나, 뒤에 {% raw %}`+`{% endraw %} 기호가 붙은 경우에 일치합니다. 단, {% raw %}`declaration`{% endraw %}의 앞이나 뒤에 다른 문자나 {% raw %}`declaration`{% endraw %}이 바로 연결되어 있지 않아야 합니다.

