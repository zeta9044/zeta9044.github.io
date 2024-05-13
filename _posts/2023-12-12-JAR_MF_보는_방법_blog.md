---
layout: post
title: "JAR MF 보는 방법"
date: 2023-12-12
tags: [JAR, MF, 방법]
categories: blog
---

## 사용자 입력:
> jar명령어로 MF보는 법

## ChatGPT 응답:
> `jar` 명령어를 사용하여 Java ARchive (JAR) 파일 내의 Manifest 파일(MF)을 보는 방법은 간단합니다. Manifest 파일은 JAR 파일의 메타데이터를 포함하며, JAR 파일의 구조와 설정에 대한 중요한 정보를 담고 있습니다. 다음 단계를 따라 하시면 됩니다:

1. **명령 프롬프트 또는 터미널 열기**: 컴퓨터에서 명령 프롬프트(Windows) 또는 터미널(Linux, MacOS)을 엽니다.

2. **JAR 파일이 있는 디렉토리로 이동**: `cd` 명령어를 사용하여 JAR 파일이 있는 디렉토리로 이동합니다.

3. **Manifest 파일 보기**: 다음 명령어를 사용하여 JAR 파일 내의 Manifest 파일을 볼 수 있습니다:

   ```
   jar xf your-jar-file.jar META-INF/MANIFEST.MF
   ```

   여기서 `your-jar-file.jar`는 조회하고자 하는 JAR 파일의 이름으로 대체해야 합니다.

4. **Manifest 파일 내용 확인**: 위의 명령어를 실행한 후, `META-INF/MANIFEST.MF` 파일이 해당 디렉토리에 추출됩니다. 이 파일을 텍스트 에디터로 열어 내용을 확인할 수 있습니다.

이 방법은 JAR 파일 내의 Manifest

