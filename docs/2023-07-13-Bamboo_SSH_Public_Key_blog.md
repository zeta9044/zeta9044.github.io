---
layout: post
title: "Bamboo SSH Public Key"
date: 2023-07-13
tags: [SSH, Bamboo, Key, Public]
categories: blog
---

## 사용자 입력:
> bamboo 빌드 로그에서 ssh의  public key를 확인하는 방법

## ChatGPT 응답:
> Bamboo 빌드 로그에서 SSH의 공개 키를 확인하는 방법은 일반적으로 다음과 같은 절차를 따릅니다:

1. Bamboo 빌드 로그에 접근합니다. 이 로그는 Bamboo 웹 인터페이스를 통해 액세스할 수 있습니다. 일반적으로 빌드 로그는 빌드 계획 실행 후 해당 빌드 결과 페이지에 표시됩니다.

2. 빌드 로그에서 SSH 관련 부분을 찾습니다. SSH 관련 활동은 일반적으로 "SSH" 또는 "SSH 통신"과 같은 키워드로 식별될 수 있습니다. 이 부분은 빌드 프로세스 중에 SSH 통신이 발생한 경우에만 나타날 수 있습니다.

3. SSH 관련 부분에서 공개 키 정보를 찾습니다. 일반적으로 SSH 연결 시 서버로부터의 공개 키 인증에 대한 정보가 로그에 기록됩니다. 이 부분에는 일반적으로 "public key" 또는 "authorized key"와 같은 용어가 포함됩니다.

4. 공개 키 정보를 확인합니다. 일반적으로 공개 키는 긴 문자열로 표시되며, 키의 시작 부분에는 "ssh-rsa" 또는 "ssh-dsa"와 같은 키 유형 정보가 포함될 수 있습니다.

Bamboo 빌드 로그의 형식은 Bamboo 버전 및 구성에 따라 다를 수 있습니다. 위의 단계는 일반적인 절차를 제공하며, 실제 로그에서 특정 부분을 찾기 위해서는 로그를 자세히 살펴봐야 합니다.

## 사용자 입력:
> bamboo shell스크립트로 ssh 공개키 생성되나?

## ChatGPT 응답:
> Bamboo는 일반적으로 SSH 공개 키를 생성하는 기능을 제공하지 않습니다. SSH 공개 키는 일반적으로 로컬 시스템에서 생성되어야 합니다.

공개 키를 생성하려면 다음과 같은 절차를 따를 수 있습니다.

1. 로컬 컴퓨터에서 터미널 또는 명령 프롬프트를 엽니다.

2. SSH 키 생성 명령을 실행합니다. OpenSSH를 사용하는 경우 다음과 같은 명령을 사용할 수 있습니다:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

위 명령에서 `your_email@example.com` 부분을 본인의 이메일 주소로 대체해야 합니다.

3. 키를 저장할 경로를 지정하고 암호를 설정하라는 메시지가 표시됩니다. 기본값으로 엔터를 눌러 사용할 수 있습니다.

4. 공개 키와 개인 키가 생성됩니다. 공개 키는 `.pub` 확장자를 가지며 개인 키는 해당 확장자가 없습니다.

5. 생성된 공개 키를 Bamboo에 등록해야 합니다. Bamboo 웹 인터페이스에서 해당 프로젝트 또는 빌드 계획의 설정으로 이동하고, SSH 설정 섹션에서 공개 키를 등록하는 옵션을 찾을 수 있습니다. 이를 통해 Bamboo가 해당 키를 사용하여 SSH 연결을 수행할 수 있습니다.

요약하자면, Bamboo의 빌드 스크립트 자체에서 SSH 공개 키를 생성하는 기능은 없으며, 공개 키는 로컬 시스템에서 생성한 후 Bamboo에 등록해야 합니다.

## 사용자 입력:
> bitbucket에서 ssh와 http 둘다 지원하게 설정하려면?

## ChatGPT 응답:
> Bitbucket에서 SSH와 HTTP 둘 다 지원하도록 설정하려면 다음 단계를 따를 수 있습니다:

1. Bitbucket 웹 인터페이스에 로그인합니다.

2. 해당 프로젝트의 설정으로 이동합니다.

3. 좌측 탐색 메뉴에서 "Repository settings" (또는 "저장소 설정")을 선택합니다.

4. "Repository settings" 페이지에서 "Repository details" (또는 "저장소 세부정보") 탭으로 이동합니다.

5. "Repository details" 탭에서 "Clone URLs" (또는 "복제 URL") 섹션을 찾습니다.

6. "Clone URLs" 섹션에서 "HTTPS" 및 "SSH" 옵션을 확인할 수 있습니다.

7. 원하는 옵션을 선택합니다. SSH를 사용하려면 "SSH" 옵션을 선택하고, HTTP를 사용하려면 "HTTPS" 옵션을 선택합니다.

8. 변경 내용을 저장합니다.

이렇게 하면 Bitbucket에서 해당 저장소에 대해 SSH 및 HTTP를 모두 사용할 수 있게 됩니다. 사용자는 자신의 환경과 선호도에 따라 적절한 프로토콜을 선택하여 저장소를 클론하거나 업데이트할 수 있습니다.

## 사용자 입력:
> 'jgraphx' 경로에 대해 'jgraphx' (ssh://git@192.168.0.119:7999/qtss/jgraphx.git) 하위 모듈 등록
'/data1/bamboo-data/xml-data/build-dir/QTRAC-QSS-JOB1/jgraphx'에 복제합니다...
Host key verification failed.
fatal: 리모트 저장소에서 읽을 수 없습니다

올바른 접근 권한이 있는지, 그리고 저장소가 있는지
확인하십시오.
fatal: 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git'에서 하위 모듈 경로 '/data1/bamboo-data/xml-data/build-dir/QTRAC-QSS-JOB1/jgraphx'에 복제하는데 실패했습니다
'jgraphx' 복제하는데 실패했습니다. 재시도 예정
'/data1/bamboo-data/xml-data/build-dir/QTRAC-QSS-JOB1/jgraphx'에 복제합니다...
Host key verification failed.
fatal: 리모트 저장소에서 읽을 수 없습니다

## ChatGPT 응답:
> 이 문제는 호스트 키 검증에 실패하여 발생하는 것으로 보입니다. 호스트 키 검증은 SSH 연결을 보호하기 위해 수행되는 보안 절차입니다. 호스트 키 검증이 실패하면 SSH 연결이 거부될 수 있습니다.

문제를 해결하기 위해 다음 절차를 따르십시오:

1. 호스트 키 검증 실패 메시지가 나타나는 컴퓨터에서 터미널 또는 명령 프롬프트를 엽니다.

2. 해당 호스트(192.168.0.119)에 대한 SSH 호스트 키를 수동으로 검증합니다. 다음 명령을 실행하여 호스트 키를 검증할 수 있습니다:

```
ssh-keyscan 192.168.0.119
```

위 명령은 호스트 키를 검색하고 출력합니다.

3. 출력된 호스트 키를 복사합니다.

4. 호스트 키를 Bitbucket 또는 Bamboo의 해당 저장소 설정으로 이동하여 등록합니다. 이를 통해 Bitbucket 또는 Bamboo가 해당 호스트 키를 신뢰하고 SSH 연결을 수행할 수 있습니다. 저장소 설정에 대해 SSH 키 등록 옵션을 찾아 입력하십시오.

5. 변경 내용을 저장하고 다시 시도합니다.

위 단계를 따르면 호스트 키 검증이 성공하고 SSH 연결이 수립될 것입니다. 이제 Bitbucket 또는 Bamboo에서 해당 저장소의 하위 모듈을 복제할 수 있어야 합니다.

## 사용자 입력:
> build	13-7월-2023 11:43:50	192.168.0.119 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnSIduSHj5HsYLuyXea7CbviAuKPjqrVqXfNVI8CQ11JUoy0kVcym6hZrv/6SU/wnv5sIIPzKI55LCr/8F54GCf/EZdF3xlOqPtT+osRcBUkZeQ0A97BNM/YgB0QJqKvfBdWrEWRhMELivH+cGYNcVjF+R5S4CGijTh17XlvYxi+j4AieNDdUO+vW31BZCNNRdWGjunJCeq5/ACRFyRaTYyUsLNBFS1IX0wXilX38hUtmsLnU3Jbo8nNITBK4dSnR+7qG1iXIVzoE8kynqWjvjIH4tib3ySLXWHnbgCmQowlFZmgmMnVv3jKdVas0LoYsAGweQp6RHSrT/vkMXQP0x
build	13-7월-2023 11:43:50	192.168.0.119 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJGb6Otarm8rMMC5SpffuTqQUh0lo64dB7x64QmMByXtsVBIh3WObm/KLCGqphEVrKkIR7Z8UmPeStqbyeWz6KM=
error	13-7월-2023 11:43:50	'jgraphx' 경로에 대해 'jgraphx' (ssh://git@192.168.0.119:7999/qtss/jgraphx.git) 하위 모듈 등록
error	13-7월-2023 11:43:50	'/data1/bamboo-data/xml-data/build-dir/QTRAC-QSS-JOB1/jgraphx'에 복제합니다...
error	13-7월-2023 11:43:50	Host key verification failed.

## ChatGPT 응답:
> 주어진 로그를 보면, 호스트 키 검증이 실패하여 복제 작업이 실패했다는 것을 알 수 있습니다. 이 문제를 해결하기 위해 다음 단계를 따르십시오:

1. 호스트 키 검증에 실패한 호스트(192.168.0.119)에서 터미널 또는 명령 프롬프트를 엽니다.

2. 다음 명령을 사용하여 호스트 키를 수동으로 검증합니다:

   ```
   ssh-keyscan 192.168.0.119
   ```

   위 명령은 호스트 키를 검색하고 출력합니다.

3. 출력된 호스트 키를 복사합니다.

4. Bitbucket 또는 Bamboo의 저장소 설정으로 이동하여 해당 호스트 키를 등록합니다. 저장소 설정에서 호스트 키 등록 옵션을 찾아 입력하십시오.

5. 변경 사항을 저장하고 다시 시도합니다.

위 단계를 따르면 호스트 키 검증이 성공하고 SSH 연결이 수립될 것입니다. 이제 Bitbucket 또는 Bamboo에서 해당 저장소의 하위 모듈을 복제할 수 있어야 합니다.

