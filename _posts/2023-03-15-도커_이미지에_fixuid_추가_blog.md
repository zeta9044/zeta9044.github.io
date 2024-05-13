---
layout: post
title: "도커 이미지에 fixuid 추가"
tags: [추가, 이미지, fixuid, 도]
---

## 사용자 입력:
> docker bitami/jenkins 이미지에 fixuid기능을 추가하는 법 알려줘

## ChatGPT 응답:
> Docker 이미지에 {% raw %}`fixuid`{% endraw %} 기능을 추가하려면 다음과 같은 단계를 따르면 됩니다.

1. {% raw %}`Dockerfile`{% endraw %}을 열고 {% raw %}`bitnami/jenkins`{% endraw %} 이미지를 기반으로하는 새 이미지를 만듭니다.

{% raw %}```
FROM bitnami/jenkins
```{% endraw %}

2. {% raw %}`fixuid`{% endraw %}를 설치합니다. {% raw %}`fixuid`{% endraw %}는 Linux 계정과 그룹 ID를 조정하여 호스트와 컨테이너 간의 파일 권한 문제를 해결합니다.

{% raw %}```
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
```{% endraw %}

위의 명령은 {% raw %}`curl`{% endraw %}을 사용하여 {% raw %}`fixuid`{% endraw %}를 다운로드하고 설치합니다. {% raw %}`USER`{% endraw %} 및 {% raw %}`GROUP`{% endraw %} 환경 변수는 이미지 내에 사용할 Jenkins 사용자 및 그룹을 지정합니다. 마지막으로, {% raw %}`/etc/fixuid/config.yml`{% endraw %} 파일에 사용자와 그룹을 구성합니다.

3. {% raw %}`entrypoint.sh`{% endraw %} 파일에 {% raw %}`fixuid`{% endraw %}를 실행하는 명령을 추가합니다. {% raw %}`entrypoint.sh`{% endraw %} 파일은 이미지가 시작될 때 실행되는 스크립트입니다.

{% raw %}```
RUN \
  sed -i 's/set -e/set -e\n\n# fix permissions with fixuid\nif [[ "$$*" == *"/run.sh"* ]]; then\n  exec fixuid -q\nfi\n/' /app-entrypoint.sh
```{% endraw %}

위의 명령은 {% raw %}`entrypoint.sh`{% endraw %} 파일에서 {% raw %}`/run.sh`{% endraw %} 스크립트를 실행하기 전에 {% raw %}`fixuid`{% endraw %}를 실행하도록 수정합니다.

4. 이미지를 빌드합니다.

{% raw %}```
docker build -t my-jenkins .
```{% endraw %}

위의 명령은 현재 디렉토리에서 {% raw %}`Dockerfile`{% endraw %}을 사용하여 이미지를 빌드하고 {% raw %}`my-jenkins`{% endraw %}라는 태그를 지정합니다.

5. 컨테이너를 실행합니다.

{% raw %}```
docker run --rm -it -p 8080:8080 my-jenkins
```{% endraw %}

위의 명령은 {% raw %}`my-jenkins`{% endraw %} 이미지에서 새 컨테이너를 시작하고 호스트의 8080 포트와 컨테이너의 8080 포트를 연결합니다.

