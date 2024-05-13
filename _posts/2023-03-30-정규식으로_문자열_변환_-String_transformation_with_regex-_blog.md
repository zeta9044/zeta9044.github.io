---
layout: post
title: "정규식으로 문자열 변환 (String transformation with regex)"
date: 2023-03-30
tags: [문자열, 변환, transformation, 정규식, with, String, regex]
categories: blog
---

## 사용자 입력:
> 정규식을 만들고 싶은데 "" 왼쪽앞에 =이 아닌 경우는 모두 " 이걸로 바꾸고 싶어

## ChatGPT 응답:
> 아래와 같은 정규식을 사용하면 "" 왼쪽 앞에 =이 아닌 경우를 모두 "로 바꿀 수 있습니다.

```
(?<!\=)"
```

위 정규식의 의미는 다음과 같습니다.

- `(?<!\=)`: = 문자가 바로 앞에 오지 않는 경우에만 매치합니다. (?<!...) 은 Negative Lookbehind Assertion을 의미합니다.
- `"`: " 문자와 매치합니다.

따라서 이 정규식을 사용하면 다음과 같은 문자열에서

```
abc"def"=ghi"jkl"=mno"pqr"
```

다음과 같이 결과가 나옵니다.

```
abc"def"=ghi"jkl"=mno"pqr"
```

즉, = 문자 왼쪽에 있는 " 문자는 바뀌지 않고 그대로 유지되고, = 문자 오른쪽에 있는 " 문자는 "로 바뀌게 됩니다.

