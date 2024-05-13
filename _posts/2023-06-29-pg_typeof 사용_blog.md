---
layout: post
title: "pg_typeof 사용"
date: 2023-06-29
tags: [사용]
categories: blog
---

## 사용자 입력:
> postgresql 에서 컬럼의 타입을 알 수 있는 내장함수는?

## ChatGPT 응답:
> PostgreSQL에서 컬럼의 타입을 알 수 있는 내장 함수는 `pg_typeof`입니다. 이 함수는 주어진 표현식의 데이터 타입을 반환합니다. 다음은 `pg_typeof` 함수의 사용 예입니다:

```sql
SELECT pg_typeof(column_name) FROM table_name;
```

위의 쿼리에서 `column_name`은 확인하려는 컬럼의 이름이고, `table_name`은 해당 컬럼이 있는 테이블의 이름입니다. 이 쿼리를 실행하면 컬럼의 데이터 타입이 결과로 반환됩니다.

