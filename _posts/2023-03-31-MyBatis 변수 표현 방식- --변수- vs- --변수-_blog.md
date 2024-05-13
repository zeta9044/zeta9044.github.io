---
layout: post
title: "MyBatis 변수 표현 방식: ${변수} vs. #{변수}"
date: 2023-03-31
tags: [표현, MyBatis, vs, 변수, 방식]
categories: blog
---

## 사용자 입력:
> mybatis에서 ${변수}와 #{변수} 의 차이는?

## ChatGPT 응답:
> MyBatis에서 `${변수}`와 `#{변수}`는 서로 다른 변수 표현 방식입니다.

`${변수}`는 SQL 문자열을 직접 대입하는 방식으로 변수를 처리합니다. 이는 문자열 치환 방식으로, 변수를 문자열로 변환한 값을 쿼리에 대입합니다. 이 방식은 쿼리 구성이 유연하고, 동적인 쿼리를 작성하기 쉬워지는 장점이 있지만, SQL 삽입 공격(SQL Injection)과 같은 보안 문제가 발생할 가능성이 있습니다.

`#{변수}`는 SQL 쿼리 파라미터를 바인딩하는 방식으로 변수를 처리합니다. 이는 PrepareStatement의 placeholder와 유사하며, 자동으로 쿼리 파라미터를 인용 부호로 묶어줍니다. 이 방식은 보안 문제를 예방할 수 있으며, 캐시 활용이 가능하다는 장점이 있습니다.

따라서, SQL Injection 보안 문제와 캐시 사용 등의 이유로, MyBatis에서는 `#{변수}`를 사용하는 것이 권장됩니다.

## 사용자 입력:
> 오라클19c에서 insert문장을 넣는데, pk가 중복되면 update로 바꾸는 sql 알려줘

## ChatGPT 응답:
> Oracle 19c에서 Primary Key 중복 시 Insert문장을 Update로 변경하는 SQL은 다음과 같이 작성할 수 있습니다.

```sql
MERGE INTO [테이블명] t
USING (SELECT [값1] AS [PK칼럼명] FROM DUAL) s
ON (t.[PK칼럼명] = s.[PK칼럼명])
WHEN MATCHED THEN
  UPDATE SET t.[컬럼1] = [값1], t.[컬럼2] = [값2], ...
WHEN NOT MATCHED THEN
  INSERT (t.[PK칼럼명], t.[컬럼1], t.[컬럼2], ...)
  VALUES ([값1], [값2], ...);
```

위의 SQL 문장에서 MERGE INTO 절은 대상 테이블과 임시 테이블(DUAL)을 병합합니다. USING 절은 임시 테이블에서 가져온 값으로 대상 테이블의 해당 PK 칼럼과 비교합니다. WHEN MATCHED THEN 절은 대상 테이블에서 해당 PK 칼럼의 값이 일치하는 경우, UPDATE문을 실행합니다. WHEN NOT MATCHED THEN 절은 대상 테이블에서 해당 PK 칼럼의 값이 일치하지 않는 경우, INSERT문을 실행합니다.

예를 들어, `EMPLOYEE` 테이블의 `EMPLOYEE_ID` 컬럼이 Primary Key이고, 중복된 값이 입력될 경우 `EMPLOYEE_NAME`, `EMPLOYEE_DEPT` 등 다른 컬럼 값은 새로운 값으로 변경하고자 할 경우, 다음과 같은 SQL 문장을 사용할 수 있습니다.

```sql
MERGE INTO EMPLOYEE t
USING (SELECT 1001 AS EMPLOYEE_ID, 'John' AS EMPLOYEE_NAME, 'IT' AS EMPLOYEE_DEPT FROM DUAL) s
ON (t.EMPLOYEE_ID = s.EMPLOYEE_ID)
WHEN MATCHED THEN
  UPDATE SET t.EMPLOYEE_NAME = s.EMPLOYEE_NAME, t.EMPLOYEE_DEPT = s.EMPLOYEE_DEPT
WHEN NOT MATCHED THEN
  INSERT (t.EMPLOYEE_ID, t.EMPLOYEE_NAME, t.EMPLOYEE_DEPT)
  VALUES (s.EMPLOYEE_ID, s.EMPLOYEE_NAME, s.EMPLOYEE_DEPT);
```

위의 SQL 문장은 `EMPLOYEE` 테이블에서 `EMPLOYEE_ID`가 1001인 레코드가 이미 존재하는 경우, 해당 레코드의 `EMPLOYEE_NAME`과 `EMPLOYEE_DEPT` 값을 변경하고, 존재하지 않는 경우 새로운 레코드를 추가합니다.

