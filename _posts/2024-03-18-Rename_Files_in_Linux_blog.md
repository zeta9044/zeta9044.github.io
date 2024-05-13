---
layout: post
title: "Rename Files in Linux"
date: 2024-03-18
tags: [Linux, Rename, Files, in]
categories: blog
---

## 사용자 입력:
> 리눅스 find로   ./include 디렉토리 밑에 확장자 붙지 않은 파일은 모두 h라는 확장자를 붙여서 이름바꾸는 방법 알려줘.

## ChatGPT 응답:
> 리눅스에서 `find` 명령어를 사용하여 `./include` 디렉토리 아래에 있는 확장자가 없는 파일들을 찾고, 그 파일들의 이름을 변경하는 스크립트를 작성할 수 있습니다. 이를 수행하기 위해 다음 단계를 따를 수 있습니다:

1. `find` 명령어로 `./include` 디렉토리 아래에 있는 확장자가 없는 파일들을 찾습니다.
2. 찾은 파일들의 이름을 변경합니다.

아래는 위 단계를 수행하는 스크립트의 예제입니다. 이 스크립트를 실행하면 `./include` 디렉토리 아래의 확장자가 없는 모든 파일의 이름이 `.h`로 끝나도록 변경됩니다.

```bash
#!/bin/bash

# 1. ./include 디렉토리 아래의 확장자가 없는 파일들을 찾습니다.
find ./include -type f ! -name '*.*' | while read filename; do
    # 2. 파일 이름에 .h 확장자를 추가하여 변경합니다.
    mv "$filename" "$filename.h"
    echo "Renamed: $filename -> $filename.h"
done
```

위 스크립트를 텍스트 파일로 저장한 후 실행 가능하도록 권한을 부여하고 실행하면 됩니다. 아래와 같이 명령어를 사용하여 스크립트 파일을 실행 가능하도록 설정할 수 있습니다:

```bash
chmod +x rename_files.sh
./rename_files.sh
```

이렇게 하면 `./include` 디렉토리 아래의 확장자가 없는 모든 파일들의 이름이 `.h`로 끝나도록 변경됩니다.

