---
layout: post
title: "다운로드 OpenJDK 8 Ubuntu"
date: 2023-12-01
tags: [Ubuntu, OpenJDK, 다운로드]
categories: blog
---

## 사용자 입력:
> 우분투에서 오라클 open-jdk8을 wget으로 내려받을 수 있는 방법을 알려줘. 

## ChatGPT 응답:
> # I will use Python to format a wget command for downloading OpenJDK 8 on Ubuntu.
jdk_version = "8"
openjdk_package = f"openjdk-{jdk_version}-jdk"

# Format the wget command
wget_command = f"sudo apt-get install {openjdk_package}"

wget_command


## ChatGPT 응답:
> Ubuntu에서 OpenJDK 8을 설치하려면 다음 명령어를 사용하시면 됩니다:

```bash
sudo apt-get install openjdk-8-jdk
```

이 명령어는 `apt-get` 패키지 관리자를 사용하여 OpenJDK 8을 설치합니다. `wget`을 직접 사용하여 JDK를 다운로드하는 것은 일반적이지 않으며, Ubuntu의 패키지 관리 시스템을 사용하는 것이 더 쉽고 안전합니다.

## 사용자 입력:
> 별도의 시스템을 만들려고 하는거니 알려줘.

## ChatGPT 응답:
> OpenJDK 8을 Ubuntu에 설치하려면 일반적으로 `apt-get` 패키지 관리자를 사용하지만, `wget`을 사용하여 직접 다운로드하고 싶으시다면, 다음과 같은 절차를 따라야 합니다:

1. **Oracle JDK 대신 AdoptOpenJDK 사용하기**: Oracle JDK는 라이센스 제한으로 인해 `wget`을 사용한 직접 다운로드가 복잡할 수 있습니다. 대신, AdoptOpenJDK와 같은 오픈 소스 버전을 사용하는 것이 좋습니다.

2. **AdoptOpenJDK 다운로드 링크 찾기**: AdoptOpenJDK 또는 다른 OpenJDK 제공업체의 웹사이트에서 OpenJDK 8의 바이너리 링크를 찾습니다.

3. **wget을 사용하여 JDK 다운로드**: 찾은 링크를 사용하여 `wget` 명령어로 JDK를 다운로드합니다.

4. **다운로드한 파일 설치**: 다운로드한 JDK 파일을 압축 해제하고 시스템에 설치합니다.

이러한 절차는 일반적으로 고급 사용자나 특정 시스템 요구 사항을 가진 경우에 사용됩니다. 이 방법을 사용하려면 정확한 JDK 다운로드 링크를 찾는 것이 중요합니다. AdoptOpenJDK는 GitHub 릴리스 페이지나 공식 웹사이트에서 다운로드 링크를 제공합니다.

## 사용자 입력:
> tar.gz 압축 푸는 법 알려줘. 우분투에서.

## ChatGPT 응답:
> Ubuntu에서 `.tar.gz` 파일을 압축 해제하는 방법은 다음과 같습니다:

1. **터미널 열기**: 먼저, Ubuntu 시스템에서 터미널을 엽니다.

2. **압축 해제할 폴더로 이동**: `cd` 명령어를 사용하여 `.tar.gz` 파일이 있는 폴더로 이동합니다. 예를 들어, 파일이 `Downloads` 폴더에 있다면 다음과 같이 입력합니다:
   ```bash
   cd ~/Downloads
   ```

3. **압축 해제하기**: `tar` 명령어를 사용하여 `.tar.gz` 파일을 압축 해제합니다. 예를 들어, 파일 이름이 `file.tar.gz`라면 다음 명령어를 사용합니다:
   ```bash
   tar -xzvf file.tar.gz
   ```

    - `x`는 "extract" (압축 해제)을 의미합니다.
    - `z`는 `gzip`을 통한 압축 해제를 나타냅니다.
    - `v`는 "verbose" (상세한 출력) 모드를 활성화합니다.
    - `f`는 "file" (파일 이름을 지정)을 의미합니다.

4. **압축 해제 확인**: 압축 해제 후, `ls` 명령어를 사용하여 파일들이 제대로 압축 해제되었는지 확인할 수 있습니다.

이 명령어들을 사용하여 Ubuntu에서 `.tar.gz` 파일을 쉽게 압축 해제할 수 있습니다.

## 사용자 입력:
> scp 포트는 22인가?

## ChatGPT 응답:
> 네, `scp` (Secure Copy Protocol)는 기본적으로 SSH 프로토콜을 사용하며, SSH의 기본 포트는 22번입니다. 따라서 `scp`를 사용할 때도 기본적으로 22번 포트를 사용합니다. 

만약 SSH 서비스가 다른 포트를 사용하도록 구성되어 있다면, `scp` 명령어에서도 해당 포트를 명시적으로 지정해야 합니다. 예를 들어, SSH 서비스가 2222번 포트를 사용한다면 `scp` 명령어는 다음과 같이 사용됩니다:

```bash
scp -P 2222 파일명 사용자명@서버주소:/경로
```

여기서 `-P` 옵션은 `scp` 명령어에 사용되는 포트를 지정합니다.

## 사용자 입력:
> 개발 서버 A IP주소에서 현재 내가 쓰고 있는 윈도우의 WSL로 scp 접속하려면, 내가 쓰고 있는 윈도우에서 무엇을 해줘야 scp 접속이 가능할 까?

## ChatGPT 응답:
> 윈도우의 WSL (Windows Subsystem for Linux)로 개발 서버 A에서 파일을 `scp`를 통해 복사하려면, 몇 가지 단계를 따라야 합니다:

1. **WSL의 IP 주소 확인**: 먼저 WSL에서 자신의 IP 주소를 확인해야 합니다. WSL 터미널을 열고 `ip addr` 명령어를 실행하여 IP 주소를 찾습니다.

2. **방화벽 설정 확인/변경**: 윈도우 방화벽 설정에서 SSH (포트 22)에 대한 인바운드 연결이 허용되어 있는지 확인합니다. 방화벽 설정에서 SSH 연결이 차단되어 있다면, 해당 연결을 허용해야 합니다.

3. **SSH 서버 구성 (필요한 경우)**: WSL에서 SSH 서버가 실행 중인지 확인하고, 실행되고 있지 않다면 설정해야 합니다. 이를 위해 WSL 터미널에서 `sudo service ssh start` 명령어를 사용하여 SSH 서버를 시작할 수 있습니다.

4. **SCP 명령어 사용**: 이제 개발 서버 A에서 WSL로 파일을 복사할 수 있습니다. 개발 서버의 터미널에서 다음과 같은 형식의 `scp` 명령어를 사용합니다:

   ```
   scp 파일명 사용자명@WSL_IP_주소:/경로
   ```

   여기서 `파일명`은 복사할 파일의 이름, `사용자명`은 WSL의 사용자 이름, `WSL_IP_주소`는 WSL의 IP 주소, `/경로`는 파일을 복사할 WSL 내의 경로입니다.

위 단계를 따르면 개발 서버에서 WSL로 파일을 `scp`를 통해 복사할 수 있습니다. SCP 접속에 문제가 발생하면 네트워크 연결, 방화벽 설정, SSH 서버 상태 등을 다시 확인해 보세요.

