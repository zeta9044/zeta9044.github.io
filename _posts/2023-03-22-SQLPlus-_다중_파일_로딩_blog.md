---
layout: post
title: "SQLPlus: 다중 파일 로딩"
date: 2023-03-22
tags: [SQLPlus, 다중, 파일, 로딩]
categories: blog
---

## 사용자 입력:
> sqlplus에서 여러개 파일을 한꺼번에 로딩하는 방법은?

## ChatGPT 응답:
> SQLPlus에서 여러 개의 파일을 한꺼번에 로딩하는 방법은 다음과 같습니다.

1. 로딩하려는 파일들을 하나의 디렉토리에 모아둡니다.

2. SQLPlus를 실행합니다.

3. SQLPlus에서 다음 명령어를 입력합니다.

```
@/path/to/directory/file1.sql
@/path/to/directory/file2.sql
@/path/to/directory/file3.sql
```

위의 명령어는 파일1.sql, 파일2.sql, 파일3.sql이 모두 /path/to/directory/ 디렉토리에 있고 SQLPlus에서 실행 중인 것으로 가정합니다. 파일들의 경로를 적절히 수정하여 사용하시면 됩니다.

4. 각 파일이 순차적으로 실행됩니다.

이 방법을 사용하여 SQLPlus에서 여러 파일을 한꺼번에 로딩할 수 있습니다.

