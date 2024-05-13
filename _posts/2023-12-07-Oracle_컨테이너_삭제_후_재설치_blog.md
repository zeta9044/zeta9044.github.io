---
layout: post
title: "Oracle 컨테이너 삭제 후 재설치"
tags: [설치, 삭제, 컨테이너, 재, 후, Oracle]
---

## 사용자 입력:
> docker-compose로 2개의 container (oracle,postgres)를 COMPOSE_FILE을 이용해서  up -d를 실행했는데,  oracle만 container를 완전삭제하고  새로 설치하고 싶어.

## ChatGPT 응답:
> {% raw %}`docker-compose`{% endraw %}를 사용하여 여러 컨테이너(이 경우 Oracle과 PostgreSQL)를 관리할 때 특정 컨테이너(여기서는 Oracle)만 완전히 삭제하고 새로 설치하려면 다음 단계를 따르면 됩니다.

1. **컨테이너 정지 및 삭제**: 먼저 Oracle 컨테이너를 정지하고 삭제해야 합니다. 이를 위해 {% raw %}`docker-compose down`{% endraw %} 명령을 사용하면 컨테이너, 네트워크, 볼륨 등 모든 관련 리소스가 삭제됩니다. 하지만 PostgreSQL 컨테이너도 함께 삭제되므로 이를 방지하기 위해 Oracle 컨테이너만 명시적으로 지정해야 합니다.

   {% raw %}```bash
   docker-compose stop oracle
   docker-compose rm oracle
   ```{% endraw %}

2. **Oracle 컨테이너 이미지 삭제**: 컨테이너를 삭제한 후, Oracle 컨테이너 이미지를 삭제할 수 있습니다. 이렇게 하면 새로운 이미지를 다운로드할 때 최신 버전을 가져올 수 있습니다.

   {% raw %}```bash
   docker rmi [oracle 이미지 이름]
   ```{% endraw %}

3. **컨테이너 재생성 및 실행**: 이제 {% raw %}`docker-compose up -d`{% endraw %} 명령을 사용하여 Oracle 컨테이너를 새로 생성하고 실행할 수 있습니다.

   {% raw %}```bash
   docker-compose up -d oracle
   ```{% endraw %}

이 과정은 {% raw %}`docker-compose.yml`{% endraw %} 파일에 정의된 서비스 이름을 기반으로 합니다. Oracle 컨테이너의 서비스 이름이 {% raw %}`oracle`{% endraw %}이 아니라면 해당 이름으로 대체해야 합니다.

또한, {% raw %}`COMPOSE_FILE`{% endraw %} 환경 변수를 사용하여 특정 {% raw %}`docker-compose.yml`{% endraw %} 파일을 지정했다면, 이러한 명령을 실행할 때도 동일한 환경 변수를 사용해야 합니다. 예를 들어, {% raw %}`COMPOSE_FILE=my-docker-compose.yml`{% endraw %}로 설정했다면, 명령을 실행할 때 이 환경 변수를 명시해야 합니다.

## 사용자 입력:
> 위에 docker-compose다음에 특정 서비스명을 쓰는 거야? 아니면 container명을 적는 거야?

## ChatGPT 응답:
> {% raw %}`docker-compose`{% endraw %} 명령어에서 특정 서비스를 지정할 때는 {% raw %}`docker-compose.yml`{% endraw %} 파일에 정의된 **서비스 이름**을 사용합니다. 컨테이너 이름이 아닌 서비스 이름을 사용하는 것이 중요합니다.

예를 들어, {% raw %}`docker-compose.yml`{% endraw %} 파일에 다음과 같이 두 서비스가 정의되어 있다고 가정해 보겠습니다:

{% raw %}```yaml
services:
  oracle:
    image: oracle-image
    # ... (기타 설정)

  postgres:
    image: postgres-image
    # ... (기타 설정)
```{% endraw %}

이 경우, {% raw %}`oracle`{% endraw %}과 {% raw %}`postgres`{% endraw %}는 서비스 이름입니다. 따라서 Oracle 서비스에 대해서만 작업을 수행하려면, 서비스 이름 {% raw %}`oracle`{% endraw %}을 사용하여 {% raw %}`docker-compose`{% endraw %} 명령을 실행합니다. 예를 들면:

- Oracle 서비스를 정지하려면:
  {% raw %}```bash
  docker-compose stop oracle
  ```{% endraw %}

- Oracle 서비스를 삭제하려면:
  {% raw %}```bash
  docker-compose rm oracle
  ```{% endraw %}

- Oracle 서비스를 다시 시작하려면:
  {% raw %}```bash
  docker-compose up -d oracle
  ```{% endraw %}

이렇게 서비스 이름을 사용하면 {% raw %}`docker-compose`{% endraw %}가 자동으로 해당 서비스에 연결된 컨테이너를 찾아 명령을 수행합니다.

## 사용자 입력:
> docker-compose로 oracle-19c를 설치했는데, 환경변수로  environment:
      ORACLE_SID: "oracle"
      ORACLE_PWD: "oracle"
      ORACLE_CHARACTERSET: "KO16MSWIN949"
 이렇게 입력했어. 그런데, 접속할 때 id/pw를 oracle/oracle로 접속했더니, ORA-01017: invalid username/password; logon denied 이렇게 나오네. 뭐가 잘못됐지?

## ChatGPT 응답:
> {% raw %}`ORA-01017: invalid username/password; logon denied`{% endraw %} 오류는 일반적으로 잘못된 사용자 이름 또는 비밀번호로 인해 발생합니다. Oracle 컨테이너 설정에서 환경 변수를 사용하여 비밀번호를 설정했지만, 이 오류는 여전히 몇 가지 가능한 원인으로 인해 발생할 수 있습니다:

1. **환경 변수 설정 확인**: 환경 변수 {% raw %}`ORACLE_PWD`{% endraw %}가 올바르게 설정되었는지 확인하세요. {% raw %}`docker-compose.yml`{% endraw %} 파일에서 설정한 값이 실제로 Oracle 데이터베이스 인스턴스에 적용되었는지 확인해야 합니다.

2. **사용자 이름과 비밀번호 확인**: 일반적으로 Oracle 데이터베이스에 처음 접속할 때 사용하는 사용자 이름은 {% raw %}`sys`{% endraw %} 또는 {% raw %}`system`{% endraw %}입니다. {% raw %}`oracle`{% endraw %} 사용자는 기본적으로 존재하지 않을 수 있으므로, 기본 관리자 계정으로 접속을 시도해 보세요. 예를 들어, {% raw %}`sys as sysdba`{% endraw %} 또는 {% raw %}`system`{% endraw %}과 같이 사용자 이름을 사용합니다.

3. **비밀번호 케이스 감수성**: Oracle 11g 이상 버전부터 비밀번호는 대소문자를 구분합니다. 환경 변수에서 설정한 비밀번호가 정확한 대소문자를 사용하는지 확인하세요.

4. **데이터베이스 초기화**: Oracle 컨테이너를 처음 실행할 때 데이터베이스가 초기화되고 환경 변수의 설정이 적용되어야 합니다. 만약 컨테이너를 재시작하거나 재생성했다면, 이 과정이 올바르게 완료되었는지 확인해야 합니다.

5. **컨테이너 로그 확인**: 컨테이너 로그를 확인하여 데이터베이스가 올바르게 초기화되었는지, 환경 변수 설정이 적용되었는지 확인할 수 있습니다. 로그를 보려면 다음 명령을 사용할 수 있습니다:
   {% raw %}```bash
   docker-compose logs oracle
   ```{% endraw %}

6. **네트워크 설정**: 데이터베이스가 네트워크를 통해 올바르게 노출되고 있는지 확인하세요. {% raw %}`docker-compose.yml`{% endraw %} 파일에서 포트 매핑이 올바르게 설정되어 있는지 확인해야 합니다.

이러한 점들을 확인한 후에도 여전히 문제가 해결되지 않는다면, {% raw %}`docker-compose.yml`{% endraw %} 파일의 설정을 다시 검토하거나, 필요한 경우 Oracle 컨테이너를 재생성해야 할 수도 있습니다.

## 사용자 입력:
> docker-compose로 service에 접속할 수 있나?

## ChatGPT 응답:
> {% raw %}`docker-compose`{% endraw %}를 사용하여 서비스에 직접 접속하는 것은 직접적으로 지원되지 않습니다. {% raw %}`docker-compose`{% endraw %}는 주로 컨테이너의 구성, 생성, 시작, 정지 등을 관리하는 도구입니다. 그러나 {% raw %}`docker-compose`{% endraw %}를 사용하여 관리되는 서비스(컨테이너)에 접근하거나 상호작용하기 위한 다른 방법들이 있습니다.

1. **컨테이너 내부로 접속하기**: {% raw %}`docker-compose exec`{% endraw %} 명령을 사용하여 실행 중인 컨테이너 내부에 접속할 수 있습니다. 이를 통해 컨테이너 내부에서 명령을 실행할 수 있습니다. 예를 들어, Oracle 데이터베이스가 실행 중인 컨테이너에 접속하려면 다음과 같은 명령을 사용할 수 있습니다:

   {% raw %}```bash
   docker-compose exec [서비스 이름] bash
   ```{% endraw %}

   이 명령은 컨테이너 내부의 쉘(bash)에 접속합니다. {% raw %}`[서비스 이름]`{% endraw %}은 {% raw %}`docker-compose.yml`{% endraw %} 파일에 정의된 서비스의 이름으로 대체해야 합니다.

2. **포트 매핑을 통한 접속**: {% raw %}`docker-compose.yml`{% endraw %} 파일에서 포트 매핑을 설정하여 외부에서 컨테이너의 특정 서비스에 접근할 수 있습니다. 예를 들어, Oracle 데이터베이스 서비스에 대한 포트 매핑을 설정하면 데이터베이스 클라이언트를 사용하여 해당 포트를 통해 데이터베이스에 접속할 수 있습니다.

3. **컨테이너 로그 확인하기**: {% raw %}`docker-compose logs`{% endraw %} 명령을 사용하여 서비스의 로그를 확인할 수 있습니다. 이는 서비스의 상태나 문제 해결에 유용할 수 있습니다.

4. **컨테이너 상태 확인하기**: {% raw %}`docker-compose ps`{% endraw %} 명령을 사용하여 현재 실행 중인 컨테이너의 상태를 확인할 수 있습니다.

{% raw %}`docker-compose`{% endraw %}를 사용하여 서비스에 직접적으로 "접속"하는 것은 주로 이러한 방식으로 이루어집니다. 컨테이너 내부로의 접근, 네트워크를 통한 서비스 접근, 로그 확인 등을 통해 원하는 서비스와 상호작용할 수 있습니다.

## 사용자 입력:
> 혹시 오라클리스너가 컨테이너  안에 떠 있는지 확인할려면?

## ChatGPT 응답:
> Oracle 리스너의 상태를 확인하기 위해서는 컨테이너 내부에서 명령을 실행해야 합니다. {% raw %}`docker-compose`{% endraw %}와 {% raw %}`docker`{% endraw %} 명령어를 사용하여 이를 수행할 수 있습니다. 다음은 Oracle 리스너의 상태를 확인하는 방법입니다:

1. **컨테이너 내부로 접속하기**: 먼저, Oracle 데이터베이스가 실행 중인 컨테이너 내부에 접속해야 합니다. 이를 위해 {% raw %}`docker-compose exec`{% endraw %} 명령을 사용할 수 있습니다. {% raw %}`docker-compose.yml`{% endraw %} 파일에 정의된 Oracle 서비스의 이름을 사용해야 합니다. 예를 들어, 서비스 이름이 {% raw %}`oracle`{% endraw %}이라면 다음과 같이 입력합니다:

   {% raw %}```bash
   docker-compose exec oracle bash
   ```{% endraw %}

   이 명령은 Oracle 서비스가 실행 중인 컨테이너에 대한 쉘 접근을 제공합니다.

2. **리스너 상태 확인하기**: 컨테이너 내부에 접속한 후, Oracle 리스너의 상태를 확인하기 위해 {% raw %}`lsnrctl status`{% endraw %} 명령을 실행할 수 있습니다. 이 명령은 리스너의 현재 상태와 구성 정보를 제공합니다.

   {% raw %}```bash
   lsnrctl status
   ```{% endraw %}

이 과정은 Oracle 리스너가 정상적으로 구성되고 실행 중인지를 확인하는 데 사용됩니다. {% raw %}`lsnrctl status`{% endraw %} 명령의 출력에는 리스너의 상태, 등록된 서비스, 연결 주소 등의 중요한 정보가 포함됩니다. 만약 리스너가 실행되고 있지 않다면, {% raw %}`lsnrctl start`{% endraw %} 명령을 사용하여 리스너를 시작할 수 있습니다.

Oracle 데이터베이스와 리스너의 정확한 구성은 설치된 Oracle 버전과 환경에 따라 다를 수 있으므로, 위의 명령들은 일반적인 경우에 대한 지침입니다.

## 사용자 입력:
> Copyright (c) 1991, 2020, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
Start Date                07-DEC-2023 06:30:26
Uptime                    0 days 0 hr. 15 min. 14 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /opt/oracle/product/19c/dbhome_1/network/admin/listener.ora
Listener Log File         /opt/oracle/diag/tnslsnr/65a141497761/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=1521)))
Services Summary...
Service "ORACLE" has 1 instance(s).
  Instance "ORACLE", status READY, has 1 handler(s) for this service...
Service "ORACLEXDB" has 1 instance(s).
  Instance "ORACLE", status READY, has 1 handler(s) for this service...
The command completed successfully
 이게 결과인데.

## ChatGPT 응답:
> 제공하신 Oracle 리스너의 상태 정보를 살펴보면, 리스너가 정상적으로 작동하고 있음을 알 수 있습니다. 여기서 중요한 몇 가지 포인트를 확인할 수 있습니다:

1. **리스너 시작 일시**: 리스너는 2023년 12월 7일 06시 30분 26초에 시작되었습니다.

2. **업타임**: 리스너는 이미 약 15분 이상 정상적으로 작동하고 있습니다.

3. **리스닝 엔드포인트 요약**:
   - **IPC**: Inter-Process Communication을 위한 엔드포인트가 있습니다. {% raw %}`KEY=EXTPROC1`{% endraw %}을 통해 내부 프로세스 간 통신이 가능합니다.
   - **TCP**: TCP 프로토콜을 사용하여 호스트의 모든 IP({% raw %}`0.0.0.0`{% endraw %})에서 1521 포트를 통해 외부 접속을 받을 수 있습니다. 이는 원격 데이터베이스 접속을 위한 표준 구성입니다.

4. **서비스 요약**:
   - **Service "ORACLE"**: 'ORACLE' 서비스가 실행 중이며, 상태는 'READY'입니다. 이는 해당 서비스가 정상적으로 작동하고 연결을 받을 준비가 되었음을 의미합니다.
   - **Service "ORACLEXDB"**: 'ORACLEXDB' 서비스 역시 실행 중이며, 상태는 'READY'입니다.

이 정보를 바탕으로, Oracle 리스너가 정상적으로 구성되고 작동하고 있음을 확인할 수 있습니다. 리스너가 정상적으로 작동하고 있음에도 불구하고 데이터베이스 접속에 문제가 발생한다면, 접속 시 사용하는 클라이언트 설정, 데이터베이스 사용자 이름 및 비밀번호, 네트워크 설정 등을 다시 확인해야 합니다.

데이터베이스 접속 오류가 계속된다면, 데이터베이스 사용자 계정의 생성 및 권한 설정, 데이터베이스 URL 및 접속 포트 설정 등을 다시 한 번 확인해 보시기 바랍니다.

## 사용자 입력:
> 오라클 컨테이너내부에서 sqlpulus로 접속하는 명령알려줘. 

## ChatGPT 응답:
> Oracle 컨테이너 내부에서 SQL*Plus를 사용하여 데이터베이스에 접속하려면, 먼저 컨테이너 내부의 쉘에 접근해야 합니다. 그 후 SQL*Plus를 사용하여 데이터베이스에 로그인할 수 있습니다. 다음 단계를 따라 해보세요.

1. **컨테이너 내부로 접속하기**:
   먼저, 실행 중인 Oracle 컨테이너의 쉘에 접근합니다. 이를 위해 {% raw %}`docker-compose exec`{% endraw %} 명령을 사용합니다. 여기서 {% raw %}`[서비스 이름]`{% endraw %}은 {% raw %}`docker-compose.yml`{% endraw %}에 정의된 Oracle 서비스의 이름으로 대체해야 합니다.
   
   {% raw %}```bash
   docker-compose exec [서비스 이름] bash
   ```{% endraw %}

   예를 들어, 서비스 이름이 {% raw %}`oracle`{% endraw %}이라면 명령은 다음과 같습니다:

   {% raw %}```bash
   docker-compose exec oracle bash
   ```{% endraw %}

2. **SQL*Plus를 사용하여 데이터베이스에 접속하기**:
   컨테이너의 쉘에 접근한 후, SQL*Plus를 사용하여 데이터베이스에 로그인합니다. 일반적으로 데이터베이스 사용자 이름과 비밀번호를 사용합니다. 예를 들어, 사용자 이름이 {% raw %}`system`{% endraw %}이고 비밀번호가 {% raw %}`oracle`{% endraw %}이라면 다음과 같이 입력합니다:

   {% raw %}```bash
   sqlplus system/oracle
   ```{% endraw %}

   데이터베이스 관리자 계정으로 접속하려면, {% raw %}`sysdba`{% endraw %} 권한을 사용할 수 있습니다:

   {% raw %}```bash
   sqlplus sys/oracle as sysdba
   ```{% endraw %}

   여기서 {% raw %}`oracle`{% endraw %}은 설정한 데이터베이스 비밀번호로 대체해야 합니다.

이러한 방법으로 Oracle 컨테이너 내부에서 SQL*Plus를 통해 데이터베이스에 접속할 수 있습니다. 컨테이너 내부의 환경은 설치된 Oracle 데이터베이스 버전에 따라 약간씩 다를 수 있으므로, 해당 환경에 맞게 명령을 조정해야 할 수도 있습니다.

