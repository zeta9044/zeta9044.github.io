---
layout: post
title: "Jekyll에서 iframe 사용"
date: 2023-09-07
tags: [iframe, 사용]
categories: blog
---

## 사용자 입력:
> jekyll 의 md파일에서 iframe을 넣을 려면?

## ChatGPT 응답:
> Jekyll에서 `.md` (Markdown) 파일에 `iframe`을 삽입하려면, HTML을 직접 사용하면 됩니다. 

예를 들어, YouTube 동영상을 임베드하려면:

```markdown
# 제목

내용...

<iframe width="560" height="315" src="https://www.youtube.com/embed/VIDEO_ID" frameborder="0" allowfullscreen></iframe>

더 많은 내용...
```

몇 가지 주의 사항:

1. **Markdown과 HTML 혼합**: Markdown 내에서 직접 HTML을 사용할 수 있으므로, `iframe` 태그를 직접 삽입하는 것은 문제가 되지 않습니다.
2. **보안**: 사이트에 임베드된 `iframe`의 출처를 신뢰할 수 있는지 확인하세요. 미확인 소스의 `iframe`은 웹사이트의 보안을 위협할 수 있습니다.
3. **레이아웃**: `iframe`의 너비와 높이를 조절하여 웹사이트의 레이아웃에 적합하게 만드십시오.
4. **렌더링**: 모든 Jekyll 플러그인이나 설정이 `iframe`을 올바르게 처리하지 않을 수 있으므로, 변경 사항을 저장한 후 웹사이트를 미리 보고 `iframe`이 올바르게 렌더링되는지 확인하십시오.

마지막으로, 일부 Jekyll 테마나 설정은 내장된 HTML을 다르게 처리할 수 있으므로, `iframe`을 사용하기 전에 해당 테마나 설정의 문서를 확인하는 것이 좋습니다.

