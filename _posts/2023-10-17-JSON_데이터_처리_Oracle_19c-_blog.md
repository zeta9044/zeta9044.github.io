---
layout: post
title: "JSON 데이터 처리 Oracle 19c+"
date: 2023-10-17
tags: [데이터, JSON, Oracle, 처리]
categories: blog
---

## 사용자 입력:
> oracle19c이후 버전에서 json데이터를 처리하는 컬럼이 따로 있나?

## ChatGPT 응답:
> 네, Oracle Database 19c부터는 JSON을 지원하기 위한 명시적인 데이터 타입인 `JSON` 데이터 타입이 추가되었습니다. Oracle Database 21c에서는 이 데이터 타입이 소개되었는데, 이전 버전인 Oracle 12c와 Oracle 19c에서는 JSON 데이터를 `VARCHAR2`, `CLOB` 또는 `BLOB` 컬럼에 저장했습니다. 그러나 `JSON` 타입을 사용하면 JSON 데이터를 저장, 검색 및 조작하기 위한 기능과 최적화를 이용할 수 있습니다.

Oracle 21c에서 `JSON` 데이터 타입을 사용하면:
- JSON 문서의 유효성을 자동으로 검증할 수 있습니다.
- 표준 SQL 및 JSON 함수와 조건을 사용하여 JSON 데이터에 질의하고 조작할 수 있습니다.
- JSON 데이터의 저장 및 검색 성능이 향상될 수 있습니다.

다음은 `JSON` 데이터 타입을 사용하여 테이블을 생성하는 예입니다:

```sql
CREATE TABLE my_json_table (
  id NUMBER,
  json_data JSON
);
```

이렇게 `JSON` 데이터 타입을 사용하면 JSON 데이터를 보다 효과적으로 다룰 수 있습니다.

