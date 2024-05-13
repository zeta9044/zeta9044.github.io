---
layout: post
title: "Node.js 설치 스크립트 (Node.js Installation Script)"
tags: [js, 설치, Script, Node, Installation, 스크립트]
---

## 사용자 입력:
> nodejs 14.21.3을 온라인상으로 설치할 bash 스크립트 알려줘

## ChatGPT 응답:
> 아래는 Node.js 14.21.3을 온라인상으로 설치하는 bash 스크립트입니다.

{% raw %}```bash
#!/bin/bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```{% endraw %}

위 스크립트는 다음과 같은 작업을 수행합니다.

1. {% raw %}`curl`{% endraw %} 명령어를 사용하여 Node.js 14.x 버전을 설치하기 위한 저장소 설정 파일을 다운로드합니다.
2. {% raw %}`sudo`{% endraw %} 권한으로 {% raw %}`bash`{% endraw %}를 실행하여 저장소 설정 파일을 실행합니다.
3. {% raw %}`apt-get`{% endraw %} 명령어를 사용하여 Node.js를 설치합니다.

위 스크립트를 실행하면 Node.js 14.21.3 버전이 설치됩니다.

## 사용자 입력:
> liunx 버전 아는 법

## ChatGPT 응답:
> 리눅스(Linux)의 버전을 확인하는 방법은 여러 가지가 있습니다. 

1. uname 명령어를 사용하는 방법

{% raw %}`uname`{% endraw %} 명령어는 현재 운영체제의 정보를 출력하는 명령어입니다. {% raw %}`uname -a`{% endraw %} 명령어를 실행하면 리눅스 버전 정보를 확인할 수 있습니다.

{% raw %}```
$ uname -a
Linux example.com 4.15.0-91-generic #92-Ubuntu SMP Fri Feb 28 11:09:48 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```{% endraw %}

위 출력 결과에서 "4.15.0-91-generic"은 리눅스 버전 정보입니다.

2. cat /etc/*-release 명령어를 사용하는 방법

{% raw %}`/etc/*-release`{% endraw %} 파일은 리눅스 배포판 정보를 포함하고 있습니다. {% raw %}`cat /etc/*-release`{% endraw %} 명령어를 실행하면 리눅스 버전 정보와 함께 리눅스 배포판 정보도 확인할 수 있습니다.

{% raw %}```
$ cat /etc/*-release
NAME="Ubuntu"
VERSION="18.04.5 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.5 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```{% endraw %}

위 출력 결과에서 "VERSION" 또는 "VERSION_ID" 항목에 있는 정보가 리눅스 버전 정보입니다.

3. lsb_release 명령어를 사용하는 방법

{% raw %}`lsb_release`{% endraw %} 명령어를 실행하면 리눅스 배포판 정보를 확인할 수 있습니다. {% raw %}`lsb_release -a`{% endraw %} 명령어를 실행하면 리눅스 버전 정보와 함께 리눅스 배포판 정보도 확인할 수 있습니다.

{% raw %}```
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.5 LTS
Release:	18.04
Codename:	bionic
```{% endraw %}

위 출력 결과에서 "Release" 항목에 있는 정보가 리눅스 버전 정보입니다.

## 사용자 입력:
> nodejs 14.21.3을 온라인상으로 설치할 bash 스크립트 알려줘.os는 centos7

## ChatGPT 응답:
> Node.js 14.21.3을 CentOS 7에 설치하기 위한 Bash 스크립트는 다음과 같습니다.

{% raw %}```bash
#!/bin/bash

# Update the package manager and install necessary dependencies
sudo yum update -y
sudo yum install -y gcc-c++ make

# Download the Node.js source code
wget https://nodejs.org/dist/v14.21.3/node-v14.21.3.tar.gz

# Extract the source code and navigate to the directory
tar -zxvf node-v14.21.3.tar.gz
cd node-v14.21.3

# Configure, build and install Node.js
./configure
make
sudo make install

# Verify that Node.js and npm are installed properly
node -v
npm -v
```{% endraw %}

이 스크립트는 먼저 CentOS 패키지 관리자를 업데이트하고 필요한 종속성을 설치합니다. 그런 다음 Node.js 소스 코드를 다운로드하고 압축을 해제합니다. 그 후, 소스 코드 디렉토리로 이동하여 Node.js를 구성하고 빌드하고 설치합니다. 마지막으로, Node.js와 npm이 올바르게 설치되었는지 확인하기 위해 각각의 버전을 확인합니다.

이 스크립트는 root 권한이 필요하므로 {% raw %}`sudo`{% endraw %}를 사용하여 실행해야합니다. 또한, 다운로드 및 설치에 몇 분 정도 소요될 수 있습니다.

## 사용자 입력:
> nodejs 14.21.3을 온라인상으로 설치할 bash 스크립트 알려줘.os는 centos7. binary로 설치할 거야. 홈디렉토리에 설치해서 실행할 거야.

## ChatGPT 응답:
> 다음은 CentOS 7에서 Node.js 14.21.3을 홈 디렉토리에 바이너리로 설치하는 Bash 스크립트입니다.

{% raw %}```bash
#!/bin/bash

# Node.js version to install
NODE_VERSION="14.21.3"

# Download URL for the Node.js binary
NODE_URL="https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz"

# Create a directory for Node.js installation
mkdir -p ~/nodejs

# Download and extract Node.js binary
curl -sL ${NODE_URL} | tar -xJ --strip-components=1 -C ~/nodejs

# Add Node.js bin directory to PATH
echo 'export PATH="$HOME/nodejs/bin:$PATH"' >> ~/.bashrc

# Refresh the current shell session
source ~/.bashrc

# Verify installation
node -v
```{% endraw %}

이 스크립트는 다음과 같은 작업을 수행합니다.

1. {% raw %}`NODE_VERSION`{% endraw %} 변수를 설정하여 설치할 Node.js 버전을 지정합니다.
2. {% raw %}`NODE_URL`{% endraw %} 변수를 설정하여 다운로드할 Node.js 이진 파일의 URL을 지정합니다.
3. {% raw %}`~/nodejs`{% endraw %} 디렉토리를 생성하여 Node.js를 설치할 위치를 지정합니다.
4. {% raw %}`curl`{% endraw %} 명령을 사용하여 {% raw %}`NODE_URL`{% endraw %}에서 Node.js 이진 파일을 다운로드하고 {% raw %}`tar`{% endraw %} 명령을 사용하여 {% raw %}`~/nodejs`{% endraw %} 디렉토리에 추출합니다.
5. {% raw %}`.bashrc`{% endraw %} 파일에 Node.js 바이너리 경로를 추가합니다.
6. 현재 셸 세션을 새로 고칩니다.
7. {% raw %}`node -v`{% endraw %} 명령을 사용하여 Node.js 설치가 올바르게 작동하는지 확인합니다.

이 스크립트를 실행하기 전에 스크립트를 실행할 수 있는 권한을 부여해야 합니다. 다음 명령어를 사용하여 스크립트를 실행 가능한 권한으로 설정할 수 있습니다.

{% raw %}```bash
chmod +x install_nodejs.sh
```{% endraw %}

그리고 스크립트를 실행하려면 다음 명령을 실행하십시오.

{% raw %}```bash
./install_nodejs.sh
```{% endraw %}

## 사용자 입력:
> bamboo는 무조건 source check부터 해야 하나?

## ChatGPT 응답:
> Bamboo는 Continuous Integration(CI) 및 Continuous Deployment(CD) 도구 중 하나입니다. Bamboo에서는 빌드, 테스트 및 배포 프로세스를 자동화하여 효율성을 높이고 품질을 향상시킬 수 있습니다. 

Bamboo에서 소스 체크는 CI/CD 프로세스의 시작점이 됩니다. 일반적으로 Bamboo에서 소스 체크는 빌드를 수행하기 전에 코드 레파지토리에서 최신 코드를 가져와서 검사하고 빌드하기 위해 설정합니다. 이를 통해 소스 코드가 빌드 및 배포되기 전에 자동으로 품질 검사를 수행할 수 있습니다.

하지만 Bamboo에서 소스 체크를 무조건 해야 하는 것은 아닙니다. Bamboo에서는 사용자가 원하는 대로 CI/CD 프로세스를 설정할 수 있습니다. 예를 들어, Bamboo에서 빌드 및 테스트를 수행하기 전에 수동으로 소스 코드를 업로드하거나, 다른 CI/CD 도구를 사용하여 소스 체크를 수행할 수도 있습니다.

따라서 Bamboo에서 소스 체크를 해야 하는지 여부는 사용자가 프로젝트의 요구 사항에 따라 결정해야 합니다. 일반적으로 Bamboo에서 소스 체크를 수행하는 것이 좋지만, 특별한 경우에는 다른 방식을 선택할 수 있습니다.

## 사용자 입력:
> 
20-3월-2023 13:12:18	Build Q-Track WAS - release - release-4.0.0 빌드 #7 (QTRAC-REL4-JOB1-7) started building on agent Base Agent
20-3월-2023 13:12:18	
20-3월-2023 13:12:18	Running on server: post build plugin 'NCover Results Collector'
20-3월-2023 13:12:18	Running on server: post build plugin 'Build Hanging Detection Configuration'
20-3월-2023 13:12:18	Running on server: post build plugin 'Clover Delta Calculator'
20-3월-2023 13:12:18	Running on server: post build plugin 'Maven Dependencies Postprocessor'
20-3월-2023 13:12:18	All post build plugins have finished
20-3월-2023 13:12:18	Generating build results summary...
20-3월-2023 13:12:18	Saving build results to disk...
20-3월-2023 13:12:18	Logging substituted variables...
20-3월-2023 13:12:18	Indexing build results...
20-3월-2023 13:12:18	Finished building QTRAC-REL4-JOB1-7.

## ChatGPT 응답:
> 이 로그는 Bamboo에서 Q-Track WAS release 버전 4.0.0을 빌드하는 중인 것으로 보입니다. 빌드가 Base Agent 에이전트에서 실행되고 있으며, 빌드 중에 다양한 후 처리 작업이 수행되고 있습니다. 빌드가 성공적으로 완료되었으며, 빌드 결과가 디스크에 저장되었고, 결과 요약이 생성되었으며, 빌드 결과가 색인되었습니다. 빌드 과정에서 어떤 문제가 발생하지 않았으며, 빌드가 성공적으로 완료되었다는 것을 나타내고 있습니다.

## 사용자 입력:
> intellj console에서 webapp를 실행했을 때, 한글이 깨지는 현상해결방법?

## ChatGPT 응답:
> IntelliJ에서 웹 애플리케이션을 실행할 때, 기본적으로 IntelliJ 내장 톰캣 서버를 사용하며, 서버가 실행되면 IntelliJ 콘솔 창에 출력됩니다. IntelliJ 콘솔 창은 기본적으로 UTF-8 인코딩을 사용하므로, 한글이 깨질 경우에는 대부분 서버에서 사용하는 인코딩이 UTF-8이 아닌 경우입니다.

따라서 IntelliJ에서 한글이 깨지는 현상을 해결하려면, 다음 두 가지 방법 중 하나를 시도할 수 있습니다.

1. 서버의 인코딩 설정 변경

웹 애플리케이션이 사용하는 서버의 인코딩 설정을 UTF-8로 변경하면, IntelliJ에서 한글이 제대로 표시됩니다. 대부분의 서버에서는 설정 파일 또는 환경 변수를 통해 인코딩을 설정할 수 있습니다. 서버가 실행되기 전에 인코딩 설정을 확인하고 변경하십시오.

2. IntelliJ 콘솔 인코딩 설정 변경

IntelliJ 콘솔 창에서 사용되는 인코딩을 UTF-8로 변경하면, 한글이 깨지지 않습니다. IntelliJ 설정에서 콘솔 인코딩을 변경할 수 있습니다.

- IntelliJ 메뉴에서 "Help" > "Edit Custom Properties..."를 선택합니다.
- "idea.properties" 파일이 열리면, 다음 라인을 추가합니다.
  {% raw %}```
  idea.jvm.args=-Dfile.encoding=UTF-8
  ```{% endraw %}
- IntelliJ을 재시작합니다.

위 두 가지 방법 중 하나를 시도해보시기 바랍니다.

## 사용자 입력:
> 계속 깨지는데...com.datastreams.qtrack.exception.exception.QtrackException : ������ �׸� ���Ͽ� ���������� �����ϹǷ� ������ �� �����ϴ�


## ChatGPT 응답:
> 이 예외 메시지는 한글이 깨진 상태로 출력되고 있습니다. 이 문제를 해결하려면, IntelliJ에서 실행 중인 애플리케이션의 인코딩 설정을 확인하고 변경해야 합니다.

1. IntelliJ에서 "Run" 메뉴를 선택하고, "Edit Configurations"을 선택합니다.
2. 실행 중인 애플리케이션의 구성을 선택하고, "Configuration" 탭에서 "VM options"란에 다음과 같이 인코딩 설정을 추가합니다.

   {% raw %}```
   -Dfile.encoding=UTF-8
   ```{% endraw %}
   
3. "OK" 버튼을 클릭하여 설정을 저장합니다.
4. IntelliJ에서 애플리케이션을 다시 실행하고, 한글이 깨지는지 확인합니다.

이 방법으로도 한글이 깨지는 경우가 있다면, 애플리케이션에서 출력하는 데이터의 인코딩 설정도 확인해보시기 바랍니다.

## 사용자 입력:
> 여전히 깨지는 데...

## ChatGPT 응답:
> 애플리케이션에서 출력하는 한글이 깨지는 경우에는 애플리케이션에서 사용하는 인코딩 설정을 확인해야 합니다.

만약 애플리케이션에서 UTF-8 인코딩을 사용하고 있다면, 다음과 같은 설정을 추가하여 애플리케이션의 인코딩 설정을 변경할 수 있습니다.

1. 애플리케이션에서 사용하는 WAS 서버의 설정 파일에 다음과 같은 JVM 옵션을 추가합니다.

    {% raw %}```
    -Dfile.encoding=UTF-8
    ```{% endraw %}

    대부분의 WAS에서는 Java 옵션을 설정할 수 있는 방법을 제공합니다. 이 방법은 WAS마다 다를 수 있습니다. WAS에서 Java 옵션을 설정하는 방법을 찾아서 해당 JVM 옵션을 추가해주시면 됩니다.

2. 만약 애플리케이션이 스프링 프레임워크를 사용하고 있다면, {% raw %}`application.properties`{% endraw %} 또는 {% raw %}`application.yml`{% endraw %} 파일에 다음과 같은 설정을 추가합니다.

    {% raw %}```
    spring.http.encoding.charset=UTF-8
    spring.http.encoding.enabled=true
    spring.http.encoding.force=true
    ```{% endraw %}

위 방법을 시도해도 문제가 해결되지 않는 경우, 애플리케이션이 출력하는 데이터가 올바른 인코딩으로 저장되어 있는지 다시 확인해보시기 바랍니다.

