---
layout: post
title: "Docker에서 fixuid 추가하기"
date: 2023-03-15
tags: [추가, fixuid]
categories: blog
---

## 사용자 입력:
> docker bitami/jenkins 이미지에 fixuid기능을 추가하는 법 알려줘

## ChatGPT 응답:
> Docker를 사용해 Bitnami Jenkins 이미지에 fixuid 기능을 추가하려면 다음 단계를 수행하세요. 이 단계는 Dockerfile을 사용하여 새 이미지를 빌드합니다. fixuid는 컨테이너에서 호스트 시스템과 동일한 사용자 ID 및 그룹 ID를 사용하도록 Jenkins를 구성하는 데 도움이 됩니다.

1. 먼저 작업 디렉토리를 만들고 이동하세요.
```
mkdir my-custom-jenkins
cd my-custom-jenkins
```

2. 다음 내용을 포함하는 Dockerfile을 작성하세요.
```
FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# Install fixuid
RUN curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid

# Configure fixuid
RUN echo "user: jenkins\ngroup: jenkins\n" > /etc/fixuid/config.yml

# Switch back to Jenkins user
USER 1001

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "entrypoint.sh"]
```

3. Dockerfile이 있는 디렉토리에서 이미지를 빌드하세요.
```
docker build -t my-custom-jenkins:latest .
```

4. 새로 생성된 이미지를 실행하세요.
```
docker run --rm -p 8080:8080 -v /path/to/your/jenkins_home:/bitnami/jenkins my-custom-jenkins:latest
```

이제 fixuid 기능이 추가된 사용자 정의 Bitnami Jenkins 이미지를 사용할 수 있습니다. fixuid는 컨테이너를 시작할 때 사용자 ID와 그룹 ID를 적절하게 매핑합니다.

## 사용자 입력:
> 위에 내용 실행하면, fixuid: yaml: mapping values are not allowed in this context 이런 에러 나는데

## ChatGPT 응답:
> 죄송합니다. `/etc/fixuid/config.yml` 파일에 문제가 있었습니다. 다음 수정된 Dockerfile을 사용하세요.

```
FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# Install fixuid
RUN curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid

# Configure fixuid
RUN printf "user: jenkins\ngroup: jenkins\n" > /etc/fixuid/config.yml

# Switch back to Jenkins user
USER 1001

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "entrypoint.sh"]
```

위의 Dockerfile에서 수정된 부분은 다음과 같습니다.

`RUN echo "user: jenkins\ngroup: jenkins\n" > /etc/fixuid/config.yml`

을

`RUN printf "user: jenkins\ngroup: jenkins\n" > /etc/fixuid/config.yml`

로 변경했습니다.

이제 Dockerfile을 다시 빌드하고 실행하면 fixuid 관련 에러가 발생하지 않을 것입니다.

## 사용자 입력:
> sudo docker run  -u lia:lia --rm -p 9000:8080 -v /home/lia/jenkins_home:/bitnami/jenkins jenkins/fixuid:1.0.0 이 명령을 실행하면, docker: Error response from daemon: unable to find user --rm: no matching entries in passwd file. 이런 에러가 나오네

## ChatGPT 응답:
> `-u` 옵션을 사용할 때, `username:groupname` 형식을 사용할 수 없습니다. 대신 `uid:gid` 형식을 사용해야 합니다. 먼저 호스트 시스템에서 사용자 ID(`uid`)와 그룹 ID(`gid`)를 찾아야 합니다. 터미널에서 다음 명령을 실행하여 해당 정보를 찾으세요.

```bash
id -u lia
id -g lia
```

예를 들어, 위의 명령의 결과가 다음과 같다고 가정하겠습니다.

```
uid=1000(lia) gid=1000(lia)
```

이제 다음과 같이 수정된 `docker run` 명령을 사용하세요.

```bash
sudo docker run -u 1000:1000 --rm -p 9000:8080 -v /home/lia/jenkins_home:/bitnami/jenkins jenkins/fixuid:1.0.0
```

위 명령에서 `1000:1000`은 실제로 찾은 사용자 ID와 그룹 ID로 대체해야 합니다. 이렇게 하면 이전에 발생한 에러가 해결됩니다.

## 사용자 입력:
> FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# debian / ubuntu
RUN addgroup --gid 1000 docker && \
    adduser --uid 1000 --ingroup docker --home /home/docker --shell /bin/sh --disabled-password --gecos "" docker    

# Install fixuid
RUN USER=docker && \
    GROUP=docker && \
    curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid && \
    printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml

# Switch back to Jenkins user
USER docker:docker

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "/opt/bitnami/scripts/jenkins/entrypoint.sh"]
CMD [ "/opt/bitnami/scripts/jenkins/run.sh" ] 이 dockerfile로 실행하면, docker image실행할 때 다음과 같은 오류가 발생하네  mkdir: cannot create directory '/bitnami/jenkins/home': Permission denied

## ChatGPT 응답:
> 이 문제는 `/bitnami/jenkins/home` 디렉토리에 대한 권한 문제로 보입니다. Dockerfile에 있는 `USER docker:docker`가 컨테이너 내에서 실행되는 사용자를 변경하지만, 이전에 생성된 `/bitnami/jenkins` 디렉토리에 대한 권한이 없습니다.

Dockerfile을 수정하여 `/bitnami/jenkins` 디렉토리에 대한 권한을 부여하세요.

```
FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# debian / ubuntu
RUN addgroup --gid 1000 docker && \
    adduser --uid 1000 --ingroup docker --home /home/docker --shell /bin/sh --disabled-password --gecos "" docker    

# Install fixuid
RUN USER=docker && \
    GROUP=docker && \
    curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid && \
    printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml

# Change ownership of /bitnami/jenkins directory
RUN chown -R docker:docker /bitnami/jenkins

# Switch back to Jenkins user
USER docker:docker

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "/opt/bitnami/scripts/jenkins/entrypoint.sh"]
CMD [ "/opt/bitnami/scripts/jenkins/run.sh" ]
```

위 Dockerfile에서 추가된 부분은 다음과 같습니다.

```
# Change ownership of /bitnami/jenkins directory
RUN chown -R docker:docker /bitnami/jenkins
```

이제 다시 이미지를 빌드하고 실행하면 권한 문제가 해결되어야 합니다.

## 사용자 입력:
> 위와 같이 했는데도, mkdir: cannot create directory '/bitnami/jenkins/home': Permission denied 이런 메세지가 docker 실행할 때 발생한다.

## ChatGPT 응답:
> 죄송합니다. 이 문제는 Jenkins 이미지가 실행되는 동안 사용되는 볼륨 마운트에 대한 권한 문제일 수 있습니다. Dockerfile에서 컨테이너의 `/bitnami/jenkins` 디렉토리에 대한 권한을 변경하면 실행 중인 컨테이너 내에서만 적용됩니다. 실제로 호스트 시스템에 마운트된 볼륨의 권한이 여전히 문제가 될 수 있습니다.

이 문제를 해결하기 위해 컨테이너를 시작하기 전에 호스트 시스템에서 볼륨에 대한 권한을 변경해야 합니다. 다음 명령을 사용하여 호스트 시스템의 Jenkins 홈 디렉토리에 대한 권한을 변경하세요.

```bash
sudo chown -R 1000:1000 /home/lia/jenkins_home
```

위 명령에서 `/home/lia/jenkins_home`은 실제로 사용 중인 Jenkins 홈 디렉토리로 변경해야 합니다. 변경된 권한이 있으면 컨테이너를 다시 시작해 보세요. 이제 권한 문제가 해결되어야 합니다.

참고로, Dockerfile에서 이미지를 생성하는 동안 `/bitnami/jenkins` 디렉토리의 권한을 변경하는 것은 더 이상 필요하지 않습니다. 따라서 해당 부분을 제거해도 됩니다.

## 사용자 입력:
> fixuid는 볼륨 마운트된 소유자 권한도 host id기준으로 실행하는 것으로 아는데, 네가 잘못된 정보를 제공한 것은 아닐까?

## ChatGPT 응답:
> 제가 제공한 정보가 부정확했습니다. 죄송합니다. fixuid를 사용하여 마운트된 볼륨의 소유자 권한도 호스트 ID 기준으로 실행하도록 다음과 같이 Dockerfile을 수정하겠습니다.

```Dockerfile
FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# Install fixuid
RUN curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid

# Configure fixuid
RUN printf "user: jenkins\ngroup: jenkins\npaths:\n  - /bitnami/jenkins\n" > /etc/fixuid/config.yml

# Switch back to Jenkins user
USER 1001

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "/opt/bitnami/scripts/jenkins/entrypoint.sh"]
CMD [ "/opt/bitnami/scripts/jenkins/run.sh" ]
```

Dockerfile의 변경 사항은 다음과 같습니다.

1. 사용자 및 그룹 생성 부분을 제거했습니다.
2. fixuid 설정 파일(`/etc/fixuid/config.yml`)에 `/bitnami/jenkins` 경로를 추가했습니다.

이렇게 하면 fixuid가 `/bitnami/jenkins` 경로의 소유자 및 그룹을 호스트와 일치하게 설정합니다. 이제 이미지를 다시 빌드하고 실행하면 권한 문제가 해결되어야 합니다.

## 사용자 입력:
> 
 => ERROR [ 2/11] COPY prebuildfs /                                                                                                                                     0.0s
 => CACHED [ 3/11] RUN install_packages ca-certificates curl fontconfig git jq libfontconfig1 openssh-client procps unzip zlib1g                                        0.0s
 => CACHED [ 4/11] RUN mkdir -p /tmp/bitnami/pkg/cache/ && cd /tmp/bitnami/pkg/cache/ &&     COMPONENTS=(       "render-template-1.0.5-1-linux-${OS_ARCH}-debian-11"    0.0s
 => CACHED [ 5/11] RUN apt-get update && apt-get upgrade -y &&     apt-get clean && rm -rf /var/lib/apt/lists /var/cache/apt/archives                                   0.0s
 => CACHED [ 6/11] RUN chmod g+rwX /opt/bitnami                                                                                                                         0.0s
 => CACHED [ 7/11] RUN addgroup --gid 1000 docker &&     adduser --uid 1000 --ingroup docker --home /home/docker --shell /bin/sh --disabled-password --gecos "" docker  0.0s
 => CACHED [ 8/11] RUN USER=docker &&     GROUP=docker &&     curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | t  0.0s
 => ERROR [ 9/11] COPY rootfs /                                                                                                                                         0.0s
------
 > [ 2/11] COPY prebuildfs /:
------
------
 > [ 9/11] COPY rootfs /:
------
Dockerfile:59
--------------------
  57 |     USER docker:docker
  58 |
  59 | >>> COPY rootfs /
  60 |     RUN /opt/bitnami/scripts/java/postunpack.sh
  61 |     RUN /opt/bitnami/scripts/jenkins/postunpack.sh
--------------------
ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref moby::tv711drt24epaq98032rp9yml: "/rootfs": not found


## ChatGPT 응답:
> 위의 오류는 Dockerfile의 `COPY rootfs /` 명령에 대한 문제로 보입니다. 이 명령은 현재 디렉토리의 `rootfs` 폴더를 이미지의 루트(`/`)에 복사하려고 합니다. 하지만 `rootfs` 폴더가 존재하지 않아 오류가 발생합니다.

이전에 제공한 Dockerfile에서는 `COPY rootfs /` 명령을 사용하지 않았습니다. 이 문제를 해결하려면 해당 명령을 제거하거나 필요한 `rootfs` 디렉토리를 작성해야 합니다.

제공한 이전 Dockerfile을 참고하여 수정된 Dockerfile을 사용하세요.

```Dockerfile
FROM bitnami/jenkins:latest

# Install necessary packages
USER 0
RUN apt-get update && \
    apt-get install -y curl sudo

# Install fixuid
RUN curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid

# Configure fixuid
RUN printf "user: jenkins\ngroup: jenkins\npaths:\n  - /bitnami/jenkins\n" > /etc/fixuid/config.yml

# Switch back to Jenkins user
USER 1001

# Set entrypoint to use fixuid
ENTRYPOINT ["fixuid", "-q", "/opt/bitnami/scripts/jenkins/entrypoint.sh"]
CMD [ "/opt/bitnami/scripts/jenkins/run.sh" ]
```

이 Dockerfile을 사용하여 이미지를 다시 빌드하고 실행하면 권한 문제가 해결되어야 합니다.

## 사용자 입력:
> addgroup adduser가 들어 있는 패키지 apt-get으로 install하는 방법

## ChatGPT 응답:
> Debian 기반 시스템에서 `addgroup` 및 `adduser` 명령은 기본적으로 사용 가능한 `adduser` 패키지에 포함되어 있습니다. 일반적으로 이 패키지는 이미 시스템에 설치되어 있지만, 만약 설치되어 있지 않다면 다음 명령을 사용하여 설치할 수 있습니다.

```bash
sudo apt-get update
sudo apt-get install adduser
```

이렇게 하면 `addgroup` 및 `adduser` 명령을 사용할 수 있게 됩니다. 또한 `adduser` 패키지는 대부분의 기본 이미지에서 사용할 수 있기 때문에, Dockerfile에서 별도로 설치할 필요가 없습니다.

## 사용자 입력:
> [root@liabuild jenkins_docker]# docker run -u 1000:500 --rm -p 9000:8080 -v /home/lia/.jenkins:/var/jenkins_home jenkins/jenkins
touch: cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied
Can not write to /var/jenkins_home/copy_reference_file.log. Wrong volume permissions?


## ChatGPT 응답:
> 위의 오류 메시지는 `/var/jenkins_home` 디렉토리에 대한 권한 문제를 나타냅니다. 볼륨 마운트는 호스트 시스템의 `/home/lia/.jenkins` 디렉토리를 컨테이너의 `/var/jenkins_home` 디렉토리에 연결합니다.

이 문제를 해결하려면 호스트 시스템에서 볼륨에 대한 권한을 변경해야 합니다. 다음 명령을 사용하여 호스트 시스템의 Jenkins 홈 디렉토리에 대한 권한을 변경하세요.

```bash
sudo chown -R 1000:500 /home/lia/.jenkins
```

위 명령에서 `/home/lia/.jenkins`은 실제로 사용 중인 Jenkins 홈 디렉토리로 변경해야 합니다. 변경된 권한이 있으면 컨테이너를 다시 시작해 보세요. 이제 권한 문제가 해결되어야 합니다.

## 사용자 입력:
> systemd-journald(8)

## ChatGPT 응답:
> `systemd-journald(8)`는 systemd 시스템 및 서비스 관리자의 일부로, 리눅스 시스템에서 로그를 수집, 처리 및 저장하는 데 사용되는 journald 데몬입니다. systemd-journald는 로그를 바이너리 형식으로 저장하며, 이러한 로그는 `journalctl` 명령을 사용하여 검색하고 조회할 수 있습니다.

systemd-journald의 주요 기능은 다음과 같습니다:

1. 시스템, 커널 및 서비스 로그를 수집합니다.
2. 로그 메시지를 인덱싱하여 빠른 검색을 지원합니다.
3. 로그 메시지에 메타데이터를 추가하여 검색 및 분석을 쉽게 합니다.
4. 로그 데이터의 압축 및 저장을 관리합니다.

systemd-journald를 사용하면 기존의 로그 관리 시스템(예: syslog)과 비교하여 로그 수집 및 처리가 더 효율적이고 안전하게 이루어질 수 있습니다. 로그 파일의 경우, 외부에서 접근하거나 조작할 수 있는 위험이 있지만, systemd-journald는 로그를 바이너리 형식으로 저장하여 이러한 위험을 줄입니다. 또한, 메타데이터를 이용하여 로그 데이터를 보다 쉽게 분석하고 관리할 수 있습니다.

`journalctl` 명령을 사용하여 systemd-journald에 저장된 로그를 조회할 수 있습니다. 예를 들어, 시스템 전체 로그를 조회하려면 다음 명령을 사용합니다.

```bash
journalctl
```

특정 서비스에 대한 로그만 조회하려면 다음과 같이 사용합니다.

```bash
journalctl -u SERVICE_NAME
```

더 많은 필터링 및 검색 옵션을 사용하여 로그를 조회할 수 있습니다. `journalctl` 명령에 대한 자세한 정보는 다음 명령을 사용하여 확인할 수 있습니다.

```bash
man journalctl
```

