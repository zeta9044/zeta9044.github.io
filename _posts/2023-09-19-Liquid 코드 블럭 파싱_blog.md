---
layout: post
title: "Liquid 코드 블럭 파싱"
date: 2023-09-19
tags: [Liquid, 블럭, 파싱, 코드]
categories: blog
---

## 사용자 입력:
> jekyll의 liquid 오류를 없애기 위해서,   md 내용애서 코드블럭 전 까지는 {% raw %} {% endraw %}로 감싸고, 코드블럭은 코드스트링만 {% raw %} {% endraw %}로 감싸고, 코드블럭이 이후는 {% raw %} {% endraw %}로 감싸는 파이썬 코드 만들어봐.

## ChatGPT 응답:
> Jekyll의 Liquid 템플릿 엔진에서 `{% raw %}`와 `{% endraw %}`는 Liquid 코드를 escape하기 위해 사용됩니다. 그러므로, 주어진 Markdown 파일에서 코드 블럭 밖의 모든 내용을 `{% raw %}`와 `{% endraw %}`로 감싸고, 코드 블럭 내의 내용만 감싸기 위한 파이썬 코드를 작성하겠습니다.

코드 블럭은 일반적으로 아래와 같은 형식을 가집니다:
\```
코드 내용
\```

이를 기반으로 작성한 파이썬 코드는 다음과 같습니다:

## ChatGPT 응답:
> import re

def wrap_content_with_raw(content):
    # 코드 블럭 패턴 찾기
    code_blocks = re.findall(r'```([\s\S]*?)```', content)
    
    # 코드 블럭을 split하여 코드 블럭과 그 외의 내용 분리
    parts = re.split(r'```[\s\S]*?```', content)
    
    wrapped_content = []
    
    # parts와 code_blocks를 교대로 추가
    for i, part in enumerate(parts):
        # 코드 블럭이 아닌 부분은 전체를 {% raw %}와 {% endraw %}로 감싼다
        wrapped_content.append("{% raw %}" + part + "{% endraw %}")
        
        # code_blocks의 내용이 더 남아있다면, 해당 코드 블럭을 추가
        if i < len(code_blocks):
            # 코드 블럭 내부의 내용만 {% raw %}와 {% endraw %}로 감싼다
            code_inside = code_blocks[i]
            wrapped_code = "```{% raw %}" + code_inside + "{% endraw %}```"
            wrapped_content.append(wrapped_code)
    
    return ''.join(wrapped_content)

# 예제
sample_md = """
Hello, this is a markdown file.
It contains some text and code.

```python
print("Hello, World!")
```

And this is some text after the code block.
"""

wrapped_md = wrap_content_with_raw(sample_md)
wrapped_md

## ChatGPT 응답:
> 주어진 예제에 대한 결과는 다음과 같습니다:

```
{% raw %}
Hello, this is a markdown file.
It contains some text and code.

{% endraw %}```{% raw %}python
print("Hello, World!")
{% endraw %}```{% raw %}

And this is some text after the code block.
{% endraw %}
```

이 코드는 Markdown 파일 내의 코드 블럭 밖의 내용을 `{% raw %}`와 `{% endraw %}`로 감싸며, 코드 블럭 내부의 내용만 `{% raw %}`와 `{% endraw %}`로 감쌉니다.

이러한 방식으로 원하는 Markdown 파일의 내용을 변환하실 수 있습니다.

