---
layout: post
title: "부여된 Oracle 권한 LIAUSR"
date: 2023-12-08
tags: [Oracle, LIAUSR, 권한, 부여]
categories: blog
---

## 사용자 입력:
> 오라클에서 export 권한을 LIAUSR에게 주고 싶어.

## ChatGPT 응답:
> Oracle에서 사용자에게 export 권한을 부여하는 것은 DBA(데이터베이스 관리자)가 수행할 수 있는 작업입니다. `LIAUSR`라는 사용자에게 export 권한을 부여하려면, Oracle 데이터베이스의 SQL*Plus 또는 다른 SQL 인터페이스를 사용하여 권한을 부여할 수 있습니다. 아래의 단계를 따라해 보세요:

1. **SQL*Plus에 로그인:** 먼저 Oracle 데이터베이스에 DBA 권한을 가진 계정으로 로그인해야 합니다. 일반적으로 `sys` 계정을 사용합니다.

   ```sql
   sqlplus sys as sysdba
   ```

2. **Export 권한 부여:** `LIAUSR` 사용자에게 필요한 export 권한을 부여합니다. Oracle에서 데이터를 export하는 데 필요한 권한은 `EXP_FULL_DATABASE` 또는 필요에 따라 더 제한적인 권한일 수 있습니다.

   ```sql
   GRANT EXP_FULL_DATABASE TO LIAUSR;
   ```

   또는 특정 테이블에 대한 export 권한만 부여하고 싶다면, 해당 테이블에 대한 `SELECT` 권한을 부여할 수도 있습니다.

   ```sql
   GRANT SELECT ON [테이블 이름] TO LIAUSR;
   ```

3. **변경 사항 확인:** 권한 부여가 제대로 이루어졌는지 확인하기 위해 `LIAUSR` 계정으로 로그인하여 권한을 확인해볼 수 있습니다.

   ```sql
   SELECT * FROM USER_SYS_PRIVS;
   ```

4. **로그아웃:** 작업이 완료되면 SQL*Plus에서 로그아웃합니다.

   ```sql
   EXIT;
   ```

이러한 단계를 통해 `LIAUSR` 사용자에게 Oracle 데이터베이스에서 데이터를 export하는 권한을 부여할 수 있습니다. 작업을 수행하기 전에 해당 데이터베이스의 보안 정책과 권한 관리 방침을 확인하는 것이 중요합니다.

