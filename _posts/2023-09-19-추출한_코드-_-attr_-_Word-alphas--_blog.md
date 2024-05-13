---
layout: post
title: "추출한 코드: "attr = Word(alphas)""
date: 2023-09-19
tags: [alphas, 코드, Word, 추출, attr]
categories: blog
---

## 사용자 입력:
> 첨부한 텍스트는 md파일의 일부분인데, 여기서 attr = Word(alphas) + "=" + QuotedString('"') 만 추출하는 파이썬 코드 만들어봐. 

## ChatGPT 응답:
> 먼저, 첨부하신 `md_test.txt` 파일의 내용을 확인하겠습니다.

## ChatGPT 응답:
> # Reading the content of the uploaded file
with open("/mnt/data/md_test.txt", "r", encoding="utf-8") as file:
    md_content = file.read()

md_content

## ChatGPT 응답:
> 첨부하신 파일 내용에는 다음과 같은 Python 코드가 포함되어 있습니다:

\[
\texttt{attr = Word(alphas) + "=" + QuotedString('\"')}
\]

이제 해당 내용을 추출하는 파이썬 코드를 작성하겠습니다.

## ChatGPT 응답:
> import re

# Using regex to extract the desired pattern
pattern = r'attr = Word\(alphas\) \+ "=" \+ QuotedString\(\'"\'\)'
match = re.search(pattern, md_content)

extracted_content = match.group(0) if match else None
extracted_content

## ChatGPT 응답:
> 추출한 내용은 다음과 같습니다:

\[
\texttt{attr = Word(alphas) + "=" + QuotedString('\"')}
\]

이제 이 내용을 기반으로 파이썬 코드를 제작하였습니다.

## 사용자 입력:
> 위 텍스트만 추출하는게 아니라, md형식의 코드 스트링만 추출하는 거잖아.

## ChatGPT 응답:
> 아, 죄송합니다. 이해를 잘못했네요.

Markdown 형식에서 코드는 보통 \````python ... \````와 같은 형식으로 감싸져 있습니다. 따라서 이러한 형식을 사용하여 코드 스트링만 추출하는 정규표현식을 작성하겠습니다.

## ChatGPT 응답:
> # Extracting code blocks from markdown content using regex
code_pattern = r'```python\n(.*?)\n```'
code_matches = re.findall(code_pattern, md_content, re.DOTALL)

code_blocks = code_matches[0] if code_matches else None
code_blocks

## ChatGPT 응답:
> Markdown 형식의 텍스트에서 코드 부분만을 추출했습니다:

\[
\texttt{attr = Word(alphas) + "=" + QuotedString('\"')}
\]

이렇게 코드 블록을 추출하는 방식을 사용하면, 다른 코드 블록도 동일한 방식으로 추출할 수 있습니다.

## 사용자 입력:
> 답답하네. 코드가 파이썬만 있냐? 다른 코드도 추출할 수 있게 해야지.

## ChatGPT 응답:
> 죄송합니다. 이해했습니다.

Markdown에서 코드 블록은 일반적으로 \````언어명 ... \```` 형식으로 작성됩니다. 이러한 형식을 기반으로 모든 코드 블록(언어에 관계없이)을 추출하도록 정규 표현식을 수정하겠습니다.

## ChatGPT 응답:
> # Extracting all code blocks from markdown content regardless of the language
all_code_pattern = r'```.*?\n(.*?)\n```'
all_code_matches = re.findall(all_code_pattern, md_content, re.DOTALL)

all_code_matches

## ChatGPT 응답:
> 이제 Markdown 텍스트에서 모든 코드 블록을 추출했습니다:

\[
\texttt{attr = Word(alphas) + "=" + QuotedString('\"')}
\]

이 정규 표현식을 사용하면, 언어에 상관없이 모든 코드 블록을 추출할 수 있습니다. 다시 한번 죄송합니다. 더 깔끔하게 요구사항을 이해하고 처리하겠습니다.

## 사용자 입력:
> 그러면, 위 추출한 코드를 {% raw %} {% endrow%}로 감싸서, jekyll의 liquid엔진에서 오류가 안나도록 처리하는 파이썬 코드 만들어.

## ChatGPT 응답:
> Jekyll의 Liquid 엔진에서 원시 코드 블록을 처리할 때, `{% raw %} ... {% endraw %}` 태그를 사용하면 됩니다.

따라서 추출한 코드 블록을 해당 태그로 감싸는 작업을 진행하겠습니다.

## ChatGPT 응답:
> # Wrapping the extracted code blocks with {% raw %} and {% endraw %} tags
wrapped_code_blocks = ["{% raw %}\n" + code + "\n{% endraw %}" for code in all_code_matches]

wrapped_code_blocks_str = "\n\n".join(wrapped_code_blocks)
wrapped_code_blocks_str

## ChatGPT 응답:
> 다음과 같이 추출한 코드 블록을 `{% raw %} ... {% endraw %}` 태그로 감쌌습니다:

```
{% raw %}
attr = Word(alphas) + "=" + QuotedString('"')
{% endraw %}
```

이렇게 처리하면 Jekyll의 Liquid 엔진에서 오류 없이 원시 코드 블록을 표시할 수 있습니다.

## 사용자 입력:
> 그러면, 마지막으로 첨부한 텍스트에서 원시코드블록만 {% raw %} {% endrow %}로 감싸서 치환하는 파이썬코드를 만들어봐.

## ChatGPT 응답:
> 알겠습니다. 첨부하신 텍스트에서 원시 코드 블록만을 찾아서 `{% raw %} ... {% endraw %}` 태그로 감싸는 코드를 작성하겠습니다. 

주의: `{% endrow %}`는 오타로 보이는데, `{% endraw %}`가 맞는 것 같습니다. 이에 따라 코드를 작성하겠습니다.

## ChatGPT 응답:
> # Function to wrap code blocks with {% raw %} and {% endraw %} tags
def wrap_code_blocks_with_raw(content):
    def replacement(match):
        return "```" + match.group(1) + "\n{% raw %}\n" + match.group(2) + "\n{% endraw %}\n```"
    
    # Using regex to replace code blocks with wrapped versions
    wrapped_content = re.sub(r'```(.*?)\n(.*?)\n```', replacement, content, flags=re.DOTALL)
    return wrapped_content

wrapped_md_content = wrap_code_blocks_with_raw(md_content)
wrapped_md_content

## ChatGPT 응답:
> 첨부하신 텍스트에서 원시 코드 블록을 찾아서 다음과 같이 `{% raw %} ... {% endraw %}` 태그로 감쌌습니다:

```
```python
{% raw %}
attr = Word(alphas) + "=" + QuotedString('"')
{% endraw %}
```

이렇게 처리하면 Jekyll의 Liquid 엔진에서 원시 코드 블록을 오류 없이 표시할 수 있습니다.

