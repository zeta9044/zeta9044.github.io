---
layout: post
title: "Oracle SQL*Plus ORA-01034 Troubleshooting"
date: 2023-12-07
tags: [SQL, Troubleshooting, ORA, Plus, Oracle]
categories: blog
---

## 사용자 입력:
> 오라클  sqlplus에서 SQL> start /home/lia/scripts/qtrack_tablespace_100G.sql
CREATE TABLESPACE QTRACK
*
ERROR at line 1:
ORA-01034: ORACLE not available
Process ID: 0
Session ID: 0 Serial number: 0


CREATE USER LIAUSR IDENTIFIED BY LIAUSR
*
ERROR at line 1:
ORA-01034: ORACLE not available


## ChatGPT 응답:
> The errors you're encountering in Oracle SQL*Plus indicate that the Oracle database instance is not currently available. The `ORA-01034: ORACLE not available` error typically occurs when the Oracle instance is not started or when there are issues with the connection to the database. Here are a few steps you can take to troubleshoot and resolve this issue:

1. **Check Database Instance Status:**
   - Ensure that the Oracle database instance is up and running. You can do this by using the `sqlplus` command to connect as `SYSDBA` and then executing `startup` to start the instance if it's not already running.

2. **Verify Environment Variables:**
   - Make sure that your environment variables such as `ORACLE_HOME` and `ORACLE_SID` are set correctly. These variables are crucial for SQL*Plus to connect to the correct Oracle instance.

3. **Check Listener Status:**
   - Verify that the Oracle listener process is running. You can check the status of the listener by using the `lsnrctl status` command. If the listener is not running, start it using `lsnrctl start`.

4. **Database Connectivity:**
   - If you are connecting to a remote database, ensure that the network connection is stable and the database URL, username, and password are correct.

5. **Check for Database Locks or Shutdown:**
   - Sometimes, the database might be in a locked state or might be shutting down. Check for any such issues that might prevent new connections.

6. **Consult Oracle Logs:**
   - Check the Oracle logs for any detailed error messages or alerts that can provide more insight into why the instance is not available.

7. **Permissions and User Roles:**
   - Ensure that the user you are using to execute these commands has the necessary permissions and roles assigned.

8. **Database Recovery:**
   - In some cases, the database might need recovery due to an improper shutdown or other issues. If this is the case, consult the Oracle documentation for recovery procedures.

If you continue to face issues, it might be helpful to consult the Oracle documentation specific to your version or reach out to Oracle support for more specialized assistance.

## 사용자 입력:
> 한글로 알려줘.

## ChatGPT 응답:
> Oracle SQL*Plus에서 'ORA-01034: ORACLE not available' 오류가 발생하는 것은 Oracle 데이터베이스 인스턴스가 현재 사용할 수 없다는 것을 나타냅니다. 이 오류는 일반적으로 Oracle 인스턴스가 시작되지 않았거나 데이터베이스에 연결하는 데 문제가 있을 때 발생합니다. 이 문제를 해결하기 위해 다음과 같은 조치를 취할 수 있습니다:

1. **데이터베이스 인스턴스 상태 확인:**
   - Oracle 데이터베이스 인스턴스가 실행 중인지 확인하세요. `sqlplus` 명령어로 `SYSDBA`로 연결한 후, 이미 실행 중이지 않다면 `startup` 명령어를 실행하여 인스턴스를 시작할 수 있습니다.

2. **환경 변수 검증:**
   - `ORACLE_HOME` 및 `ORACLE_SID`와 같은 환경 변수가 올바르게 설정되었는지 확인하세요. 이러한 변수들은 SQL*Plus가 올바른 Oracle 인스턴스에 연결하는 데 필수적입니다.

3. **리스너 상태 확인:**
   - Oracle 리스너 프로세스가 실행 중인지 확인하세요. `lsnrctl status` 명령어를 사용하여 리스너의 상태를 확인할 수 있습니다. 리스너가 실행되지 않고 있다면, `lsnrctl start`를 사용하여 시작하세요.

4. **데이터베이스 연결성:**
   - 원격 데이터베이스에 연결하는 경우, 네트워크 연결이 안정적인지, 데이터베이스 URL, 사용자 이름 및 비밀번호가 정확한지 확인하세요.

5. **데이터베이스 락이나 종료 확인:**
   - 때때로 데이터베이스가 잠겨 있거나 종료 중일 수 있습니다. 새로운 연결을 방해할 수 있는 이러한 문제를 확인하세요.

6. **Oracle 로그 확인:**
   - 인스턴스가 사용할 수 없는 이유에 대한 자세한 오류 메시지나 경고를 제공할 수 있는 Oracle 로그를 확인하세요.

7. **권한 및 사용자 역할:**
   - 이 명령어를 실행하는 데 사용하는 사용자가 필요한 권한과 역할을 가지고 있는지 확인하세요.

8. **데이터베이스 복구:**
   - 경우에 따라 데이터베이스가 부적절한 종료나 기타 문제로 인해 복구가 필요할 수 있습니다. 이 경우 Oracle 문서를 참조하여 복구 절차를 확인하세요.

문제가 계속되는 경우에는 사용 중인 버전에 맞는 Oracle 문서를 참조하거나 Oracle 지원에 문의하여 보다 전문적인 도움을 받는 것이 좋습니다.

## 사용자 입력:
> SQL> startup
ORA-01078: failure in processing system parameters
LRM-00109: could not open parameter file '/opt/oracle/product/19c/dbhome_1/dbs/initoracle.ora'
SQL> exit
Disconnected


## ChatGPT 응답:
> Oracle SQL*Plus에서 발생하는 `ORA-01078: failure in processing system parameters` 및 `LRM-00109: could not open parameter file '/opt/oracle/product/19c/dbhome_1/dbs/initoracle.ora'` 오류는 데이터베이스가 시스템 매개변수를 처리하는 데 실패하고, 이에 필요한 매개변수 파일을 열 수 없음을 나타냅니다. 이를 해결하기 위한 몇 가지 조치는 다음과 같습니다:

1. **매개변수 파일의 경로와 이름 확인:**
   - 오류 메시지에 나타난 경로와 파일 이름이 올바른지 확인하세요. 일반적으로 매개변수 파일의 이름은 `init<SID>.ora` 형식이며, `<SID>`는 데이터베이스의 SID입니다. `initoracle.ora` 파일이 존재하는지, 그리고 올바른 위치에 있는지 확인하세요.

2. **파일 접근 권한 확인:**
   - 해당 매개변수 파일에 대한 읽기 권한이 있는지 확인하세요. Oracle 데이터베이스 소프트웨어를 실행하는 사용자가 이 파일에 접근할 수 있어야 합니다.

3. **환경 변수 검증:**
   - `ORACLE_HOME` 및 `ORACLE_SID` 환경 변수가 올바르게 설정되어 있는지 확인하세요. 이 변수들은 Oracle이 매개변수 파일을 찾는 데 중요합니다.

4. **매개변수 파일 내용 확인:**
   - 매개변수 파일을 열어 올바른 구성이 되어 있는지 확인하세요. 파일이 손상되었거나 잘못된 구성이 있는 경우 이러한 오류가 발생할 수 있습니다.

5. **매개변수 파일 복구 또는 재생성:**
   - 매개변수 파일이 손상되었거나 없는 경우, 백업에서 복구하거나 새로운 매개변수 파일을 생성할 수 있습니다.

6. **Oracle 데이터베이스 로그 확인:**
   - 추가적인 오류 메시지나 정보를 얻기 위해 Oracle 데이터베이스의 로그 파일을 확인하세요.

7. **Oracle 문서 및 지원:**
   - 이 문제에 대한 구체적인 해결책을 찾기 위해 Oracle 데이터베이스 버전에 맞는 공식 문서를 참조하거나 Oracle 지원에 문의할 수 있습니다.

이러한 단계를 수행한 후에도 문제가 해결되지 않으면, 전문적인 Oracle 지원을 받는 것이 좋습니다. 데이터베이스 설정과 관리에는 복잡한 요소들이 많으며, 경우에 따라 전문적인 지식이 필요할 수 있습니다.

## 사용자 입력:
> [oracle@65a141497761 ~]$ ls -al /opt/oracle/product/19c/dbhome_1/dbs
total 24
drwxr-xr-x 1 oracle dba      4096 Dec  7 06:30 .
drwxr-xr-x 1 oracle dba      4096 Dec  7 06:30 ..
-rw-rw---- 1 oracle oinstall 1544 Dec  7 06:30 hc_ORACLE.dat
-rw-r--r-- 1 oracle dba      3079 May 14  2015 init.ora
-rw-r----- 1 oracle oinstall   24 Dec  7 06:30 lkORACLE
lrwxrwxrwx 1 oracle oinstall   47 Dec  7 06:30 orapwORACLE -> /opt/oracle/oradata/dbconfig/ORACLE/orapwORACLE
lrwxrwxrwx 1 oracle oinstall   52 Dec  7 06:30 spfileORACLE.ora -> /opt/oracle/oradata/dbconfig/ORACLE/spfileORACLE.ora


## ChatGPT 응답:
> 파일 목록을 보면 `/opt/oracle/product/19c/dbhome_1/dbs` 디렉토리에는 `init.ora` 파일이 존재하며, 이는 표준 초기화 매개변수 파일의 이름 형식과 다릅니다. 일반적으로 초기화 매개변수 파일의 이름은 `init<SID>.ora` 형식을 따릅니다. 예를 들어, 데이터베이스 SID가 `ORACLE`이라면 파일 이름은 `initORACLE.ora`가 되어야 합니다. 여기서는 이러한 이름의 파일이 보이지 않습니다.

이 문제를 해결하기 위한 몇 가지 조치는 다음과 같습니다:

1. **적절한 초기화 매개변수 파일 생성 또는 변경:**
   - 현재 `init.ora` 파일을 올바른 이름으로 복사하거나 변경해야 할 수 있습니다. 예를 들어, `cp init.ora initORACLE.ora` 명령어를 사용하여 파일을 복사할 수 있습니다.

2. **심볼릭 링크 확인:**
   - `spfileORACLE.ora` 파일이 심볼릭 링크로 존재합니다. 이 파일이 실제 SPFILE(서버 매개변수 파일)을 가리키고 있는지 확인하세요. 이 파일은 데이터베이스 시작 시 사용되는 매개변수를 포함합니다.

3. **환경 변수 재확인:**
   - `ORACLE_SID` 환경 변수가 정확히 설정되어 있는지 다시 한번 확인하세요. 이 변수는 Oracle이 사용할 데이터베이스 인스턴스를 식별하는 데 사용됩니다.

4. **파일 및 권한 확인:**
   - 생성하거나 변경한 파일의 권한을 확인하고, Oracle 데이터베이스 소프트웨어를 실행하는 사용자가 이 파일에 접근할 수 있도록 하세요.

5. **데이터베이스 재시작 시도:**
   - 변경 사항을 적용한 후, 데이터베이스를 다시 시작해 보세요. `sqlplus / as sysdba`로 로그인 한 후, `startup` 명령어를 사용합니다.

6. **로그 파일 및 오류 메시지 확인:**
   - 데이터베이스를 다시 시작할 때 발생하는 오류 메시지나 로그 파일을 확인하여 추가적인 문제가 있는지 확인하세요.

7. **Oracle 지원:**
   - 위의 단계를 모두 수행한 후에도 문제가 해결되지 않으면, Oracle 지원에 문의하여 도움을 받는 것이 좋습니다.

## 사용자 입력:
> 그러면, docker container의 sqlplus사용법이 달라야 되는거 아녀?

## ChatGPT 응답:
> Docker 컨테이너 내에서 SQL*Plus를 사용할 때의 기본 접근 방식은 호스트 시스템에서 SQL*Plus를 사용하는 것과 크게 다르지 않습니다. 그러나 Docker 환경은 몇 가지 추가적인 고려 사항을 필요로 합니다. 이러한 고려 사항은 다음과 같습니다:

1. **컨테이너 실행 및 접근:**
   - Oracle 데이터베이스가 포함된 Docker 컨테이너를 실행할 때, 필요한 포트, 볼륨, 환경 변수 등을 올바르게 설정해야 합니다.
   - 데이터베이스가 실행 중인 컨테이너에 접근하려면, `docker exec` 명령을 사용해 컨테이너 내부에 쉘 세션을 시작할 수 있습니다.

2. **환경 변수 설정:**
   - Docker 컨테이너 내부에서는 `ORACLE_HOME`, `ORACLE_SID` 등의 환경 변수가 컨테이너 구성에 따라 미리 설정되어 있거나, 수동으로 설정해야 할 수 있습니다.

3. **네트워크 설정:**
   - 컨테이너가 네트워크에 어떻게 노출되는지에 따라, 컨테이너 외부에서 SQL*Plus를 사용하여 데이터베이스에 연결하는 경우에는 적절한 네트워크 포트 설정이 필요합니다.

4. **파일 시스템 및 볼륨:**
   - Docker 컨테이너는 격리된 파일 시스템을 사용합니다. 따라서 호스트 시스템의 파일을 컨테이너 내에서 접근하려면, 해당 파일들이 Docker 볼륨을 통해 컨테이너에 마운트되어 있어야 합니다.

5. **컨테이너 내 Oracle 서비스 관리:**
   - 컨테이너 내에서 Oracle 서비스를 시작, 중지 또는 재시작할 때는 컨테이너의 특성을 고려하여 관련 명령어를 실행해야 합니다.

6. **로그 파일 및 진단 정보:**
   - 오류 진단 시, 컨테이너 내부의 Oracle 로그 파일 위치를 확인해야 할 수 있습니다.

Docker를 사용하여 Oracle 데이터베이스를 실행하는 경우, Oracle 공식 이미지 또는 신뢰할 수 있는 다른 소스에서 제공하는 이미지의 문서와 지침을 참고하는 것이 좋습니다. 이러한 문서는 일반적으로 컨테이너 실행, 환경 설정, 데이터베이스 접근 방법 등에 대한 구체적인 안내를 제공합니다.

