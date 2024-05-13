---
layout: post
title: "pg_typeof 사용"
tags: [사용]
---

## 사용자 입력:
> postgresql 에서 컬럼의 타입을 알 수 있는 내장함수는?

## ChatGPT 응답:
> PostgreSQL에서 컬럼의 타입을 알 수 있는 내장 함수는 {% raw %}`pg_typeof`{% endraw %}입니다. 이 함수는 주어진 표현식의 데이터 타입을 반환합니다. 다음은 {% raw %}`pg_typeof`{% endraw %} 함수의 사용 예입니다:

{% raw %}```sql
SELECT pg_typeof(column_name) FROM table_name;
```{% endraw %}

위의 쿼리에서 {% raw %}`column_name`{% endraw %}은 확인하려는 컬럼의 이름이고, {% raw %}`table_name`{% endraw %}은 해당 컬럼이 있는 테이블의 이름입니다. 이 쿼리를 실행하면 컬럼의 데이터 타입이 결과로 반환됩니다.

