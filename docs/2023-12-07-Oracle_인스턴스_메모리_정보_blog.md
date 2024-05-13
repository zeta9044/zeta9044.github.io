---
layout: post
title: "Oracle 인스턴스 메모리 정보"
date: 2023-12-07
tags: [정보, Oracle, 인스턴스, 메모리]
categories: blog
---

## 사용자 입력:
> oracle  | SQL> ORACLE instance started.
oracle  |
oracle  | Total System Global Area 5033162888 bytes
oracle  | Fixed Size                8906888 bytes
oracle  | Variable Size           989855744 bytes
oracle  | Database Buffers       4026531840 bytes
oracle  | Redo Buffers              7868416 bytes
oracle  | Database mounted.
oracle  | Database opened.
인간이 쉽게 읽을 수 있는 단위로 알려줘.

## ChatGPT 응답:
> 이 Oracle 데이터베이스 인스턴스의 정보를 보다 쉽게 읽을 수 있는 단위로 변환해 보겠습니다.

1. **Total System Global Area:** 5033162888 bytes
   - 약 5.03 기가바이트 (GB)

2. **Fixed Size:** 8906888 bytes
   - 약 8.5 메가바이트 (MB)

3. **Variable Size:** 989855744 bytes
   - 약 944 메가바이트 (MB)

4. **Database Buffers:** 4026531840 bytes
   - 약 3.75 기가바이트 (GB)

5. **Redo Buffers:** 7868416 bytes
   - 약 7.5 메가바이트 (MB)

이 정보는 Oracle 데이터베이스 인스턴스의 메모리 할당 및 구성을 보여줍니다. "Total System Global Area"는 데이터베이스의 전체 메모리 영역 크기를 나타내며, 나머지 항목들은 이 영역 내에서 각각 할당된 메모리의 크기를 나타냅니다.

## 사용자 입력:
> MB의 천배가 GB인가?

## ChatGPT 응답:
> 네, 맞습니다. 1 기가바이트(GB)는 1,000 메가바이트(MB)와 같습니다. 이는 디지털 저장 공간의 측정 단위에 대한 일반적인 규칙입니다. 

이렇게 디지털 저장공간의 단위는 다음과 같이 증가합니다:
- 1 메가바이트 (MB) = 1,000 킬로바이트 (KB)
- 1 기가바이트 (GB) = 1,000 메가바이트 (MB)
- 1 테라바이트 (TB) = 1,000 기가바이트 (GB)

그러나 컴퓨터 과학에서는 때때로 1 기가바이트를 1,024 메가바이트로 계산하기도 합니다. 이는 2의 거듭제곱을 사용하는 바이너리(이진) 시스템에 기반을 둔 것입니다. 그러나 일반적인 상업적 및 소비자용 제품에서는 1 GB = 1,000 MB라는 규칙을 따릅니다.

## 사용자 입력:
> docker-compose 로 oracle시작할 때, 테이블스페이스를 추가 정의할 수 없나?

## ChatGPT 응답:
> `docker-compose`를 사용하여 Oracle 데이터베이스를 시작할 때, 초기 테이블스페이스 설정을 직접 정의하는 것은 일반적으로 `docker-compose.yml` 파일 자체에서 직접 수행되지 않습니다. 그러나 여기에는 몇 가지 방법이 있습니다:

1. **사전 구성된 이미지 사용:** Oracle 데이터베이스 이미지를 빌드할 때 필요한 테이블스페이스 설정을 포함시킬 수 있습니다. 이렇게 하면 `docker-compose`를 사용하여 컨테이너를 시작할 때 이미 테이블스페이스가 설정된 상태로 시작됩니다.

2. **초기화 스크립트 사용:** Oracle 이미지는 보통 초기화 스크립트를 실행할 수 있는 기능을 제공합니다. 이 스크립트는 컨테이너가 처음 시작될 때 실행되며, 여기에 테이블스페이스를 생성하는 SQL 명령문을 포함시킬 수 있습니다. 이러한 스크립트는 `docker-compose.yml`에서 볼륨 마운트를 통해 제공될 수 있습니다.

3. **사용자 정의 Dockerfile:** Oracle 데이터베이스를 위한 사용자 정의 `Dockerfile`을 만들어 특정 설정(테이블스페이스 포함)을 포함시킬 수 있습니다. 이 `Dockerfile`은 데이터베이스를 초기화하고 필요한 설정을 구성하는 스크립트를 실행할 수 있습니다.

예를 들어, 초기화 스크립트를 사용하는 방법은 다음과 같습니다:

```yaml
version: '3'
services:
  oracle:
    image: oracle/database:19.3.0-ee
    volumes:
      - ./init.sql:/opt/oracle/scripts/startup/init.sql
    ports:
      - "1521:1521"
```

여기서 `init.sql`은 데이터베이스가 시작될 때 실행될 SQL 스크립트를 포함하며, 이 스크립트 내에서 필요한 테이블스페이스를 정의할 수 있습니다.

이 방법들은 모두 Oracle 데이터베이스 환경을 커스터마이징하는 유연성을 제공합니다. 그러나 특정 Oracle 이미지의 기능과 제한 사항에 대한 문서를 참조하는 것이 중요합니다.

## 사용자 입력:
> 그러면, 테이블 스페이스를 100G 추가 정의하는 스크립트 만들어줘.

## ChatGPT 응답:
> 테이블 스페이스를 100GB로 설정하는 SQL 스크립트를 작성하겠습니다. 이 스크립트는 Oracle 데이터베이스에서 실행될 수 있으며, 새로운 테이블 스페이스를 생성합니다. 

아래 스크립트 예시에서, 테이블 스페이스의 이름을 'MYTABLESPACE'로 설정했습니다. 데이터 파일의 경로는 Oracle 데이터베이스 서버에서 접근 가능한 경로로 설정해야 합니다. 데이터 파일의 이름을 'mytablespace.dbf'로 지정했습니다. 이는 필요에 따라 변경할 수 있습니다.

```sql
CREATE TABLESPACE MYTABLESPACE 
DATAFILE '/path/to/your/oracle/data/mytablespace.dbf' 
SIZE 100G 
AUTOEXTEND ON 
NEXT 10M 
MAXSIZE UNLIMITED 
LOGGING
EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO;
```

이 스크립트는 다음을 수행합니다:

- 새 테이블 스페이스 `MYTABLESPACE`를 생성합니다.
- 데이터 파일 경로를 설정합니다. (`/path/to/your/oracle/data/mytablespace.dbf`는 예시 경로입니다. 실제 경로로 변경해야 합니다.)
- 초기 크기를 100GB로 설정합니다.
- `AUTOEXTEND ON` 옵션을 사용하여 데이터 파일이 자동으로 확장되도록 합니다.
- 파일 확장 시 증가하는 크기를 10MB로 설정합니다.
- `MAXSIZE UNLIMITED`를 사용하여 데이터 파일의 최대 크기를 제한하지 않습니다.
- 로깅을 활성화합니다.
- 로컬로 관리되는 익스텐트와 자동 세그먼트 공간 관리를 사용합니다.

이 스크립트를 사용하기 전에, 경로와 테이블 스페이스의 이름을 실제 환경에 맞게 조정해야 합니다. 또한, 이 스크립트는 관리자 권한이 있는 사용자에 의해 실행되어야 합니다.

## 사용자 입력:
> 위 테이블스페이스를 사용하는 유저를 지정해야 되지?

## ChatGPT 응답:
> 네, 특정 테이블 스페이스를 사용하도록 사용자를 지정하려면 해당 사용자에게 해당 테이블 스페이스를 기본 테이블 스페이스로 설정하고, 필요한 권한을 부여해야 합니다. 다음은 이를 위한 SQL 스크립트의 예시입니다.

먼저, 새 사용자를 생성하거나 기존 사용자를 사용합니다. 사용자에게는 해당 테이블 스페이스에 객체를 생성할 수 있는 권한이 필요합니다. 예를 들어, 사용자 이름을 `newuser`라고 하고, 비밀번호를 `password`라고 가정하겠습니다. 또한, 이 사용자에게 `MYTABLESPACE`를 기본 테이블 스페이스로 설정하겠습니다.

```sql
-- 사용자 생성 (이미 존재하는 경우는 이 단계를 생략)
CREATE USER newuser IDENTIFIED BY password;

-- 사용자에게 필요한 시스템 권한 부여
GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW, CREATE PROCEDURE, CREATE SEQUENCE, CREATE TRIGGER TO newuser;

-- MYTABLESPACE를 기본 테이블 스페이스로 설정
ALTER USER newuser DEFAULT TABLESPACE MYTABLESPACE;

-- 필요한 경우 추가적인 권한 부여
GRANT UNLIMITED TABLESPACE TO newuser;
```

이 스크립트는 다음과 같은 작업을 수행합니다:

1. `newuser`라는 이름의 사용자를 생성합니다.
2. `newuser`에게 세션을 생성하고, 테이블, 뷰, 프로시저, 시퀀스, 트리거를 생성할 수 있는 권한을 부여합니다.
3. `MYTABLESPACE`를 `newuser`의 기본 테이블 스페이스로 지정합니다.
4. `newuser`에게 무제한 테이블 스페이스 사용 권한을 부여합니다.

사용자 이름, 비밀번호, 권한은 실제 사용 환경과 요구 사항에 맞게 조정해야 합니다. 또한, 이러한 변경 사항을 데이터베이스에 적용하기 위해서는 관리자 권한이 필요합니다.

## 사용자 입력:
> 패스워드는 대소문자를 구분하나?

## ChatGPT 응답:
> Oracle 데이터베이스에서 패스워드는 기본적으로 대소문자를 구분합니다. 이는 Oracle 11g 릴리스 2부터 적용된 보안 기능의 일부입니다. 이전 버전에서는 대소문자 구분 없이 패스워드를 처리했지만, 보안을 강화하기 위해 이후 버전에서는 대소문자를 구분하게 되었습니다.

따라서, 사용자 계정의 패스워드를 설정하거나 변경할 때, 입력한 대소문자가 정확히 일치해야 합니다. 예를 들어, 패스워드를 'Password123'로 설정한 경우, 로그인할 때 'password123', 'PASSWORD123', 'Password123' 등으로 대소문자가 다르게 입력되면 인증에 실패합니다.

패스워드의 대소문자 구분 설정은 데이터베이스 매개변수 `SEC_CASE_SENSITIVE_LOGON`을 통해 관리할 수 있습니다. 이 매개변수를 `FALSE`로 설정하면 대소문자 구분 없이 패스워드를 처리할 수 있지만, 이는 보안 관점에서 권장되지 않습니다.

## 사용자 입력:
> CREATE USER newuser IDENTIFIED BY password;과 CREATE USER newuser IDENTIFIED BY passWord;는 암호가 다른거지?

## ChatGPT 응답:
> 네, 맞습니다. `CREATE USER newuser IDENTIFIED BY password;`와 `CREATE USER newuser IDENTIFIED BY passWord;`는 서로 다른 암호를 지정하는 것으로 간주됩니다. Oracle 데이터베이스는 패스워드에 대해 대소문자를 구분합니다. 따라서, 첫 번째 명령어에서 사용된 'password'와 두 번째 명령어에서 사용된 'passWord'는 서로 다른 패스워드로 인식됩니다.

이는 Oracle 11g 릴리스 2부터 도입된 보안 기능의 일부로, 데이터베이스의 보안을 강화하기 위해 패스워드의 대소문자를 구분하게 되었습니다. 따라서, 사용자 계정을 생성하거나 패스워드를 변경할 때는 대소문자를 정확히 고려해야 합니다.

## 사용자 입력:
> sqlplus에서 특정디렉토리의 스크립트를 실행하는 방법은?

## ChatGPT 응답:
> SQL*Plus에서 특정 디렉토리에 있는 SQL 스크립트를 실행하려면, 스크립트 파일의 경로를 지정하고 `@` 또는 `START` 명령을 사용할 수 있습니다. 이 방법은 SQL*Plus 세션에서 직접 SQL 명령을 입력하거나 SQL 스크립트 파일을 실행할 때 모두 사용됩니다.

예를 들어, `/path/to/script.sql` 경로에 있는 스크립트 파일을 실행하려면 다음과 같이 할 수 있습니다:

```
SQL> @/path/to/script.sql
```

또는:

```
SQL> START /path/to/script.sql
```

여기서 `/path/to/script.sql`는 스크립트 파일의 전체 경로입니다. 이 경로는 실제 파일 시스템의 구조에 따라 달라집니다. 파일 경로는 절대 경로일 수도 있고, SQL*Plus를 실행하는 현재 디렉토리에 대한 상대 경로일 수도 있습니다.

스크립트 파일 내에서 다른 스크립트 파일을 호출할 때도 동일한 문법(`@` 또는 `START`)을 사용할 수 있습니다.

SQL*Plus를 사용할 때는 파일 경로와 스크립트 이름이 정확히 일치하는지 확인하는 것이 중요합니다. 또한, 필요한 권한이 있는지도 확인해야 합니다. 예를 들어, 스크립트 파일을 읽을 수 있는 권한이 있어야 합니다.

