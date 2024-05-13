---
layout: post
title: "Escape Liquid code in Jekyll"
date: 2023-09-19
tags: [code, Escape, in, Jekyll, Liquid]
categories: blog
---

## 사용자 입력:
>  <div class="archive">
    {% unless page.header.overlay_color or page.header.overlay_image %}
      <h1 id="page-title" class="page__title">{{ page.title }}</h1>
    {% endunless %}
    
    {{ content }} 여기에서 jekyll의  liquid가 content 안의 내용을 그대로 사용하게 하는 방법은?

## ChatGPT 응답:
> Jekyll uses the Liquid templating language to process templates. The `{{ content }}` you see is a Liquid output markup, which outputs the content of the current page or post.

If you want the `{{ content }}` to display as-is (literally) without being processed by Liquid, you have to escape it. Here's how you can do that:

1. **Using the `raw` and `endraw` tags**: These tags allow you to escape a block of Liquid code.

```liquid
{% raw %}
{{ content }}
{% endraw %}
```

By surrounding `{{ content }}` with `{% raw %}` and `{% endraw %}`, the Liquid processor will ignore whatever is inside, and it will be rendered as-is in the resulting HTML.

2. **Using HTML entities**: You can replace `{` with `&#123;` and `}` with `&#125;` to prevent Liquid from recognizing and processing the markup.

```html
&#123;&#123; content &#125;&#125;
```

Using either of these methods will display `{{ content }}` literally in your rendered page.

## 사용자 입력:
> 한글로 설명해.

## ChatGPT 응답:
> 죄송합니다. 다시 한글로 설명하겠습니다.

Jekyll은 Liquid 템플릿 언어를 사용하여 템플릿을 처리합니다. `{{ content }}`는 Liquid 출력 마크업입니다. 이것은 현재 페이지나 게시물의 내용을 출력합니다.

`{{ content }}`를 그대로 (문자 그대로) 표시하려면 Liquid에서 처리되지 않도록 이를 이스케이프(escape)해야 합니다. 이것을 할 수 있는 방법은 다음과 같습니다:

1. **`raw` 및 `endraw` 태그 사용**: 이 태그들을 사용하면 Liquid 코드 블록을 이스케이프할 수 있습니다.

```liquid
{% raw %}
{{ content }}
{% endraw %}
```

`{{ content }}`를 `{% raw %}`와 `{% endraw %}`로 감싸면 Liquid 프로세서는 그 안의 내용을 무시하고 결과 HTML에서 그대로 표시됩니다.

2. **HTML 엔터티 사용**: `{`를 `&#123;`로, `}`를 `&#125;`로 바꿔서 Liquid가 마크업을 인식하고 처리하는 것을 방지할 수 있습니다.

```html
&#123;&#123; content &#125;&#125;
```

이 두 가지 방법 중 하나를 사용하면 렌더링된 페이지에서 `{{ content }}`를 그대로 표시할 수 있습니다.

## 사용자 입력:
> 그러면, content가 markup문법으로 되어 있는데 html로 렌더링 안한다는 건가?

## ChatGPT 응답:
> 아니요, 제가 설명을 명확하게 하지 못했습니다. 죄송합니다.

`{{ content }}` 태그 자체는 Jekyll과 Liquid에서 현재 페이지나 게시물의 내용을 출력하는 데 사용됩니다. 이 내용은 일반적으로 Markdown과 같은 마크업 언어로 작성되며, Jekyll은 이를 HTML로 변환하여 렌더링합니다.

제가 앞서 설명한 방법은 `{{ content }}` 태그 자체를 화면에 그대로 보여주기 위한 것입니다. 즉, 사용자에게 `{{ content }}`라는 문자열을 그대로 표시하려는 경우에 사용하는 방법입니다.

하지만 일반적인 경우, `{{ content }}` 태그는 Liquid에 의해 처리되어 해당 페이지나 게시물의 실제 내용을 HTML 형식으로 렌더링합니다.

## 사용자 입력:
> 네가 잘못이해하고 있네. 내가 보여준 코드는 jekyll의 템플릿이야. 즉, {{content}}이 모양 자체를 그대로 표시하라는게,  content안에 {{과 }}이런 문자가 있는데, 이걸 그대로 표시하란 얘기야.

## ChatGPT 응답:
> 아, 이해했습니다. 죄송합니다.

`{{ content }}` 내부의 내용에 Liquid 태그나 마크업(`{{`, `}}` 또는 `{%`, `%}` 등)이 포함되어 있고 이를 그대로 표시하려면 `raw`와 `endraw` 태그를 사용해야 합니다.

예를 들어, 내용 중간에 `{{ example }}`이라는 Liquid 코드가 포함되어 있다면 이를 그대로 표시하려면 다음과 같이 사용해야 합니다:

```liquid
{% raw %}
{{ content }}
{% endraw %}
```

이렇게 하면 `{{ content }}` 내부의 모든 Liquid 코드가 그대로 출력됩니다. 

즉, `{{ content }}` 내의 모든 Liquid 마크업을 무시하고 싶다면, 해당 내용을 `raw`와 `endraw` 태그로 감싸면 됩니다.

