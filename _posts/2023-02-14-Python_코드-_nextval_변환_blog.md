---
layout: post
title: "Python 코드: nextval 변환"
date: 2023-02-14
tags: [nextval, 변환, 코드, Python]
categories: blog
---

## 사용자 입력:
> nextval('${tempSequenceName}')을 대소문자관계없이 ${tempSequenceName}.NEXTVAL로 re를 이용해서 바꾸는 python code 알려줘

## ChatGPT 응답:
> 다음은 대소문자 관계없이 주어진 문자열에서 'nextval'을 '.NEXTVAL'로 대체하는 Python 코드입니다.

```python
import re

tempSequenceName = '${tempSequenceName}'
pattern = re.compile(r'nextval', re.IGNORECASE)
result = re.sub(pattern, '.NEXTVAL', tempSequenceName)

print(result)
```

이 코드는 주어진 문자열에서 'nextval' 패턴을 대소문자를 구분하지 않고 찾아서 '.NEXTVAL'로 대체하고 결과를 출력합니다. 패턴은 정규식 표현을 사용하여 re.compile 함수를 사용해 미리 컴파일했습니다.

