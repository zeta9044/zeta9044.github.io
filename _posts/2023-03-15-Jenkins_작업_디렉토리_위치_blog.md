---
layout: post
title: "Jenkins 작업 디렉토리 위치"
date: 2023-03-15
tags: [Jenkins, 위치, 디렉토리, 작업]
categories: blog
---

## 사용자 입력:
> jenkins job에서 execute shell 안 cd ~하면 어느 디렉토리로 가나?

## ChatGPT 응답:
> Jenkins에서 execute shell을 사용할 때, `cd ~` 명령어를 사용하지 않으면 기본적으로 Jenkins 작업의 "작업 공간" (workspace) 디렉토리로 갑니다. 작업 공간은 각 Jenkins 작업에 대해 자동으로 생성되며, 소스 코드 체크아웃, 빌드 및 테스트와 같은 작업을 수행하기 위한 공간입니다. 일반적으로 이 경로는 Jenkins의 홈 디렉토리 아래에 위치해 있습니다. 예를 들어, Linux 시스템에서 Jenkins의 기본 홈 디렉토리는 `/var/lib/jenkins`입니다. 따라서 작업 공간 디렉토리는 `/var/lib/jenkins/workspace/your_job_name`과 같이 될 것입니다. 하지만 Jenkins 구성에 따라 위치가 다를 수 있습니다.

Jenkins 관리자가 작업 공간의 경로를 변경했다면, 변경된 경로로 이동하게 됩니다. 이를 확인하려면, Jenkins 웹 인터페이스에서 해당 작업을 선택한 후 "작업 구성" 또는 "Configure"로 이동하고, "고급 프로젝트 옵션" 아래에서 "사용자 지정 작업 공간"을 확인하십시오.

## 사용자 입력:
> Jenkins에서 execute shell을 사용할 때, cd ~ 명령어를 사용하면 어디로 가나?

## ChatGPT 응답:
> Jenkins에서 execute shell을 사용할 때 `cd ~` 명령어를 사용하면, 해당 shell이 실행되는 사용자의 홈 디렉토리로 이동합니다. Jenkins는 대부분의 경우 'jenkins' 사용자로 실행되므로, 이 명령은 Jenkins 사용자의 홈 디렉토리로 이동하게 됩니다.

리눅스 시스템에서 Jenkins 사용자의 홈 디렉토리는 일반적으로 `/var/lib/jenkins`입니다. 다른 운영 체제에서는 Jenkins 사용자의 홈 디렉토리 경로가 다를 수 있으며, 시스템 관리자가 Jenkins 설치시에 사용자 및 경로를 변경한 경우에도 달라질 수 있습니다.

`cd ~` 명령을 실행한 후, Jenkins 사용자의 홈 디렉토리로 이동한 상태에서 작업을 수행하게 됩니다. 작업을 실행하기 전에 원하는 디렉토리로 이동하는 것이 좋습니다. 이렇게 하면 Jenkins 작업의 결과물이나 중간 파일들이 사용자의 홈 디렉토리에 군데군데 저장되는 것을 방지할 수 있습니다.

