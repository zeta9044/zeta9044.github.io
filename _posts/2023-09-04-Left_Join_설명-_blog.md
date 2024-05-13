---
layout: post
title: "Left Join 설명."
date: 2023-09-04
tags: [설명, Left, Join]
categories: blog
---

## 사용자 입력:
> ansi sql에서 left join을 상세히 예를 들어 쉽게 설명해줘

## ChatGPT 응답:
> `LEFT JOIN`은 ANSI SQL의 중요한 기능 중 하나로, 두 테이블을 결합할 때 왼쪽 테이블의 모든 레코드와 오른쪽 테이블의 일치하는 레코드를 반환합니다. 오른쪽 테이블에 해당하는 일치하는 레코드가 없는 경우, 해당 레코드에 대한 결과는 NULL로 반환됩니다.

간단한 예제로 설명해보겠습니다.

우선 두 개의 테이블이 있다고 가정해봅시다:

1. `학생` 테이블:

\[
\begin{array}{|c|c|}
\hline
학번 & 이름 \\
\hline
1 & 홍길동 \\
2 & 이순신 \\
3 & 강감찬 \\
\hline
\end{array}
\]

2. `과목` 테이블:

\[
\begin{array}{|c|c|}
\hline
학번 & 과목 \\
\hline
1 & 수학 \\
2 & 영어 \\
\hline
\end{array}
\]

만약 `학생` 테이블을 기준으로 `과목` 테이블과 LEFT JOIN을 수행하면 다음과 같은 결과를 얻을 수 있습니다:

\[
\begin{array}{|c|c|c|}
\hline
학번 & 이름 & 과목 \\
\hline
1 & 홍길동 & 수학 \\
2 & 이순신 & 영어 \\
3 & 강감찬 & NULL \\
\hline
\end{array}
\]

여기서 중요한 점은 학번 3인 강감찬은 `과목` 테이블에 해당하는 레코드가 없기 때문에 `과목` 칼럼의 값이 NULL로 나타난다는 것입니다.

SQL 쿼리로 표현하면 다음과 같습니다:
```sql
SELECT 학생.학번, 학생.이름, 과목.과목
FROM 학생
LEFT JOIN 과목 ON 학생.학번 = 과목.학번;
```

이런 방식으로 `LEFT JOIN`은 왼쪽 테이블의 모든 레코드를 보존하면서 오른쪽 테이블의 일치하는 레코드를 결합하게 됩니다.

## 사용자 입력:
> 위 left조인에서 3번의 리스트를 보려면

## ChatGPT 응답:
> 만약 학번 3인 학생의 정보만을 조회하고 싶다면, `WHERE` 절을 사용하여 해당 조건을 명시하면 됩니다.

다음 SQL 쿼리는 `LEFT JOIN`을 사용하여 학번 3인 학생의 정보만을 조회하는 예제입니다:

```sql
SELECT 학생.학번, 학생.이름, 과목.과목
FROM 학생
LEFT JOIN 과목 ON 학생.학번 = 과목.학번
WHERE 학생.학번 = 3;
```

위 쿼리를 실행하면, 다음과 같은 결과를 얻을 수 있습니다:

\[
\begin{array}{|c|c|c|}
\hline
학번 & 이름 & 과목 \\
\hline
3 & 강감찬 & NULL \\
\hline
\end{array}
\]

학번 3인 강감찬 학생은 `과목` 테이블에 대응하는 레코드가 없기 때문에, `과목` 칼럼의 값은 NULL로 반환됩니다.

## 사용자 입력:
> where절은 왼쪽 테이블 기준으로 학생.학번=3을 해야 되는 거지?

## ChatGPT 응답:
> 네, 맞습니다. `LEFT JOIN`을 사용할 때 `WHERE` 절은 왼쪽 테이블 (`학생` 테이블)을 기준으로 필터링을 합니다. 

즉, `WHERE 학생.학번 = 3` 조건을 적용하면, 왼쪽 테이블인 `학생` 테이블에서 학번이 3인 레코드만 선택되고, 그 레코드에 대해 오른쪽 테이블 (`과목` 테이블)과의 조인이 수행됩니다.

따라서 학번 3인 학생의 정보와 그에 해당하는 과목 정보 (만약 있으면)를 조회할 수 있습니다. 만약 해당 학생이 `과목` 테이블에 없는 경우, 과목 칼럼의 값은 NULL로 반환됩니다.

## 사용자 입력:
> A,B,C테이블이 있는데, A,B는 inner join, B,C는 left join으로 하면?

## ChatGPT 응답:
> A, B, C 테이블이 있을 때, A와 B는 `INNER JOIN`으로, 그 후 B와 C는 `LEFT JOIN`으로 결합하려는 상황입니다. 이를 SQL 쿼리로 표현하면 다음과 같습니다:

```sql
SELECT *
FROM A
INNER JOIN B ON A.key = B.key
LEFT JOIN C ON B.key = C.key;
```

이 쿼리의 작동 방식을 간단히 설명하겠습니다:

1. 먼저, `INNER JOIN`을 사용하여 A와 B 테이블을 결합합니다. 이 때, A와 B 모두에서 일치하는 레코드만 선택됩니다.
2. 그 다음, `LEFT JOIN`을 사용하여 B와 C 테이블을 결합합니다. 이 때, B 테이블의 모든 레코드와 C 테이블에서 일치하는 레코드가 선택됩니다. 만약 B 테이블의 어떤 레코드에 해당하는 C 테이블의 레코드가 없다면, 해당 부분은 NULL로 채워집니다.

결과적으로, A와 B에서 일치하는 레코드들 중, B와 C에서 일치하지 않는 레코드들에 대해서는 C 테이블의 칼럼 값들이 NULL로 표시됩니다.

## 사용자 입력:
> 그러면, 위에 where는 B.컬럼 = 상수 이렇게 하는 건가?

## ChatGPT 응답:
> 네, 맞습니다. 만약 B 테이블의 특정 컬럼 값을 기준으로 필터링하고 싶다면 `WHERE` 절을 사용하여 해당 조건을 명시할 수 있습니다.

예를 들어, B 테이블의 `컬럼`이라는 필드에서 값이 "상수"인 레코드만을 선택하고 싶다면, 다음과 같은 SQL 쿼리를 작성할 수 있습니다:

```sql
SELECT *
FROM A
INNER JOIN B ON A.key = B.key
LEFT JOIN C ON B.key = C.key
WHERE B.컬럼 = '상수';
```

이 쿼리는 A와 B 테이블에서 일치하는 레코드들 중, B 테이블의 `컬럼` 값이 "상수"인 레코드만을 선택합니다. 그 후, 선택된 B 테이블의 레코드와 C 테이블의 일치하는 레코드를 결합합니다. 만약 B 테이블의 어떤 레코드에 해당하는 C 테이블의 레코드가 없다면, 해당 부분은 NULL로 채워집니다.

## 사용자 입력:
> SELECT * FROM ais0102 WHERE prj_id ='296' AND file_id =8 AND sql_id =2; 실행하면,  |prj_id|file_id|sql_id|table_id|obj_id|func_id|table_name       |caps_table_name  |owner_name                      |query_type|query_line_no|sql_obj_type|inlineview_yn|dblink_name|table_alias_name|inlineview_src|sql_state|column_no|table_depth|table_order_no|rel_table_id|rel_flow_id|dbc_mapping_yn|teradata_sql_id|unique_owner_name               |unique_owner_tgt_srv_id|sql_name|system_biz_id    |fl_tbl_uid|
|------|-------|------|--------|------|-------|-----------------|-----------------|--------------------------------|----------|-------------|------------|-------------|-----------|----------------|--------------|---------|---------|-----------|--------------|------------|-----------|--------------|---------------|--------------------------------|-----------------------|--------|-----------------|----------|
|296   |8      |2     |72      |0     |0      |L0ME_CONTENT_MAST|L0ME_CONTENT_MAST|#MOB_NBIS_$MOB_NBIS_TNS#.BIL0ADM|C         |212          |tbl         |N            |           |                |              |INSERT   |21       |0          |0             |-1          |105        |N             |               |#MOB_NBIS_$MOB_NBIS_TNS#.BIL0ADM|[owner_undefined]      |        |[owner_undefined]|          |
이고, SELECT * FROM ais0103 WHERE prj_id ='296' AND file_id =8 AND sql_id =2;을 실행하면, |prj_id|file_id|sql_id|table_id|col_id|obj_id|func_id|col_name|col_use_type|col_alias_yn|col_name_src|table_alias_name|query_line_no|column_no|col_alias_name|col_expr|sql_state|col_order_no|col_value_yn|rel_flow_id|rel_table_id|col_scalar_yn|fmt_col_type|fmt_begin_pos|fmt_end_pos|fmt_type|adjust_col_order_no|caps_col_name|fl_tbl_uid|
|------|-------|------|--------|------|------|-------|--------|------------|------------|------------|----------------|-------------|---------|--------------|--------|---------|------------|------------|-----------|------------|-------------|------------|-------------|-----------|--------|-------------------|-------------|----------|
|296   |8      |2     |72      |521   |0     |0      |*       |O           |N           |*           |                |212          |1        |              |*       |INSERT   |0           |N           |105        |-1          |N            |            |-1           |-1         |        |0                  |*            |          |
|296   |8      |2     |73      |522   |0     |0      |*       |O           |N           |*           |                |213          |8        |              |*       |SELECT   |0           |N           |106        |72          |N            |            |-1           |-1         |        |0                  |*            |          |
인데, ais0103을 기준으로 left join을 하는 데 ais0103.table_id, ais0102.table_id를 select하는  쿼리문 만들어봐.

## ChatGPT 응답:
> 요청하신대로 `ais0103`을 기준으로 `ais0102`와 `LEFT JOIN`을 수행하면서 `ais0103.table_id`와 `ais0102.table_id`를 선택하는 쿼리를 작성하겠습니다.

다음은 해당 쿼리입니다:

```sql
SELECT ais0103.table_id AS ais0103_table_id, ais0102.table_id AS ais0102_table_id
FROM ais0103
LEFT JOIN ais0102 
ON ais0103.prj_id = ais0102.prj_id
AND ais0103.file_id = ais0102.file_id
AND ais0103.sql_id = ais0102.sql_id
WHERE ais0103.prj_id ='296' AND ais0103.file_id = 8 AND ais0103.sql_id = 2;
```

이 쿼리는 `ais0103`을 기준으로 `ais0102`와 조인하면서, `prj_id`, `file_id`, `sql_id`를 기준으로 일치하는 레코드를 찾습니다. 결과적으로 `ais0103.table_id`와 `ais0102.table_id` 칼럼의 값을 반환합니다.

