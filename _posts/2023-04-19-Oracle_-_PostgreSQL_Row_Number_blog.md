---
layout: post
title: "Oracle & PostgreSQL Row Number"
tags: [PostgreSQL, Row, Number, Oracle]
---

## 사용자 입력:
> oracle과 postgresql 양쪽에서 쓸 수 있는 rownumber 매기는 쿼리 예제 알려줘

## ChatGPT 응답:
> Oracle과 PostgreSQL 모두 ROW_NUMBER() 함수를 사용하여 행 번호를 부여할 수 있습니다.

Oracle에서의 예시:

{% raw %}```sql
SELECT ROW_NUMBER() OVER (ORDER BY column_name) as row_num, column_name
FROM table_name;
```{% endraw %}

위의 쿼리에서 ROW_NUMBER() 함수는 ORDER BY 절에 지정된 열(column_name)에 따라 행 번호를 부여합니다.

PostgreSQL에서의 예시:

{% raw %}```sql
SELECT ROW_NUMBER() OVER (ORDER BY column_name) as row_num, column_name
FROM table_name;
```{% endraw %}

Oracle과 PostgreSQL에서 모두 유사한 문법으로 ROW_NUMBER() 함수를 사용할 수 있습니다. 다만, ORDER BY 절에서 정렬 순서를 지정하는 방식이 차이가 있을 수 있으니 이 점에 주의하시면 됩니다.

