---
layout: post
title: "'PostgreSQL 음수 부분문자열 오류'"
tags: [PostgreSQL, 부분, 문자열, 음수, 오류]
---

## 사용자 입력:
> org.postgresql.util.PSQLExeption: ERROR: negative substring length not allowed 에 대해 설명해줘.

## ChatGPT 응답:
> {% raw %}`org.postgresql.util.PSQLException: ERROR: negative substring length not allowed`{% endraw %}는 PostgreSQL 데이터베이스에서 발생하는 예외입니다.

이 오류는 일반적으로 SQL 쿼리에서 {% raw %}`SUBSTRING`{% endraw %}, {% raw %}`SUBSTR`{% endraw %}, {% raw %}`LEFT`{% endraw %}, {% raw %}`RIGHT`{% endraw %} 등의 문자열 함수를 사용할 때 문자열 길이 또는 시작/끝 위치를 음수로 지정하는 경우에 발생합니다. PostgreSQL은 이러한 음수 인덱스를 허용하지 않으므로, 이런 식으로 쿼리를 작성하면 위와 같은 오류가 발생합니다.

예를 들어, 다음과 같은 SQL 쿼리에서 이 오류가 발생할 수 있습니다:

{% raw %}```sql
SELECT SUBSTRING('Hello World', -5, 5);
```{% endraw %}

이를 해결하려면 음수 인덱스가 아닌 양수 인덱스를 사용해야 합니다. 시작 위치는 1부터 시작하며, 길이는 양수여야 합니다. 예를 들어, 다음과 같이 쿼리를 수정할 수 있습니다:

{% raw %}```sql
SELECT SUBSTRING('Hello World', 7, 5);
```{% endraw %}

위 쿼리는 "Hello World" 문자열의 7번째 위치에서 시작하여 길이 5의 부분 문자열을 반환합니다. 결과는 "World"가 됩니다.

