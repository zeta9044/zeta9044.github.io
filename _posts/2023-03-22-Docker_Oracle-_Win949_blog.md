---
layout: post
title: "Docker Oracle: Win949"
date: 2023-03-22
tags: [Oracle, Docker]
categories: blog
---

## 사용자 입력:
> docker oracle-19c에서 character set을 win949로 바꿀려면?

## ChatGPT 응답:
> Oracle Database 19c를 Docker 컨테이너에서 실행하고 있고, character set을 win949로 변경하고자 한다면 다음과 같은 단계를 따를 수 있습니다:

1. 새로운 Docker 이미지 생성

다음 Dockerfile을 작성합니다:

```
FROM container-registry.oracle.com/database/standard:19.3.0.0
ENV NLS_LANG=AMERICAN_AMERICA.KO16MSWIN949
```

위 Dockerfile에서는 Oracle Database 19c Standard Edition 2 이미지를 기반으로 하고, `NLS_LANG` 환경 변수를 설정합니다. 이 변수는 Oracle 데이터베이스에서 사용하는 언어와 문자 집합을 지정하는 데 사용됩니다.

2. Docker 이미지 빌드

Dockerfile이 저장된 디렉토리에서 다음 명령어를 실행하여 새로운 Docker 이미지를 빌드합니다:

```
docker build -t oracle-19c-win949 .
```

위 명령어에서 `oracle-19c-win949`는 새로운 Docker 이미지의 이름입니다.

3. Docker 컨테이너 실행

다음 명령어를 실행하여 새로운 Docker 이미지를 기반으로한 컨테이너를 실행합니다:

```
docker run --name oracle-19c-container -p 1521:1521 -p 5500:5500 -e ORACLE_PDB=ORCLPDB1 -e ORACLE_CHARACTERSET=KO16MSWIN949 -d oracle-19c-win949
```

위 명령어에서 `ORACLE_CHARACTERSET` 환경 변수를 `KO16MSWIN949`로 설정하여 새로운 character set을 적용합니다. `ORACLE_PDB` 환경 변수는 pluggable database의 이름을 지정하는 데 사용됩니다.

위 단계를 따르면 Oracle Database 19c를 Docker 컨테이너에서 실행하면서 character set을 win949로 변경할 수 있습니다.

## 사용자 입력:
> 기존 컨테이너에서 win949캐릭터 셋으로 바꾸는 방법은 없나?

## ChatGPT 응답:
> 기존에 실행 중인 Docker 컨테이너에서 Oracle Database의 character set을 win949로 변경하려면 다음 단계를 따를 수 있습니다:

1. Docker 컨테이너에 접속

먼저 실행 중인 Docker 컨테이너에 접속합니다. 다음 명령어를 실행하여 컨테이너에 접속합니다:

```
docker exec -it <container_name> bash
```

위 명령어에서 `<container_name>`은 접속하려는 컨테이너의 이름입니다.

2. Oracle Database 설정 변경

다음 명령어를 실행하여 Oracle Database의 character set을 win949로 변경합니다:

```
sqlplus /nolog
```

위 명령어를 실행하면 SQL*Plus 클라이언트가 시작됩니다. 다음 명령어를 입력하여 sysdba 권한으로 로그인합니다:

```
conn / as sysdba
```

다음 명령어를 입력하여 현재 character set을 확인합니다:

```
SELECT value FROM nls_database_parameters WHERE parameter = 'NLS_CHARACTERSET';
```

위 명령어를 실행하면 현재 character set이 표시됩니다. 이제 다음 명령어를 입력하여 character set을 win949로 변경합니다:

```
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER SYSTEM ENABLE RESTRICTED SESSION;
ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;
ALTER SYSTEM SET AQ_TM_PROCESSES=0;
ALTER DATABASE CHARACTER SET KO16MSWIN949;
ALTER SYSTEM DISABLE RESTRICTED SESSION;
ALTER SYSTEM SET JOB_QUEUE_PROCESSES=10;
ALTER SYSTEM SET AQ_TM_PROCESSES=10;
ALTER SYSTEM SET OPEN_CURSORS=1024 SCOPE=BOTH;
SHUTDOWN IMMEDIATE;
STARTUP;
```

위 명령어는 Oracle Database를 중지하고 character set을 변경한 다음 다시 시작합니다.

3. Docker 컨테이너 종료 및 재시작

Oracle Database의 character set을 변경한 후 Docker 컨테이너를 종료하고 다시 시작합니다. 다음 명령어를 실행하여 컨테이너를 종료합니다:

```
exit
```

컨테이너를 다시 시작하려면 다음 명령어를 실행합니다:

```
docker restart <container_name>
```

위 단계를 따르면 기존에 실행 중인 Docker 컨테이너에서 Oracle Database의 character set을 win949로 변경할 수 있습니다.

## 사용자 입력:
> sudo docker run -d --name oracle-19c \
-p 1521:1521 \
-e ORACLE_SID=oracle \
-e ORACLE_PWD=oracle \
-e ORACLE_CHARACTERSET=KO16MSWIN949 \
-v /home/lia/oracle-19c/oradata/:/opt/oracle/oradata \
-v /home/lia/scripts:/home/lia/scripts \
doctorkirk/oracle-19c 이 명령에서 archive 모드는 off하는 명령어는?

## ChatGPT 응답:
> Oracle Database 19c Docker 이미지를 기반으로 하는 컨테이너를 실행하면서 archive 모드를 비활성화하려면 다음 단계를 따를 수 있습니다:

1. Docker 컨테이너 실행

다음 명령어를 사용하여 Docker 컨테이너를 실행합니다:

```
sudo docker run -d --name oracle-19c \
-p 1521:1521 \
-e ORACLE_SID=oracle \
-e ORACLE_PWD=oracle \
-e ORACLE_CHARACTERSET=KO16MSWIN949 \
-v /home/lia/oracle-19c/oradata/:/opt/oracle/oradata \
-v /home/lia/scripts:/home/lia/scripts \
doctorkirk/oracle-19c
```

위 명령어에서 `-d` 옵션은 컨테이너를 백그라운드에서 실행하도록 지정합니다. `-p` 옵션은 호스트와 컨테이너 간의 포트 매핑을 설정합니다. `-e` 옵션은 환경 변수를 설정합니다. `-v` 옵션은 호스트 디렉토리와 컨테이너 디렉토리 간의 볼륨 매핑을 설정합니다.

2. Oracle Database 설정 변경

Docker 컨테이너에 접속합니다. 다음 명령어를 실행하여 SQL*Plus 클라이언트를 시작합니다:

```
docker exec -it oracle-19c sqlplus / as sysdba
```

다음 명령어를 실행하여 현재 archive 모드 상태를 확인합니다:

```
archive log list;
```

위 명령어를 실행하면 현재 archive 모드 상태가 표시됩니다. 다음 명령어를 실행하여 archive 모드를 비활성화합니다:

```
shutdown immediate;
startup mount;
alter database noarchivelog;
alter database open;
```

위 명령어는 Oracle Database를 중지하고, mount 상태로 변경한 다음, noarchivelog 모드로 설정합니다. 마지막으로 데이터베이스를 다시 엽니다.

3. Docker 컨테이너 재시작

다음 명령어를 실행하여 Docker 컨테이너를 재시작합니다:

```
sudo docker restart oracle-19c
```

위 단계를 따르면 Docker 컨테이너에서 archive 모드를 비활성화할 수 있습니다.

## 사용자 입력:
> docker container시작할 때부터 archive 모드를 비활성화하는 명령어는 없나?

## ChatGPT 응답:
> Oracle Database 19c Docker 이미지를 기반으로 하는 컨테이너를 시작할 때부터 archive 모드를 비활성화하려면 `startup` 스크립트를 수정하여 비활성화하도록 지정할 수 있습니다. 다음은 이를 수행하는 방법입니다:

1. Docker 이미지 수정

먼저 Dockerfile을 사용하여 Docker 이미지를 수정해야합니다. 새로운 Dockerfile을 만들고 다음 내용을 추가합니다:

```
FROM doctorkirk/oracle-19c:latest
COPY startup /opt/oracle/product/19c/dbhome_1/dbs/startup
```

위 Dockerfile에서는 Oracle Database 19c Docker 이미지를 기반으로 하고, `startup` 스크립트를 이미지 내부의 적절한 위치에 복사합니다.

2. startup 스크립트 수정

다음 명령어를 사용하여 `startup` 스크립트를 편집합니다:

```
sudo nano /path/to/startup
```

`startup` 스크립트의 내용을 다음과 같이 변경합니다:

```
#!/bin/bash

# Start Oracle Listener
lsnrctl start

# Start Oracle Database
sqlplus /nolog <<EOF
connect / as sysdba
startup mount;
alter database noarchivelog;
alter database open;
exit;
EOF

# Tail on the alert log to show that the database is up and running
tail -f $ORACLE_BASE/diag/rdbms/*/*/trace/alert*.log &
```

위 스크립트에서는 데이터베이스를 시작하기 전에 `noarchivelog` 모드로 데이터베이스를 마운트하도록 지정합니다.

3. Docker 이미지 빌드

다음 명령어를 사용하여 새로운 Docker 이미지를 빌드합니다:

```
docker build -t oracle-19c-noarchivelog .
```

위 명령어에서 `oracle-19c-noarchivelog`는 새로운 Docker 이미지의 이름입니다.

4. Docker 컨테이너 실행

다음 명령어를 사용하여 Docker 컨테이너를 실행합니다:

```
sudo docker run -d --name oracle-19c-noarchivelog \
-p 1521:1521 \
-e ORACLE_SID=ORCLCDB \
-e ORACLE_PDB=ORCLPDB1 \
-e ORACLE_PWD=oracle \
-v /path/to/oracle/data:/opt/oracle/oradata \
oracle-19c-noarchivelog
```

위 명령어에서는 새로운 Docker 이미지를 기반으로 하며, `-e` 옵션을 사용하여 환경 변수를 설정합니다. `ORACLE_SID`는 Oracle System Identifier를 설정하며, `ORACLE_PDB`는 Pluggable Database 이름을 설정합니다. `-v` 옵션을 사용하여 호스트 디렉토리와 컨테이너 디렉토리 간의 볼륨 매핑을 설정합니다.

위 단계를 따르면 Docker 컨테이너를 시작할 때부

