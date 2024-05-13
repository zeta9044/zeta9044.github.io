---
layout: post
title: "Join Optimization Query"
date: 2023-05-22
tags: [Optimization, Query, Join]
categories: blog
---

## 사용자 입력:
>   SELECT * 
        FROM
              ais0038_296_test t1
        JOIN ais0039_296_test s2 ON t1.PRJ_ID = s2.PRJ_ID AND t1.FILE_ID = s2.FILE_ID AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID AND s2.ATTR_NAME = 'xmi:id'
        JOIN ais0039_296_test s3 ON t1.PRJ_ID = s3.PRJ_ID AND t1.FILE_ID = s3.FILE_ID AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID AND s3.ATTR_NAME = 'columnReference'
        JOIN ais0039_296_test s4 ON t1.PRJ_ID = s4.PRJ_ID AND t1.FILE_ID = s4.FILE_ID AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID AND s4.ATTR_NAME = 'tableDef'
        JOIN ais0038_296_test t2 ON t1.PRJ_ID = t2.PRJ_ID AND t1.FILE_ID = t2.FILE_ID AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        JOIN ais0039_296_test s6 ON t2.PRJ_ID = s6.PRJ_ID AND t2.FILE_ID = s6.FILE_ID AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID AND s6.ATTR_NAME = 'xmi:id'
        JOIN ais0039_296_test s7 ON t2.PRJ_ID = s7.PRJ_ID AND t2.FILE_ID = s7.FILE_ID AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID AND s7.ATTR_NAME = 'from_OutputPin'
        JOIN ais0039_296_test s8 ON t1.PRJ_ID = s8.PRJ_ID AND t1.FILE_ID = s8.FILE_ID AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID AND s8.ATTR_NAME = 'name'
        JOIN ais0039_296_test s9 ON t2.PRJ_ID = s9.PRJ_ID AND t2.FILE_ID = s9.FILE_ID AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID AND s9.ATTR_NAME = 'name'
        JOIN t_opin_296_test opin ON t1.prj_id = opin.prj_id AND t1.file_id = opin.file_id AND s7.attr_value = opin.opin_id
        WHERE
            t1.prj_id = '296'
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND EXISTS (
            SELECT
                1
            FROM
                ais1003 b
            JOIN ais8101 c ON c.mm_object_name = b.job_id::varchar
            WHERE
                b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
            ); 이것은 postgresql쿼리인데, 여기서,  t1과 매칭되는 s3와 s4 테이블에 매칭되는 값이 없을 수도 있어. 현재는 join으로 되어 있어서, 전체 쿼리 값이 안나와. 바꿔줘.

## ChatGPT 응답:
> 아래는 변경된 쿼리입니다. 변경된 부분은 `JOIN` 절에서 `LEFT JOIN`으로 바뀌었습니다. 이렇게 함으로써 `t1`과 매칭되는 `s3`와 `s4` 테이블에 매칭되는 값이 없어도 결과가 나오게 됩니다.

```sql
SELECT * 
FROM
    ais0038_296_test t1
JOIN ais0039_296_test s2 ON t1.PRJ_ID = s2.PRJ_ID AND t1.FILE_ID = s2.FILE_ID AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID AND s2.ATTR_NAME = 'xmi:id'
LEFT JOIN ais0039_296_test s3 ON t1.PRJ_ID = s3.PRJ_ID AND t1.FILE_ID = s3.FILE_ID AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID AND s3.ATTR_NAME = 'columnReference'
LEFT JOIN ais0039_296_test s4 ON t1.PRJ_ID = s4.PRJ_ID AND t1.FILE_ID = s4.FILE_ID AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID AND s4.ATTR_NAME = 'tableDef'
JOIN ais0038_296_test t2 ON t1.PRJ_ID = t2.PRJ_ID AND t1.FILE_ID = t2.FILE_ID AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
JOIN ais0039_296_test s6 ON t2.PRJ_ID = s6.PRJ_ID AND t2.FILE_ID = s6.FILE_ID AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID AND s6.ATTR_NAME = 'xmi:id'
JOIN ais0039_296_test s7 ON t2.PRJ_ID = s7.PRJ_ID AND t2.FILE_ID = s7.FILE_ID AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID AND s7.ATTR_NAME = 'from_OutputPin'
JOIN ais0039_296_test s8 ON t1.PRJ_ID = s8.PRJ_ID AND t1.FILE_ID = s8.FILE_ID AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID AND s8.ATTR_NAME = 'name'
JOIN ais0039_296_test s9 ON t2.PRJ_ID = s9.PRJ_ID AND t2.FILE_ID = s9.FILE_ID AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID AND s9.ATTR_NAME = 'name'
JOIN t_opin_296_test opin ON t1.prj_id = opin.prj_id AND t1.file_id = opin.file_id AND s7.attr_value = opin.opin_id
WHERE
    t1.prj_id = '296'
    AND t1.TAG_ID = 'contains_FlowVariable'
    AND EXISTS (
        SELECT
            1
        FROM
            ais1003 b
        JOIN ais8101 c ON c.mm_object_name = b.job_id::varchar
        WHERE
            b.prj_id = t1.prj_id
            AND c.mm_class_id = 1016
            AND c.mm_obj_rmk2 = 'M0004'
    );
```

위의 변경된 쿼리를 사용하면, `s3`와 `s4` 테이블과 매칭되는 값이 없더라도 결과를 얻을 수 있습니다.

## 사용자 입력:
> oracle의 nvl 용도로 postgresql와 oracle 양쪽에서 쓸 수 있는 함수는?

## ChatGPT 응답:
> PostgreSQL에서 Oracle의 `NVL` 함수와 유사한 기능을 수행하는 함수는 `COALESCE`입니다. `COALESCE` 함수는 인자로 전달된 값들 중에서 NULL이 아닌 첫 번째 값을 반환합니다. 

예를 들어, Oracle의 `NVL` 함수를 PostgreSQL에서 `COALESCE` 함수로 대체하려면 다음과 같이 사용할 수 있습니다:

```sql
-- Oracle
SELECT NVL(column_name, default_value) FROM table_name;

-- PostgreSQL
SELECT COALESCE(column_name, default_value) FROM table_name;
```

`column_name`은 검사할 열이며, `default_value`는 `column_name`이 NULL인 경우 반환할 기본값입니다. `COALESCE` 함수는 인자로 전달된 값들을 왼쪽에서 오른쪽으로 순차적으로 검사하며, NULL이 아닌 첫 번째 값을 반환합니다.

따라서 PostgreSQL에서 `COALESCE` 함수를 사용하여 Oracle의 `NVL` 함수와 유사한 기능을 구현할 수 있습니다.

## 사용자 입력:
> oracle도 동일한 함수를 못쓰나?

## ChatGPT 응답:
> Oracle에서도 PostgreSQL의 `COALESCE` 함수와 유사한 기능을 수행하는 함수가 있습니다. Oracle에서 해당 기능을 제공하는 함수는 `NVL` 함수입니다. `NVL` 함수는 인자로 전달된 값들 중에서 NULL이 아닌 첫 번째 값을 반환합니다.

예를 들어, PostgreSQL의 `COALESCE` 함수를 Oracle에서 `NVL` 함수로 대체하려면 다음과 같이 사용할 수 있습니다:

```sql
-- PostgreSQL
SELECT COALESCE(column_name, default_value) FROM table_name;

-- Oracle
SELECT NVL(column_name, default_value) FROM table_name;
```

`column_name`은 검사할 열이며, `default_value`는 `column_name`이 NULL인 경우 반환할 기본값입니다. `NVL` 함수는 인자로 전달된 값들을 왼쪽에서 오른쪽으로 순차적으로 검사하며, NULL이 아닌 첫 번째 값을 반환합니다.

따라서 Oracle에서도 `NVL` 함수를 사용하여 PostgreSQL의 `COALESCE` 함수와 유사한 기능을 구현할 수 있습니다.

## 사용자 입력:
> case when 절에서 subquery를 쓰는 예제 알려줘

## ChatGPT 응답:
> 아래는 `CASE WHEN` 절에서 서브쿼리를 사용하는 예제입니다:

```sql
SELECT column_name,
       CASE
           WHEN column_name IN (SELECT subquery_column FROM subquery_table) THEN 'Match'
           ELSE 'No Match'
       END AS result
FROM your_table;
```

위의 예제에서는 `CASE WHEN` 절 내에서 `IN` 연산자와 서브쿼리를 사용하여 조건을 평가합니다. `column_name`이 `subquery_table`의 `subquery_column`과 일치하는 경우에는 'Match'를 반환하고, 그렇지 않은 경우에는 'No Match'를 반환합니다.

서브쿼리는 `IN`, `NOT IN`, `EXISTS`, `NOT EXISTS`와 같은 비교 연산자와 함께 `CASE WHEN` 절에서 사용될 수 있습니다. 이를 통해 복잡한 조건과 비교를 수행할 수 있습니다. 서브쿼리의 결과는 주로 다른 테이블의 데이터를 기반으로 도출됩니다.

위의 예제를 참고하여 필요에 맞게 `CASE WHEN` 절과 서브쿼리를 활용할 수 있습니다.

## 사용자 입력:
> 위 쿼리는 있는 컬럼을 비교하는 거고, 서브쿼리 값으로 비교하는 예제 알려줘

## ChatGPT 응답:
> 서브쿼리의 결과 값을 사용하여 `CASE WHEN` 절에서 비교하는 예제를 아래에 제시합니다:

```sql
SELECT column_name,
       CASE
           WHEN column_name = (SELECT subquery_column FROM subquery_table) THEN 'Match'
           ELSE 'No Match'
       END AS result
FROM your_table;
```

위의 예제에서는 `CASE WHEN` 절 내에서 `=` 연산자와 서브쿼리를 사용하여 조건을 평가합니다. `column_name`이 `subquery_table`의 `subquery_column`과 동일한 경우에는 'Match'를 반환하고, 그렇지 않은 경우에는 'No Match'를 반환합니다.

서브쿼리는 일반적으로 단일 값 (단일 행, 단일 열)을 반환하도록 작성되어야 합니다. 그렇게 함으로써 서브쿼리의 결과 값을 `CASE WHEN` 절에서 단일 값과 비교할 수 있습니다.

위의 예제를 참고하여 서브쿼리의 결과 값을 사용하여 `CASE WHEN` 절에서 비교하는 로직을 구현할 수 있습니다. 필요에 따라 서브쿼리를 조정하고 비교 연산자를 변경하여 원하는 결과를 얻을 수 있습니다.

## 사용자 입력:
> 원하는 대답이 아니야. (SELECT subquery_column FROM subquery_table)의 값이 null이면 "A"를 반환하고,  null이 아니면 subquery_column 값을 반환하는 걸 알려줘

## ChatGPT 응답:
> 죄송합니다. 이해가 잘못되어 원하는 대답을 드리지 못했습니다. 아래의 예제는 서브쿼리의 결과 값이 NULL인 경우 "A"를 반환하고, NULL이 아닌 경우 서브쿼리의 결과 값을 반환하는 예제입니다:

```sql
SELECT column_name,
       CASE
           WHEN (SELECT subquery_column FROM subquery_table) IS NULL THEN 'A'
           ELSE (SELECT subquery_column FROM subquery_table)
       END AS result
FROM your_table;
```

위의 예제에서는 `CASE WHEN` 절 내에서 `IS NULL` 조건을 사용하여 서브쿼리의 결과 값이 NULL인지 확인합니다. 만약 NULL인 경우에는 'A'를 반환하고, 그렇지 않은 경우에는 서브쿼리의 결과 값을 반환합니다.

서브쿼리의 결과 값이 NULL인지 확인하기 위해 `IS NULL`을 사용하고, 그에 따라 각각의 경우에 대한 값을 지정하여 반환합니다.

위의 예제를 참고하여 서브쿼리의 결과 값이 NULL인지 확인하고, NULL인 경우와 NULL이 아닌 경우에 대한 반환 값을 지정할 수 있습니다. 필요에 따라 서브쿼리와 반환 값을 조정하여 원하는 결과를 얻을 수 있습니다.

