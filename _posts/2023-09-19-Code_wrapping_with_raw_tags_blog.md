---
layout: post
title: "Code wrapping with raw tags"
tags: [Code, wrapping, tags, with, raw]
---

## 사용자 입력:
> def wrap_code_blocks_with_raw_tags(md_content):
    # return wrapped_content
    def replacement(match):
        return "{% raw %}```" + match.group(1) + "\n{% raw %}\n" + match.group(2) + "\n{% endraw %}\n```{% endraw %}"
    
    # Using regex to replace code blocks with wrapped versions
    wrapped_content = re.sub(r'{% raw %}```(.*?)\n(.*?)\n```{% endraw %}', replacement, md_content, flags=re.DOTALL)
    return wrapped_content

def wrap_content_with_raw(content):
    # 코드 블럭 패턴 찾기
    code_blocks = re.findall(r'{% raw %}```(.*?)\n(.*?)\n```{% endraw %}', content)
    
    # 코드 블럭을 split하여 코드 블럭과 그 외의 내용 분리
    parts = re.split(r'{% raw %}```[\s\S]*?```{% endraw %}', content)
    
    wrapped_content = []
    
    # parts와 code_blocks를 교대로 추가
    for i, part in enumerate(parts):
        # 코드 블럭이 아닌 부분은 전체를 {% raw %}와 {% endraw %}로 감싼다
        wrapped_content.append("{% raw %}" + part + "{% endraw %}")
        
        # code_blocks의 내용이 더 남아있다면, 해당 코드 블럭을 추가
        if i < len(code_blocks):
            # 코드 블럭 내부의 내용만 {% raw %}와 {% endraw %}로 감싼다
            # code_inside = code_blocks[i]
            # wrapped_code = "{% raw %}```\n{% raw %}" + code_inside + "{% endraw %}\n```{% endraw %}\n"
            wrapped_code = wrap_code_blocks_with_raw_tags(code_blocks[i])
            wrapped_content.append(wrapped_code)
    
    return ''.join(wrapped_content)   이 코드의 잘 못된 점 수정.

## ChatGPT 응답:
> The function {% raw %}`wrap_content_with_raw`{% endraw %} seems to be designed to wrap non-code blocks with {% raw %}`{% raw %}`{% endraw %} tags and code blocks with {% raw %}`{% raw %}`{% endraw %} tags only inside the code block. However, there are several issues with the given code:

1. The regular expression in the {% raw %}`wrap_code_blocks_with_raw_tags`{% endraw %} function is designed to match code blocks, but the replacement function is again wrapping the code blocks with backticks. This will result in duplicated backticks for code blocks when used inside the {% raw %}`wrap_content_with_raw`{% endraw %} function.
2. The {% raw %}`wrap_code_blocks_with_raw_tags`{% endraw %} function is being used incorrectly inside the {% raw %}`wrap_content_with_raw`{% endraw %} function.
3. The {% raw %}`re`{% endraw %} module has not been imported.

Here's a corrected version of your code:

{% raw %}```python
import re

def wrap_code_blocks_with_raw_tags(md_content):
    def replacement(match):
        return "```{% endraw %}" + match.group(1) + "\n{% raw %}\n" + match.group(2) + "\n{% endraw %}\n{% raw %}```"
    
    # Using regex to replace code blocks with wrapped versions
    wrapped_content = re.sub(r'```{% endraw %}(.*?)\n(.*?)\n{% raw %}```', replacement, md_content, flags=re.DOTALL)
    return wrapped_content

def wrap_content_with_raw(content):
    # 코드 블럭 패턴 찾기
    code_blocks = re.findall(r'```{% endraw %}(.*?)\n(.*?)\n{% raw %}```', content, flags=re.DOTALL)
    
    # 코드 블럭을 split하여 코드 블럭과 그 외의 내용 분리
    parts = re.split(r'```{% endraw %}[\s\S]*?{% raw %}```', content)
    
    wrapped_content = []
    
    # parts와 code_blocks를 교대로 추가
    for i, part in enumerate(parts):
        # 코드 블럭이 아닌 부분은 전체를 {% raw %}와 {% endraw %}로 감싼다
        wrapped_content.append("{% raw %}" + part + "{% endraw %}")
        
        # code_blocks의 내용이 더 남아있다면, 해당 코드 블럭을 추가
        if i < len(code_blocks):
            lang, code = code_blocks[i]
            wrapped_code = "```{% endraw %}" + lang + "\n{% raw %}\n" + code + "\n{% endraw %}\n{% raw %}```"
            wrapped_content.append(wrapped_code)
    
    return ''.join(wrapped_content)
```{% endraw %}

This should work correctly for wrapping the content with the {% raw %}`{% raw %}`{% endraw %} tags.

