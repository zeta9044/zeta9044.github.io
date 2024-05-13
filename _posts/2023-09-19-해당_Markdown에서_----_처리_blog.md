---
layout: post
title: "해당 Markdown에서 {{}} 처리"
tags: [해당, 처리]
---

## 사용자 입력:
> 코드 작성을 주제로 md를 작성하다 보면,  {{ 과 }}을 쓸 수 있는데, 이것이 jekyll의 liquid engine 에서 계속 에러가 나. 에러를 없도록 md를 작성하려면, 어떻게 해야 하나?

## ChatGPT 응답:
> Jekyll은 Liquid 템플릿 엔진을 사용하기 때문에 {% raw %}`{{`{% endraw %}와 {% raw %}`}}`{% endraw %}는 변수를 출력할 때 사용됩니다. 따라서 Markdown 파일 내에서 이러한 문자를 그대로 사용하려면 Jekyll이 그것들을 Liquid 코드로 해석하지 않도록 방지해야 합니다.

여기에 몇 가지 방법이 있습니다:

1. **raw 태그 사용하기**
   
   {% raw %}`{{`{% endraw %}와 {% raw %}`}}`{% endraw %}를 그대로 출력하고 싶을 때 {% raw %}`raw`{% endraw %}와 {% raw %}`endraw`{% endraw %} 태그 사이에 놓습니다. 이 방법은 Liquid가 해당 영역의 코드를 처리하지 않도록 합니다.
   
   {% raw %}```liquid
   {% raw %}
   {{ some code here }}
   {% endraw %}
   ```{% endraw %}

2. **문자열 치환을 사용해 표현하기**

   {% raw %}`{{`{% endraw %}를 {% raw %}`{ {`{% endraw %}와 같이 띄어쓰기를 넣어서 작성하면 Liquid는 이를 변수로 인식하지 않습니다. 또한, {% raw %}`}}`{% endraw %}도 {% raw %}`} }`{% endraw %}와 같이 띄어쓰기를 넣어서 작성할 수 있습니다.

3. **HTML 엔터티를 사용하기**

   {% raw %}`{{`{% endraw %}를 {% raw %}`&lbrace;&lbrace;`{% endraw %}로, {% raw %}`}}`{% endraw %}를 {% raw %}`&rbrace;&rbrace;`{% endraw %}로 치환해서 사용할 수 있습니다. 이 방법은 HTML에서 중괄호를 표현하기 위한 방법입니다.

이 중에서 가장 흔히 사용되는 방법은 {% raw %}`raw`{% endraw %} 태그를 사용하는 것입니다. 이 방법을 사용하면 Jekyll이 해당 부분의 코드를 처리하지 않도록 명시적으로 지정할 수 있습니다.

## 사용자 입력:
> 네가 말한 1번째 방법으로 했더니, {% raw %}```java 이런 식으로 지정하는 부분에서 오류가 발생해.

## ChatGPT 응답:
> `{% raw %}`와 `{% endraw %}` 태그를 사용할 때 코드 블록 (예: ```{% endraw %}java)와 함께 사용하면 문제가 발생할 수 있습니다. 특히, Liquid의 {% raw %}`raw`{% endraw %} 태그와 Markdown의 코드 블록 구문이 충돌하는 경우가 있습니다.

이 문제를 해결하기 위한 방법은 다음과 같습니다:

1. {% raw %}`raw`{% endraw %} 태그를 코드 블록 내부에서만 사용하기:

{% raw %}```liquid
```{% endraw %}java
{% raw %}
{{ some Java code that uses curly braces }}
{% endraw %}
{% raw %}```
```{% endraw %}

(참고: 위의 예제는 이 컨텍스트에서는 정확하게 렌더링되지 않을 수 있습니다. 실제 코드에서는 더 깔끔하게 보일 것입니다.)

2. 코드 블록을 사용하지 않고, 4개의 공백 또는 탭으로 들여쓰기를 사용하여 코드 블록을 만듭니다. 이렇게 하면 Markdown이 여전히 코드로 인식하지만, "{% raw %}```" 구문을 사용하지 않게 됩니다.

```{% endraw %}liquid
    {% raw %}
    public class Example {
        public static void main(String[] args) {
            System.out.println("Hello, World!");
        }
    }
    {% endraw %}
{% raw %}```

이 중 어느 방법이 당신의 상황에 가장 적합한지 확인하고, 그 방법을 사용해보세요.

## 사용자 입력:
> md안에 ```{% endraw %}markdown
    ---
    layout: post
    title: "Jekyll에서 excerpt_separator 사용하기"
    ---

    Jekyll은 정적 사이트 생성기입니다. 이 도구를 사용하면 Markdown, Liquid 등을 활용하여 동적인 웹사이트를 만들 수 있습니다.

    <!--more-->

    이 포스트에서는 {% raw %}`excerpt_separator`{% endraw %}의 사용법에 대해 자세히 설명합니다...
    {% raw %}```이런 코드가 있는데, ```{% endraw %}markdown 과 {% raw %}```사이에 있는 내용에 {% raw %} {% endrow%}를 삽입할 수 있는 파이썬 코드 알려줘. 이건 이전 질문과 연관있는 거야.

## ChatGPT 응답:
> import re

# Given markdown content
markdown_content = """
---
layout: post
title: "Jekyll에서 excerpt_separator 사용하기"
---

Jekyll은 정적 사이트 생성기입니다. 이 도구를 사용하면 Markdown, Liquid 등을 활용하여 동적인 웹사이트를 만들 수 있습니다.

<!--more-->

이 포스트에서는 `excerpt_separator`의 사용법에 대해 자세히 설명합니다...
"""

# Use regex to find content between ```{% endraw %}markdown and {% raw %}```
pattern = re.compile(r'```{% endraw %}markdown(.*?){% raw %}```', re.DOTALL)
match = pattern.search(markdown_content)

# If matched, wrap the content between {% raw %} and {% endraw %}
if match:
    wrapped_content = "{% raw %}" + match.group(1) + "{% endraw %}"
    markdown_content = markdown_content.replace(match.group(1), wrapped_content)

markdown_content

## ChatGPT 응답:
> 죄송합니다. 제가 원하시는 결과를 제대로 생성하지 못했습니다. 다시 한번 시도해 보겠습니다. 

이미 주어진 `markdown_content`에서 ```{% endraw %}markdown 과 {% raw %}``` 사이의 내용을 찾아서, 그 사이의 내용을 `{% raw %}`와 `{% endraw %}`로 감싸는 작업을 해보겠습니다.

## ChatGPT 응답:
> # Correctly wrap the content between ```{% endraw %}markdown and {% raw %}``` with {% raw %} and {% endraw %}
wrapped_content = pattern.sub(r'```{% endraw %}markdown{% raw %}\1{% endraw %}{% raw %}```', markdown_content)
wrapped_content

## ChatGPT 응답:
> 죄송합니다. 다시 한번 문제를 확인하겠습니다. 제가 예상했던 결과와 다르게 나왔네요. 

정확한 결과를 얻기 위해 코드를 다시 수정하겠습니다.

## ChatGPT 응답:
> # Use regex to correctly find content between ```{% endraw %}markdown and {% raw %}``` and wrap it with {% raw %} and {% endraw %}
wrapped_content = re.sub(r'(```{% endraw %}markdown)(.*?)({% raw %}```)',
                         r'\1{% raw %}\2{% endraw %}', 
                         markdown_content, 
                         flags=re.DOTALL)
wrapped_content

## ChatGPT 응답:
> 이번에도 원하는 결과를 얻지 못했습니다. 잘못된 부분을 찾아 수정해보겠습니다.

## ChatGPT 응답:
> # Modify the regex pattern to correctly identify and wrap the content between ```{% endraw %}markdown and {% raw %}``` with {% raw %} and {% endraw %}
pattern = re.compile(r'(```{% endraw %}markdown\n)(.*?)(\n{% raw %}```)', re.DOTALL)
wrapped_content = pattern.sub(r'\1{% raw %}\2{% endraw %}\3', markdown_content)
wrapped_content

## ChatGPT 응답:
> 죄송합니다. 여전히 원하시는 결과를 얻지 못했습니다.

제가 제공한 입력 샘플 `markdown_content`에서 ```{% endraw %}markdown과 {% raw %}``` 사이에 내용이 없기 때문에 문제가 발생한 것 같습니다. 입력 샘플을 수정하여 다시 시도해보겠습니다.

## ChatGPT 응답:
> # Update the markdown_content to include a sample content between ```{% endraw %}markdown and {% raw %}```
markdown_content = """
```{% endraw %}markdown
---
layout: post
title: "Jekyll에서 excerpt_separator 사용하기"
---

Jekyll은 정적 사이트 생성기입니다. 이 도구를 사용하면 Markdown, Liquid 등을 활용하여 동적인 웹사이트를 만들 수 있습니다.

<!--more-->

이 포스트에서는 {% raw %}`excerpt_separator`{% endraw %}의 사용법에 대해 자세히 설명합니다...
{% raw %}```
"""

# Using the modified regex pattern to correctly identify and wrap the content
wrapped_content = pattern.sub(r'\1{% raw %}\2{% endraw %}\3', markdown_content)
wrapped_content

## ChatGPT 응답:
> 드디어 원하시는 결과를 얻었습니다. 

```{% endraw %}markdown과 {% raw %}``` 사이의 내용을 `{% raw %}`와 `{% endraw %}`로 감싸는 파이썬 코드를 제공했습니다. 

아래는 수정된 내용입니다:

\[
\begin{{align*}}
\text{{```{% endraw %}markdown}} \\
\text{{\{% raw \%}}} \\
\text{{---}} \\
\text{{layout: post}} \\
\text{{title: "Jekyll에서 excerpt\_separator 사용하기"}} \\
\text{{---}} \\
\text{{Jekyll은 정적 사이트 생성기입니다. 이 도구를 사용하면 Markdown, Liquid 등을 활용하여 동적인 웹사이트를 만들 수 있습니다.}} \\
\text{{<!--more-->}} \\
\text{{이 포스트에서는 {% raw %}`excerpt\_separator`{% endraw %}의 사용법에 대해 자세히 설명합니다...}} \\
\text{{\{% endraw \%}}} \\
\text{{{% raw %}``{% endraw %}{% raw %}`}}
\end{{align*}}
\]

이 코드를 사용하여 `{% endraw %}.md` 파일 내의 다른 코드 블록 사이에도 적용할 수 있습니다.

