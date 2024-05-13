---
layout: post
title: "도커 이미지에 fixuid 추가"
date: 2023-03-15
tags: [도, fixuid, 이미지, 추가]
categories: blog
---

## 사용자 입력:
> docker bitami/jenkins 이미지에 fixuid기능을 추가하는 법 알려줘

## ChatGPT 응답:
> Docker 이미지에 `fixuid` 기능을 추가하려면 다음과 같은 단계를 따르면 됩니다.

1. `Dockerfile`을 열고 `bitnami/jenkins` 이미지를 기반으로하는 새 이미지를 만듭니다.

```
FROM bitnami/jenkins
```

2. `fixuid`를 설치합니다. `fixuid`는 Linux 계정과 그룹 ID를 조정하여 호스트와 컨테이너 간의 파일 권한 문제를 해결합니다.

```
RUN \
  apt-get update && \
  apt-get install -y curl && \
  USER=jenkins && \
  GROUP=jenkins && \
  VERSION=0.5 && \
  ARCH="$(dpkg --print-architecture)" && \
  curl -sSL https://github.com/boxboat/fixuid/releases/download/v${VERSION}/fixuid-${VERSION}-linux-${ARCH} \
    -o /usr/local/bin/fixuid && \
  chmod +x /usr/local/bin/fixuid && \
  printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml
```

위의 명령은 `curl`을 사용하여 `fixuid`를 다운로드하고 설치합니다. `USER` 및 `GROUP` 환경 변수는 이미지 내에 사용할 Jenkins 사용자 및 그룹을 지정합니다. 마지막으로, `/etc/fixuid/config.yml` 파일에 사용자와 그룹을 구성합니다.

3. `entrypoint.sh` 파일에 `fixuid`를 실행하는 명령을 추가합니다. `entrypoint.sh` 파일은 이미지가 시작될 때 실행되는 스크립트입니다.

```
RUN \
  sed -i 's/set -e/set -e\n\n# fix permissions with fixuid\nif [[ "$$*" == *"/run.sh"* ]]; then\n  exec fixuid -q\nfi\n/' /app-entrypoint.sh
```

위의 명령은 `entrypoint.sh` 파일에서 `/run.sh` 스크립트를 실행하기 전에 `fixuid`를 실행하도록 수정합니다.

4. 이미지를 빌드합니다.

```
docker build -t my-jenkins .
```

위의 명령은 현재 디렉토리에서 `Dockerfile`을 사용하여 이미지를 빌드하고 `my-jenkins`라는 태그를 지정합니다.

5. 컨테이너를 실행합니다.

```
docker run --rm -it -p 8080:8080 my-jenkins
```

위의 명령은 `my-jenkins` 이미지에서 새 컨테이너를 시작하고 호스트의 8080 포트와 컨테이너의 8080 포트를 연결합니다.

