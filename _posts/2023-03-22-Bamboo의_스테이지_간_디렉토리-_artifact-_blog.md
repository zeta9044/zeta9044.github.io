---
layout: post
title: "Bamboo의 스테이지 간 디렉토리: artifact."
date: 2023-03-22
tags: [스테이지, 간, 디렉토리, 의, artifact]
categories: blog
---

## 사용자 입력:
> bamboo에서 build할 때, stage간 디렉토리를 동일하게 유지되는가?

## ChatGPT 응답:
> Bamboo는 일반적으로 스테이지 간에 디렉토리를 동일하게 유지하지 않습니다. 각 스테이지는 실행 환경에서 새로운 컨테이너를 시작하며, 이는 이전 스테이지의 컨테이너와 완전히 분리됩니다.

따라서 Bamboo에서 각 스테이지에서 생성된 파일은 그 다음 스테이지에서 사용할 수 없습니다. 그러나 파일 공유를 위해 Bamboo에서 제공하는 옵션 중 하나는 artifact라는 것입니다. Artifact는 스테이지 간에 파일을 공유하기 위한 Bamboo의 내장 기능입니다. 이를 사용하면 이전 스테이지에서 생성된 파일을 다음 스테이지에서 사용할 수 있습니다. 

Artifact는 빌드에서 생성된 파일을 저장하고 필요한 다른 작업에서 사용할 수 있도록 구성된 디렉토리입니다. Bamboo 빌드 작업에서 파일을 artifact 디렉토리에 추가하면 다음 스테이지에서 이 파일을 다운로드하고 사용할 수 있습니다. 이를 통해 스테이지 간에 파일을 공유할 수 있으며, 이를 통해 파일 및 디렉토리 구조를 일관되게 유지할 수 있습니다.

## 사용자 입력:
> bamboo에서 task진행을 그래프로 표시해 주는가?

## ChatGPT 응답:
> Bamboo는 작업 진행 그래프를 표시하는 기능을 제공합니다. 이 그래프를 사용하면 빌드 프로세스의 각 단계에서 실행되는 작업 및 스크립트를 시각적으로 파악할 수 있습니다.

Bamboo의 작업 진행 그래프는 각 빌드가 표시되는 빌드 결과 요약 페이지에서 볼 수 있습니다. 이 그래프는 빌드의 각 스테이지와 해당 스테이지에서 실행된 작업을 시각적으로 표시합니다.

그래프는 다양한 색상으로 구성되며, 각 색상은 빌드의 특정 단계나 작업을 나타냅니다. 빌드가 실패한 경우, 해당 단계 또는 작업의 색상이 빨간색으로 변경되어 실패를 나타내고, 성공한 경우에는 녹색으로 변경됩니다.

또한 Bamboo는 각 단계 또는 작업에서 로그를 표시하므로 빌드를 추적하고 문제를 해결할 수 있습니다. 이러한 기능을 통해 Bamboo는 작업 진행 상황을 쉽게 파악하고 빌드 문제를 해결하는 데 도움이 됩니다.

## 사용자 입력:
> Listener refused the connection with the following error:
ORA-12526, TNS:listener: all appropriate instances are in restricted mode
 
Listener refused the connection with the following error:
ORA-12526, TNS:listener: all appropriate instances are in restricted mode
 


## ChatGPT 응답:
> 이 오류는 Oracle 데이터베이스 인스턴스가 제한 모드로 실행되고 있어서 연결이 거부된 것을 나타냅니다. 제한 모드는 일반적으로 데이터베이스 유지 보수 작업을 수행할 때 사용됩니다. 이 모드에서는 데이터베이스에 대한 일부 기능이 제한됩니다.

이 오류를 해결하려면 다음과 같은 단계를 수행할 수 있습니다.

1. 데이터베이스 관리자에게 문의하여 데이터베이스 인스턴스가 실제로 제한 모드에 있는지 확인하세요.

2. 인스턴스가 제한 모드에 있으면, 다음 명령을 사용하여 인스턴스를 종료합니다.

   ```
   SQL> shutdown immediate;
   ```

3. 데이터베이스 인스턴스를 재시작하고 일반 모드로 실행합니다.

   ```
   SQL> startup;
   ```

4. 데이터베이스가 정상적으로 시작되었는지 확인합니다.

   ```
   SQL> select status from v$instance;
   ```

   이 명령은 인스턴스 상태를 반환합니다. "OPEN"이라는 결과가 표시되면 데이터베이스가 정상적으로 시작된 것입니다.

5. 다시 시도하여 연결이 정상적으로 이루어지는지 확인합니다.

위 단계를 수행하더라도 문제가 지속되는 경우, 데이터베이스 관리자 또는 시스템 관리자에게 문의하여 추가적인 지원을 받을 수 있습니다.

## 사용자 입력:
> docker oracle-19c로 실행했을때, Listener refused the connection with the following error:
ORA-12526, TNS:listener: all appropriate instances are in restricted mode
 
Listener refused the connection with the following error:
ORA-12526, TNS:listener: all appropriate instances are in restricted mode
 
이런 메세지가 나오면 어떻게 하나?

## ChatGPT 응답:
> Docker에서 Oracle 19c를 실행할 때 이 오류가 발생하는 경우, 일반적으로 컨테이너를 처음 시작할 때 인스턴스가 제한 모드로 시작되는 것입니다. 이 경우, 인스턴스를 정상 모드로 변경해야 합니다.

다음은 이 문제를 해결하기 위한 몇 가지 단계입니다.

1. Docker 컨테이너를 시작합니다.

   ```
   $ docker start <container-name>
   ```

2. Docker 컨테이너에 대한 bash 셸을 실행합니다.

   ```
   $ docker exec -it <container-name> bash
   ```

3. 다음 명령을 사용하여 Oracle 인스턴스가 제한 모드로 시작되었는지 확인합니다.

   ```
   $ sqlplus / as sysdba
   SQL> SELECT logins FROM v$instance;
   ```

   이 명령은 인스턴스가 "RESTRICTED" 상태인지 확인합니다. 결과가 "RESTRICTED"이면, 인스턴스가 제한 모드로 시작되었습니다.

4. 다음 명령을 사용하여 인스턴스를 정상 모드로 변경합니다.

   ```
   SQL> ALTER SYSTEM DISABLE RESTRICTED SESSION;
   ```

5. 인스턴스를 다시 시작합니다.

   ```
   SQL> SHUTDOWN IMMEDIATE;
   SQL> STARTUP;
   ```

6. 다시 시도하여 연결이 정상적으로 이루어지는지 확인합니다.

위 단계를 수행하여도 문제가 지속되는 경우, Oracle 데이터베이스 관리자에게 문의하여 추가적인 지원을 받을 수 있습니다.

## 사용자 입력:
> docker oracle에서 ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


## ChatGPT 응답:
> 이 오류는 Oracle 데이터베이스 인스턴스가 시작되지 않았거나, 시작 중에 문제가 발생하여 데이터베이스가 사용할 수 없는 상태인 경우 발생합니다. 이를 해결하기 위해서는 데이터베이스 인스턴스를 시작하거나 다시 시작해야 합니다.

다음은 이 문제를 해결하기 위한 몇 가지 단계입니다.

1. Docker 컨테이너를 시작합니다.

   ```
   $ docker start <container-name>
   ```

2. Docker 컨테이너에 대한 bash 셸을 실행합니다.

   ```
   $ docker exec -it <container-name> bash
   ```

3. 다음 명령을 사용하여 Oracle 인스턴스를 시작합니다.

   ```
   $ sqlplus /nolog
   SQL> CONNECT / AS SYSDBA
   SQL> STARTUP
   ```

4. 인스턴스가 정상적으로 시작되면 다시 시도하여 연결이 정상적으로 이루어지는지 확인합니다.

이러한 단계를 수행하더라도 문제가 지속되는 경우, Docker 이미지에서 Oracle 데이터베이스를 올바르게 구성하지 않았거나 다른 문제가 있을 수 있습니다. 이 경우 Docker 이미지를 다시 작성하거나 Oracle 데이터베이스 관리자에게 문의하여 추가적인 지원을 받을 수 있습니다.

