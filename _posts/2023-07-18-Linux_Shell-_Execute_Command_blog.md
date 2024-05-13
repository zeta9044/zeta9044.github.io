---
layout: post
title: "Linux Shell: Execute Command"
date: 2023-07-18
tags: [Command, Shell, Linux, Execute]
categories: blog
---

## 사용자 입력:
> 리눅스 쉘에서 pentaho-parser디렉토리가 있으면, cd pentaho-parser  && ../gradlew compileJjtree compileJavacc clean build -x test 이 명령을 실행하도록 하는 것 알려줘.

## ChatGPT 응답:
> 리눅스 쉘에서 다음과 같은 명령을 사용하여 pentaho-parser 디렉토리로 이동한 후 해당 명령을 실행할 수 있습니다:

```shell
#!/bin/bash

if [ -d "pentaho-parser" ]; then
  cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test
else
  echo "pentaho-parser 디렉토리가 존재하지 않습니다."
fi
```

위 스크립트는 현재 디렉토리에서 pentaho-parser 디렉토리가 있는지 확인하고, 디렉토리가 존재하는 경우 해당 디렉토리로 이동한 후 명령을 실행합니다. 디렉토리가 존재하지 않는 경우 오류 메시지를 출력합니다.

위 스크립트를 텍스트 파일로 저장한 뒤 실행 가능한 스크립트로 만들려면 다음과 같은 명령을 실행합니다:

```shell
chmod +x script.sh
```

스크립트를 실행하려면 다음과 같이 입력합니다:

```shell
./script.sh
```

위 스크립트가 pentaho-parser 디렉토리에 있는 경우 해당 디렉토리로 이동한 후 명령이 실행됩니다.

## 사용자 입력:
> 한줄짜리 명령으로 만들어줘.

## ChatGPT 응답:
> 다음은 한 줄로 작성된 명령입니다:

```shell
[ -d "pentaho-parser" ] && cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test
```

이 명령은 "pentaho-parser" 디렉토리가 존재하는지 확인하고, 디렉토리가 존재하는 경우 해당 디렉토리로 이동한 후 지정한 명령을 실행합니다. 위의 스크립트와 동일한 작업을 수행하지만 한 줄로 작성되었습니다.

