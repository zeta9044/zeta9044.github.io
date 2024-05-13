---
layout: post
title: "CTAS는 DDL인가?"
tags: []
---

## 사용자 입력:
> 그러면  create table as select 문은 DDL인가?

## ChatGPT 응답:
> {% raw %}`CREATE TABLE AS SELECT`{% endraw %} 문 (종종 CTAS로 약칭)은 데이터베이스에서 새로운 테이블을 만들면서 동시에 다른 테이블로부터 데이터를 복사하는 데 사용됩니다.

이 문은 두 부분으로 구성됩니다:

1. {% raw %}`CREATE TABLE`{% endraw %} - 이 부분은 DDL(Data Definition Language)입니다. DDL은 데이터베이스의 구조를 정의하거나 변경하는 데 사용되는 명령어를 의미합니다.
2. {% raw %}`SELECT`{% endraw %} - 이 부분은 DML(Data Manipulation Language)입니다. DML은 데이터베이스 내의 데이터를 쿼리, 삽입, 수정, 삭제하는 데 사용되는 명령어를 의미합니다.

그러므로 {% raw %}`CREATE TABLE AS SELECT`{% endraw %} 문 자체는 DDL과 DML의 조합으로 볼 수 있습니다. 하지만 전체 문장의 주요 작업은 새로운 테이블을 정의하는 것이므로, 주로 DDL로 분류할 수 있습니다.

## 사용자 입력:
> 그러면 오라클에서는 create table as select에서 select문에는 ? 바인딩 변수를 쓸 수 없는 거야?

## ChatGPT 응답:
> 네, 맞습니다. 오라클의 SQL에서 {% raw %}`CREATE TABLE AS SELECT`{% endraw %} (CTAS) 문법을 사용할 때는 바인딩 변수({% raw %}`?`{% endraw %})를 직접 사용할 수 없습니다. 바인딩 변수는 주로 프로그래밍 언어와 연동할 때 사용되는데, SQL*Plus나 다른 직접적인 SQL 수행 도구에서는 CTAS에 바인딩 변수를 사용할 수 없습니다.

예를 들어, 다음과 같은 문장은 허용되지 않습니다:
{% raw %}```sql
CREATE TABLE new_table AS SELECT * FROM old_table WHERE column_name = ?;
```{% endraw %}

바인딩 변수를 사용하고자 한다면, 해당 로직을 프로그래밍 언어나 스크립트에서 구현하여 그 곳에서 바인딩 값을 할당하고, 동적 SQL을 생성하여 실행해야 합니다.

