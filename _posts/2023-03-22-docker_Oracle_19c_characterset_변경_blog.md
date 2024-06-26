---
layout: post
title: "docker Oracle 19c characterset 변경"
tags: [docker, 변경, Oracle, characterset]
---

## 사용자 입력:
> docker oracle-19c에서 chacterset을 win949로 바꿀려면?

## ChatGPT 응답:
> 

Docker에서 Oracle 19c 이미지를 실행하고, characterset을 win949로 변경하려면 다음 단계를 수행할 수 있습니다.

1. 우선, Docker에서 Oracle 19c 이미지를 실행합니다.

{% raw %}```
docker run -d -it --name oracle19c \
-p 1521:1521 -p 5500:5500 \
-e ORACLE_SID=ORCLCDB \
-e ORACLE_PDB=ORCLPDB1 \
-e ORACLE_CHARACTERSET=AL32UTF8 \
oracle/database:19.3.0-ee
```{% endraw %}

2. 이미지가 실행되면, Docker 컨테이너에 접속합니다.

{% raw %}```
docker exec -it oracle19c bash
```{% endraw %}

3. 다음으로, Oracle SQL*Plus를 사용하여 SYSDBA로 로그인합니다.

{% raw %}```
sqlplus / as sysdba
```{% endraw %}

4. 데이터베이스 인코딩을 변경하기 전에, 현재 설정된 인코딩을 확인합니다.

{% raw %}```
SELECT value FROM nls_database_parameters WHERE parameter = 'NLS_CHARACTERSET';
```{% endraw %}

5. 인코딩을 변경하기 위해 데이터베이스를 종료합니다.

{% raw %}```
SHUTDOWN IMMEDIATE;
```{% endraw %}

6. 데이터베이스의 인코딩을 변경합니다.

{% raw %}```
ALTER SYSTEM SET NLS_CHARACTERSET=WIN949;
```{% endraw %}

7. 데이터베이스를 다시 시작합니다.

{% raw %}```
STARTUP;
```{% endraw %}

8. 변경된 인코딩이 정상적으로 적용되었는지 확인합니다.

{% raw %}```
SELECT value FROM nls_database_parameters WHERE parameter = 'NLS_CHARACTERSET';
```{% endraw %}

위의 단계를 수행하면 Oracle 19c Docker 이미지의 characterset을 win949로 변경할 수 있습니다.

