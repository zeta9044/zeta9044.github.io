---
layout: post
title: "gradle 외부 jar 파일 지정"
date: 2023-03-24
tags: [jar, gradle, 파일, 외부, 지정]
categories: blog
---

## 사용자 입력:
> gradle에서 dependency를 설정할때, 외부 디렉토리의 파일 1개 jar를 지정하려면?

## ChatGPT 응답:
> 

Gradle에서 외부 디렉토리에 있는 jar 파일을 지정하는 방법은 다음과 같습니다.

```groovy
dependencies {
    compile files('/path/to/your.jar')
}
```

위 예제에서는 `dependencies` 블록 안에 `compile` 스코프를 사용하여 외부 jar 파일을 지정하고 있습니다. `files` 메소드를 사용하여 외부 디렉토리의 jar 파일 경로를 지정합니다. 경로는 문자열로 지정하며, 파일 시스템 경로 또는 URL 형식으로 지정할 수 있습니다. 

만약에 상대경로를 이용해서 직접적으로 파일에 접근하려면 다음과 같이 작성할 수 있습니다.

```groovy
dependencies {
    compile files('relative/path/to/your.jar')
}
```

또한, 여러 개의 외부 jar 파일을 지정하려면 `files` 메소드에 경로를 여러 개 지정하면 됩니다. 예를 들어:

```groovy
dependencies {
    compile files('/path/to/your.jar', '/path/to/your-second.jar')
}
```

이렇게 설정하면 Gradle이 컴파일 클래스 패스에 해당 jar 파일을 추가합니다.

