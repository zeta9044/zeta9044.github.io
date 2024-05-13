---
layout: post
title: "Liquid 엔진 코드 감싸기"
date: 2023-09-14
tags: [Liquid, 엔진, 코드]
categories: blog
---

## 사용자 입력:
> ```xml    something  ``` 이것은 md의 코드 문장인데,  jekyll에서 쓸 거라서 something을 {% raw %} ,{% endraw %} 로 감싸서,  liquid엔진에서 오류가 나지 않게 끔 하고, 싶어. python으로 코딩하면, 어떻게 해야 될까?

## ChatGPT 응답:
> Jekyll에서 Liquid 엔진을 사용할 때, raw 태그를 사용하면 Liquid 명령어를 그대로 문자열로 출력할 수 있습니다. 따라서 XML이나 다른 코드를 포함하고 있는 Markdown 문서를 Jekyll에서 처리하려면, 이러한 코드를 `{% raw %}`와 `{% endraw %}` 태그로 감싸야 합니다.

주어진 XML 코드를 `{% raw %}`와 `{% endraw %}` 태그로 감싸는 Python 함수를 작성해 보겠습니다:

## ChatGPT 응답:
> def wrap_with_raw_tags(xml_string):
    return "{% raw %}" + xml_string + "{% endraw %}"

xml_code = "```xml    something  ```"
wrapped_code = wrap_with_raw_tags(xml_code)
wrapped_code

## ChatGPT 응답:
> 다음과 같이 변환되었습니다:

```
{% raw %}```xml    something  ```{% endraw %}
```

이렇게 하면 Jekyll의 Liquid 엔진에서 코드를 그대로 문자열로 처리하여 오류를 방지할 수 있습니다.

## 사용자 입력:
> 그러면, md파일에서 코드 문장을  {% raw %} ,{% endraw %} 로 감싸서 변환하는  python 예제 보여줘.

## ChatGPT 응답:
> 물론이죠. Markdown 파일에서 코드 블록을 찾아 해당 코드 블록을 `{% raw %}`와 `{% endraw %}` 태그로 감싸는 Python 코드를 작성해 보겠습니다.

다음은 이를 위한 간단한 절차입니다:

1. Markdown 파일을 읽습니다.
2. 각 코드 블록을 찾습니다.
3. 코드 블록을 `{% raw %}`와 `{% endraw %}` 태그로 감싸줍니다.
4. 변환된 내용을 출력하거나 새로운 파일로 저장합니다.

이제 이 절차에 따라 Python 코드를 작성해 보겠습니다.

## ChatGPT 응답:
> import re

def wrap_code_blocks_with_raw_tags(md_content):
    # 정규 표현식을 사용하여 코드 블록을 찾습니다.
    code_block_pattern = re.compile(r'(```.*?```)', re.DOTALL)
    
    # 코드 블록을 찾아 raw 태그로 감싸줍니다.
    wrapped_content = code_block_pattern.sub(lambda m: wrap_with_raw_tags(m.group(1)), md_content)
    
    return wrapped_content

# 예제 Markdown 파일 내용
example_md_content = """
# This is a sample markdown

Some text here.

```python
def hello_world():
    print("Hello, world!")
```

More text.

```xml
<sample>
    <tag>Hello</tag>
</sample>
```

End of the file.
"""

wrapped_md_content = wrap_code_blocks_with_raw_tags(example_md_content)
print(wrapped_md_content)

## ChatGPT 응답:
> 다음은 변환된 Markdown 내용입니다:

```
# This is a sample markdown

Some text here.

{% raw %}```python
def hello_world():
    print("Hello, world!")
```{% endraw %}

More text.

{% raw %}```xml
<sample>
    <tag>Hello</tag>
</sample>
```{% endraw %}

End of the file.
```

이렇게 하면 Jekyll에서 이 Markdown 파일을 처리할 때 Liquid 엔진 오류를 피할 수 있습니다. 

실제로 사용하실 때는 `wrap_code_blocks_with_raw_tags` 함수를 사용하여 Markdown 파일 내용을 변환한 후 저장하시면 됩니다.

