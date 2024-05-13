---
layout: post
title: "HTML to Markdown Converter"
date: 2023-07-06
tags: [Markdown, Converter, HTML, to]
categories: blog
---

## 사용자 입력:
> html을 마크다운으로 변환해주는 툴이 있나?

## ChatGPT 응답:
> 네, HTML을 마크다운으로 변환해주는 다양한 도구와 라이브러리가 있습니다. 이러한 도구와 라이브러리를 사용하면 HTML 코드를 마크다운 형식으로 쉽게 변환할 수 있습니다. 

여기 몇 가지 인기있는 HTML을 마크다운으로 변환해주는 도구와 라이브러리를 소개해드리겠습니다:

1. Pandoc: Pandoc은 다양한 문서 형식 간 변환을 지원하는 명령 줄 도구입니다. HTML을 마크다운으로 변환하는 기능도 제공합니다. Pandoc을 사용하려면 명령 줄에서 실행해야 하지만, 강력하고 유연한 변환 기능을 제공합니다.

2. html2markdown: html2markdown은 JavaScript 기반의 온라인 도구로, 웹 브라우저에서 HTML 코드를 마크다운으로 변환할 수 있습니다. 브라우저 상에서 작동하기 때문에 별도의 설치 과정이 필요하지 않습니다.

3. Turndown: Turndown은 JavaScript용 마크다운 변환기입니다. Node.js와 브라우저 모두에서 사용할 수 있으며, HTML을 마크다운으로 변환하는 기능을 제공합니다.

4. Showdown: Showdown은 또 다른 JavaScript 기반의 마크다운 변환기입니다. HTML을 마크다운으로 변환하는 기능을 제공하며, 브라우저와 Node.js에서 모두 사용할 수 있습니다.

이러한 도구와 라이브러리는 각각 다른 기능과 특성을 가지고 있으니 사용 시 참고해보시고 자신에게 가장 적합한 도구를 선택하시면 됩니다.

## 사용자 입력:
> 파이썬으로 html을 마크다운으로 변환해주는 프로그램 만들어줘.

## ChatGPT 응답:
> 물론입니다! 파이썬에서 HTML을 마크다운으로 변환하는 프로그램을 만들 수 있습니다. 이를 위해 `beautifulsoup4`와 `html2text` 라이브러리를 사용할 것입니다. `beautifulsoup4`는 HTML 파싱을 위한 라이브러리이고, `html2text`는 HTML을 마크다운으로 변환해주는 라이브러리입니다. 먼저 이 두 라이브러리를 설치해야 합니다.

터미널 또는 명령 프롬프트에서 다음 명령을 실행하여 라이브러리를 설치합니다:

```
pip install beautifulsoup4 html2text
```

그런 다음, 다음과 같이 파이썬 코드를 작성하여 HTML을 마크다운으로 변환하는 프로그램을 만들 수 있습니다:

```python
from bs4 import BeautifulSoup
import html2text

def html_to_markdown(html):
    # BeautifulSoup을 사용하여 HTML 파싱
    soup = BeautifulSoup(html, 'html.parser')

    # 마크다운으로 변환
    markdown = html2text.html2text(str(soup))

    return markdown

# 변환할 HTML 코드
html = '''
<html>
<head>
<title>HTML to Markdown</title>
</head>
<body>
<h1>HTML to Markdown</h1>
<p>This is a sample HTML code.</p>
<ul>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ul>
</body>
</html>
'''

# HTML을 마크다운으로 변환
markdown = html_to_markdown(html)

# 변환된 마크다운 출력
print(markdown)
```

위의 코드를 실행하면, 입력한 HTML 코드가 마크다운으로 변환된 결과가 출력됩니다. 이를 원하는 대로 활용하시면 됩니다.

## 사용자 입력:
> 파일을 받아서 변환해주는 걸로 발전시켜줘.

## ChatGPT 응답:
> 물론입니다! 파일을 받아서 변환하는 기능을 추가해보겠습니다. 다음과 같이 코드를 수정하여 파일을 읽고 HTML을 마크다운으로 변환하는 프로그램을 만들 수 있습니다:

```python
from bs4 import BeautifulSoup
import html2text

def html_to_markdown(html):
    # BeautifulSoup을 사용하여 HTML 파싱
    soup = BeautifulSoup(html, 'html.parser')

    # 마크다운으로 변환
    markdown = html2text.html2text(str(soup))

    return markdown

def convert_file_to_markdown(file_path):
    try:
        # 파일 열기
        with open(file_path, 'r') as file:
            # 파일 내용 읽기
            html = file.read()

            # HTML을 마크다운으로 변환
            markdown = html_to_markdown(html)

            return markdown
    except FileNotFoundError:
        print("파일을 찾을 수 없습니다.")
        return None

# 변환할 HTML 파일 경로
file_path = 'input.html'

# HTML 파일을 마크다운으로 변환
markdown = convert_file_to_markdown(file_path)

if markdown:
    # 변환된 마크다운 출력
    print(markdown)
```

위의 코드에서 `convert_file_to_markdown` 함수는 파일 경로를 매개변수로 받아서 파일을 열고 읽은 후, HTML을 마크다운으로 변환하여 반환합니다. 파일이 존재하지 않을 경우에 대한 예외 처리도 추가되었습니다.

위 코드를 실행하기 전에 `input.html` 파일을 현재 작업 디렉토리에 준비하고, 해당 파일에 변환할 HTML 코드를 작성해주세요. 그런 다음 코드를 실행하면 파일의 내용이 마크다운으로 변환되어 출력됩니다.

## 사용자 입력:
> cmd에서 실행시킬때, 파일을 인수로 받아서 처리하게 해줘.

## ChatGPT 응답:
> 파일을 인수로 받아서 처리하는 방법은 `argparse` 라이브러리를 사용하여 구현할 수 있습니다. `argparse`는 명령 줄 인수를 파싱하는데 유용한 기능을 제공합니다. 아래의 코드는 `argparse`를 사용하여 파일을 인수로 받아 처리하는 프로그램입니다:

```python
import argparse
from bs4 import BeautifulSoup
import html2text

def html_to_markdown(html):
    # BeautifulSoup을 사용하여 HTML 파싱
    soup = BeautifulSoup(html, 'html.parser')

    # 마크다운으로 변환
    markdown = html2text.html2text(str(soup))

    return markdown

def convert_file_to_markdown(file_path):
    try:
        # 파일 열기
        with open(file_path, 'r') as file:
            # 파일 내용 읽기
            html = file.read()

            # HTML을 마크다운으로 변환
            markdown = html_to_markdown(html)

            return markdown
    except FileNotFoundError:
        print("파일을 찾을 수 없습니다.")
        return None

# 명령 줄 인수 파싱
parser = argparse.ArgumentParser(description='HTML 파일을 마크다운으로 변환하는 프로그램')
parser.add_argument('file', metavar='FILE', help='변환할 HTML 파일 경로')
args = parser.parse_args()

# HTML 파일을 마크다운으로 변환
markdown = convert_file_to_markdown(args.file)

if markdown:
    # 변환된 마크다운 출력
    print(markdown)
```

위의 코드를 실행할 때, 파일 경로를 인수로 함께 지정해주면 됩니다. 예를 들어, 다음과 같이 실행할 수 있습니다:

```
python convert_html_to_markdown.py input.html
```

위 예제에서 `convert_html_to_markdown.py`는 파이썬 스크립트 파일의 이름이며, `input.html`은 변환할 HTML 파일의 경로입니다. 해당 파일의 내용이 마크다운으로 변환되어 출력됩니다.

## 사용자 입력:
> if __main__이런거는 어떤 때 필요한 거지?

## ChatGPT 응답:
> `if __name__ == "__main__"` 구문은 파이썬에서 일반적으로 사용되는 패턴 중 하나입니다. 이 구문은 스크립트 파일이 직접 실행될 때에만 내부의 코드 블록을 실행하도록 하는 역할을 합니다. 이를테면, 모듈로서 다른 스크립트에서 임포트될 때는 내부 코드 블록이 실행되지 않습니다.

`if __name__ == "__main__"` 구문의 사용 이점은 다음과 같습니다:

1. 모듈과 스크립트의 분리: `if __name__ == "__main__"` 구문을 사용하면, 스크립트 파일이 직접 실행될 때는 내부의 코드 블록이 실행되지만, 모듈로서 다른 스크립트에서 임포트될 때는 해당 코드 블록이 실행되지 않습니다. 이를 통해 스크립트 파일을 모듈과 구분하여 사용할 수 있습니다.

2. 테스트 코드 실행: `if __name__ == "__main__"` 구문은 스크립트 파일을 실행할 때만 내부 코드 블록을 실행하므로, 스크립트 파일에 포함된 테스트 코드를 직접 실행할 수 있습니다. 이는 스크립트 파일을 모듈로서 사용하면서도 내부의 테스트를 수행할 수 있는 편리한 방법입니다.

3. 가독성과 유지 보수성: `if __name__ == "__main__"` 구문은 스크립트 파일의 진입점을 명시적으로 표시해줍니다. 이는 코드의 가독성과 유지 보수성을 향상시킬 수 있습니다. 다른 개발자가 해당 파일을 읽을 때, 스크립트의 진입점을 파악하기 쉬워지며, 코드의 동작을 이해하는 데 도움이 됩니다.

따라서, `if __name__ == "__main__"` 구문은 스크립트 파일을 실행할 때 내부 코드 블록을 실행하는 관습적인 방식으로 사용되며, 모듈과 스크립트의 분리와 테스트 용도로 유용하게 활용될 수 있습니다.

