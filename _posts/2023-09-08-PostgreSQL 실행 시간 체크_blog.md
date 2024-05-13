---
layout: post
title: "PostgreSQL 실행 시간 체크"
date: 2023-09-08
tags: [PostgreSQL, 체크, 시간, 실행]
categories: blog
---

## 사용자 입력:
> postgresql에서 어떤 sql스크립트의 수행시간을 체크하는 쿼리로직을 알려줘

## ChatGPT 응답:
> PostgreSQL에서 SQL 스크립트의 수행 시간을 체크하려면 `EXPLAIN ANALYZE`를 사용할 수 있습니다. `EXPLAIN ANALYZE`는 주어진 쿼리의 실행 계획을 보여주고, 실제로 쿼리를 실행한 후 실행 시간과 다른 유용한 통계를 제공합니다.

예를 들어, `my_table`에서 모든 행을 선택하는 쿼리의 수행 시간을 확인하려면 다음과 같이 작성할 수 있습니다:

```sql
EXPLAIN ANALYZE SELECT * FROM my_table;
```

이 쿼리의 결과로 실행 계획과 쿼리의 실행 시간이 표시됩니다.

단, `EXPLAIN ANALYZE`는 실제로 쿼리를 실행하기 때문에 데이터베이스에 영향을 줄 수 있습니다. 따라서 프로덕션 환경에서는 주의해서 사용해야 합니다.

다른 방법으로, PostgreSQL에서 제공하는 `pg_stat_statements` 모듈을 사용하여 쿼리의 실행 통계를 모니터링할 수도 있습니다. 이 모듈은 실행된 각 쿼리의 통계 정보를 수집하므로, 여러 번 실행된 쿼리의 평균 실행 시간, 최대 실행 시간 등의 정보를 얻을 수 있습니다.

