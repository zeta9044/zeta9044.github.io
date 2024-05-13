---
layout: post
title: "Code wrapping with raw tags"
date: 2023-09-19
tags: [with, tags, raw, wrapping, Code]
categories: blog
---

## 사용자 입력:
> def wrap_code_blocks_with_raw_tags(md_content):
    # return wrapped_content
    def replacement(match):
        return "```" + match.group(1) + "\n{% raw %}\n" + match.group(2) + "\n{% endraw %}\n```"
    
    # Using regex to replace code blocks with wrapped versions
    wrapped_content = re.sub(r'```(.*?)\n(.*?)\n```', replacement, md_content, flags=re.DOTALL)
    return wrapped_content

def wrap_content_with_raw(content):
    # 코드 블럭 패턴 찾기
    code_blocks = re.findall(r'```(.*?)\n(.*?)\n```', content)
    
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
            # code_inside = code_blocks[i]
            # wrapped_code = "```\n{% raw %}" + code_inside + "{% endraw %}\n```\n"
            wrapped_code = wrap_code_blocks_with_raw_tags(code_blocks[i])
            wrapped_content.append(wrapped_code)
    
    return ''.join(wrapped_content)   이 코드의 잘 못된 점 수정.

## ChatGPT 응답:
> The function `wrap_content_with_raw` seems to be designed to wrap non-code blocks with `{% raw %}` tags and code blocks with `{% raw %}` tags only inside the code block. However, there are several issues with the given code:

1. The regular expression in the `wrap_code_blocks_with_raw_tags` function is designed to match code blocks, but the replacement function is again wrapping the code blocks with backticks. This will result in duplicated backticks for code blocks when used inside the `wrap_content_with_raw` function.
2. The `wrap_code_blocks_with_raw_tags` function is being used incorrectly inside the `wrap_content_with_raw` function.
3. The `re` module has not been imported.

Here's a corrected version of your code:

```python
import re

def wrap_code_blocks_with_raw_tags(md_content):
    def replacement(match):
        return "```" + match.group(1) + "\n{% raw %}\n" + match.group(2) + "\n{% endraw %}\n```"
    
    # Using regex to replace code blocks with wrapped versions
    wrapped_content = re.sub(r'```(.*?)\n(.*?)\n```', replacement, md_content, flags=re.DOTALL)
    return wrapped_content

def wrap_content_with_raw(content):
    # 코드 블럭 패턴 찾기
    code_blocks = re.findall(r'```(.*?)\n(.*?)\n```', content, flags=re.DOTALL)
    
    # 코드 블럭을 split하여 코드 블럭과 그 외의 내용 분리
    parts = re.split(r'```[\s\S]*?```', content)
    
    wrapped_content = []
    
    # parts와 code_blocks를 교대로 추가
    for i, part in enumerate(parts):
        # 코드 블럭이 아닌 부분은 전체를 {% raw %}와 {% endraw %}로 감싼다
        wrapped_content.append("{% raw %}" + part + "{% endraw %}")
        
        # code_blocks의 내용이 더 남아있다면, 해당 코드 블럭을 추가
        if i < len(code_blocks):
            lang, code = code_blocks[i]
            wrapped_code = "```" + lang + "\n{% raw %}\n" + code + "\n{% endraw %}\n```"
            wrapped_content.append(wrapped_code)
    
    return ''.join(wrapped_content)
```

This should work correctly for wrapping the content with the `{% raw %}` tags.

