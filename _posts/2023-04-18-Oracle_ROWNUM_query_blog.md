---
layout: post
title: "Oracle ROWNUM query"
tags: [ROWNUM, Oracle, query]
---

## 사용자 입력:
> oracle에서 rownumber 매기는 쿼리 예제 알려줘

## ChatGPT 응답:
> Oracle에서 {% raw %}`ROWNUM`{% endraw %} 함수를 사용하여 각 행에 번호를 매길 수 있습니다. 다음은 {% raw %}`ROWNUM`{% endraw %}을 사용하여 {% raw %}`employee`{% endraw %} 테이블에서 직원의 번호와 이름을 선택하는 쿼리의 예입니다.

{% raw %}```sql
SELECT ROWNUM, employee_number, employee_name
FROM employee;
```{% endraw %}

위의 쿼리는 {% raw %}`employee`{% endraw %} 테이블에서 모든 행을 선택하고 각 행에 번호를 매기며, 이 번호는 {% raw %}`ROWNUM`{% endraw %} 열에 표시됩니다. 결과 집합은 {% raw %}`ROWNUM`{% endraw %}, {% raw %}`employee_number`{% endraw %}, {% raw %}`employee_name`{% endraw %} 열을 포함합니다. 

중요한 점은 {% raw %}`ROWNUM`{% endraw %}은 쿼리의 결과 집합이 생성되는 순간에 번호가 매겨지므로, 결과 집합을 다시 정렬하거나 필터링하면 {% raw %}`ROWNUM`{% endraw %}이 다시 지정됩니다. 따라서 {% raw %}`ROWNUM`{% endraw %}을 사용하여 정확한 결과를 얻으려면 결과를 정렬하거나 필터링하기 전에 쿼리를 수정해야 할 수 있습니다.

## 사용자 입력:
> postgresql 도 같이 쓸 수 있는 rownumber 매기는 쿼리 예제 알려줘

## ChatGPT 응답:
> PostgreSQL에서 {% raw %}`ROW_NUMBER()`{% endraw %} 함수를 사용하여 각 행에 번호를 매길 수 있습니다. {% raw %}`ROW_NUMBER()`{% endraw %} 함수는 창 함수이며, 쿼리 결과 집합에서 정렬된 순서로 각 행에 고유 번호를 매깁니다. 다음은 {% raw %}`employee`{% endraw %} 테이블에서 직원의 번호와 이름을 선택하는 쿼리의 예입니다.

{% raw %}```sql
SELECT ROW_NUMBER() OVER(ORDER BY employee_number) as row_num, employee_number, employee_name
FROM employee;
```{% endraw %}

위의 쿼리는 {% raw %}`employee`{% endraw %} 테이블에서 모든 행을 선택하고, {% raw %}`employee_number`{% endraw %}를 기준으로 정렬하여 각 행에 고유 번호를 매기며, 이 번호는 {% raw %}`row_num`{% endraw %} 열에 표시됩니다. 결과 집합은 {% raw %}`row_num`{% endraw %}, {% raw %}`employee_number`{% endraw %}, {% raw %}`employee_name`{% endraw %} 열을 포함합니다.

{% raw %}`ROW_NUMBER()`{% endraw %} 함수도 {% raw %}`ROWNUM`{% endraw %}과 마찬가지로 결과 집합을 정렬하거나 필터링하면 다시 지정됩니다. 필요한 경우 정렬 순서를 변경하거나 창 함수를 수정하여 원하는 결과를 얻을 수 있습니다.

## 사용자 입력:
> 위에 쿼리는 동일하게 oracle에서도 돌아가나?

## ChatGPT 응답:
> 아니요, 위에서 제시한 쿼리는 Oracle에서는 동작하지 않습니다. Oracle에서는 {% raw %}`ROW_NUMBER()`{% endraw %} 함수 대신 {% raw %}`ROWNUM`{% endraw %}을 사용하여 각 행에 번호를 매길 수 있습니다. 다음은 Oracle에서 {% raw %}`employee`{% endraw %} 테이블에서 직원의 번호와 이름을 선택하는 쿼리의 예입니다.

{% raw %}```sql
SELECT ROWNUM as row_num, employee_number, employee_name
FROM employee
ORDER BY employee_number;
```{% endraw %}

위의 쿼리는 {% raw %}`employee`{% endraw %} 테이블에서 모든 행을 선택하고, {% raw %}`employee_number`{% endraw %}를 기준으로 정렬하여 각 행에 고유 번호를 매기며, 이 번호는 {% raw %}`row_num`{% endraw %} 열에 표시됩니다. 결과 집합은 {% raw %}`row_num`{% endraw %}, {% raw %}`employee_number`{% endraw %}, {% raw %}`employee_name`{% endraw %} 열을 포함합니다.

따라서 {% raw %}`ROW_NUMBER()`{% endraw %} 함수는 PostgreSQL에서만 사용할 수 있으며, Oracle에서는 {% raw %}`ROWNUM`{% endraw %}을 사용해야합니다.

## 사용자 입력:
> 양쪽에서 다 사용할 수 있는 쿼리로 알려줘

## ChatGPT 응답:
> Oracle과 PostgreSQL에서 모두 사용할 수 있는 쿼리는 다음과 같습니다.

{% raw %}```sql
SELECT 
  row_number() OVER(ORDER BY employee_number) as row_num, 
  employee_number, 
  employee_name 
FROM employee
ORDER BY employee_number;
```{% endraw %}

위의 쿼리는 {% raw %}`employee`{% endraw %} 테이블에서 모든 행을 선택하고, {% raw %}`employee_number`{% endraw %}를 기준으로 정렬하여 각 행에 고유 번호를 매기며, 이 번호는 {% raw %}`row_num`{% endraw %} 열에 표시됩니다. 결과 집합은 {% raw %}`row_num`{% endraw %}, {% raw %}`employee_number`{% endraw %}, {% raw %}`employee_name`{% endraw %} 열을 포함합니다.

위의 쿼리는 Oracle에서는 {% raw %}`ROW_NUMBER()`{% endraw %} 함수 대신 {% raw %}`row_number()`{% endraw %} 함수를 사용하고, PostgreSQL에서는 {% raw %}`OVER()`{% endraw %} 함수에 {% raw %}`ORDER BY`{% endraw %} 절을 지정하여 동일한 결과를 생성합니다. 따라서 이 쿼리는 Oracle과 PostgreSQL 모두에서 동작하며, 각 데이터베이스의 차이점을 감안하여 작성되었습니다.

