---
layout: post
title: "조크 컨테이너 tablespace 크기 설정"
date: 2023-12-01
tags: [설정, 크기, tablespace, 컨테이너, 조]
categories: blog
---

## 사용자 입력:
> version: '3.1'

services:
  oracle-19c:
    image: doctorkirk/oracle-19c
    ports:
      - "1521:1521"
    environment:
      ORACLE_SID: "oracle"
      ORACLE_PWD: "oracle"
      ORACLE_CHARACTERSET: "KO16MSWIN949"
    volumes:
      - /home/lia/oracle-19c/oradata:/opt/oracle/oradata
      - /home/lia/oracle-scripts:/home/lia/scripts
    restart: always
위 파일에서 tablespace 크기를 100G정도 세팅하는 걸 할 수 있나?

## ChatGPT 응답:
> Oracle 데이터베이스의 `tablespace` 크기를 설정하려면, Docker 컨테이너 설정 파일(여기서는 Docker Compose 파일)을 직접 수정하는 것만으로는 충분하지 않습니다. `tablespace` 크기는 데이터베이스 내부에서 설정되어야 합니다. 그러나, Docker 컨테이너에 필요한 볼륨 크기를 조정하여 이를 수용할 수 있는 충분한 공간을 제공할 수 있습니다.

여기서 언급된 Docker Compose 파일은 Oracle 19c 데이터베이스를 위한 것이며, 볼륨 매핑을 포함하고 있습니다. 이 볼륨 매핑은 호스트 시스템의 디렉토리를 컨테이너 내부의 디렉토리에 연결합니다. 이 경우, `/home/lia/oracle-19c/oradata`가 Oracle 데이터 파일을 저장하는 데 사용됩니다.

`tablespace`의 크기를 100GB로 설정하려면 다음 단계를 따라야 합니다:

1. **볼륨 공간 확인**: 호스트 시스템에서 `/home/lia/oracle-19c/oradata` 경로에 최소 100GB의 여유 공간이 있는지 확인하세요.

2. **Oracle 데이터베이스에 접속**: Docker 컨테이너가 실행되고 있고 Oracle 데이터베이스에 접근할 수 있는지 확인하세요.

3. **Tablespace 생성 또는 수정**:
   - 이미 존재하는 `tablespace`의 크기를 변경하려면, `ALTER TABLESPACE` 명령어를 사용하세요.
   - 새로운 `tablespace`를 만들려면, `CREATE TABLESPACE` 명령어를 사용하세요.

예를 들어, 새로운 `tablespace`를 만들고 크기를 100GB로 설정하려면 다음과 같은 SQL 명령어를 사용할 수 있습니다:

```sql
CREATE TABLESPACE mytablespace 
DATAFILE '/opt/oracle/oradata/mytablespace.dbf' 
SIZE 100G 
AUTOEXTEND ON;
```

이 명령어는 `mytablespace`라는 이름의 새 `tablespace`를 생성하고, 초기 크기를 100GB로 설정합니다. `AUTOEXTEND ON` 옵션은 필요에 따라 파일 크기를 자동으로 확장할 수 있도록 설정합니다.

**참고**: Oracle 데이터베이스 관리와 관련된 변경사항은 데이터 손실이나 서비스 중단을 초래할 수 있으므로, 이러한 변경을 진행하기 전에 반드시 데이터 백업 및 테스트 환경에서의 사전 테스트가 권장됩니다.

