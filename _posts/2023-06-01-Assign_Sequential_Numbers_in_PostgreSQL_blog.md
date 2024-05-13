---
layout: post
title: "Assign Sequential Numbers in PostgreSQL"
tags: [PostgreSQL, Assign, Numbers, in, Sequential]
---

## 사용자 입력:
> SELECT 
prj_id
,file_id
,sql_id
,table_id
,col_id
,table_name
,col_name
,col_order_no
FROM (
SELECT
    m.prj_id
    , m.file_id
    , m.src_table_id AS sql_id
    , DENSE_RANK() OVER (
ORDER BY
    m.prj_id
    , m.file_id
    , link_s1.ATTR_VALUE
) + 90000 AS table_id
    , m.src_col_id AS col_id
    , link_s1.ATTR_VALUE AS table_name
    , FLOW_S2.ATTR_VALUE AS col_name
    , DENSE_RANK() OVER (
ORDER BY
    m.prj_id
    , m.file_id
    , m.src_table_id
    , link_s1.ATTR_VALUE
    ,flow_s2.TAG_ATTR_SEQ_ID
) AS col_order_no    
--    , ROW_NUMBER() OVER (
--        PARTITION BY
--        m.prj_id
--        , m.file_id
--        , link_s1.ATTR_VALUE
--    ORDER BY
--        flow_s2.TAG_ATTR_SEQ_ID
--    ) AS col_order_no
FROM
    t_transform_296_test m
JOIN ais0038_296_test link ON
    m.prj_id = link.PRJ_ID
    AND m.file_id = link.FILE_ID
    AND m.src_tag_seq_id = link.TAG_SEQ_ID
JOIN ais0039_296_test link_s1 ON
    link.PRJ_ID = LINK_S1.PRJ_ID
    AND link.FILE_ID = LINK_S1.FILE_ID
    AND link.TAG_SEQ_ID = LINK_S1.TAG_SEQ_ID
    AND LINK_S1.ATTR_NAME = 'name'
JOIN ais0038_296_test flow ON
    link.PRJ_ID = FLOW.PRJ_ID
    AND link.FILE_ID = flow.FILE_ID
    AND link.TAG_SEQ_ID = flow.PARENT_SEQ_ID
    AND flow.TAG_ID = 'contains_FlowVariable'
JOIN ais0039_296_test flow_s1 ON
    flow.PRJ_ID = FLOW_S1.PRJ_ID
    AND flow.FILE_ID = FLOW_S1.FILE_ID
    AND flow.TAG_SEQ_ID = FLOW_S1.TAG_SEQ_ID
    AND FLOW_S1.ATTR_NAME = 'xmi:id'
    AND m.prj_id = flow_s1.PRJ_ID
    AND m.file_id = flow_s1.FILE_ID
    AND flow_s1.attr_value = m.src_col_id
JOIN ais0039_296_test flow_s2 ON
    flow.PRJ_ID = FLOW_S2.PRJ_ID
    AND flow.FILE_ID = FLOW_S2.FILE_ID
    AND flow.TAG_SEQ_ID = FLOW_S2.TAG_SEQ_ID
    AND FLOW_S2.ATTR_NAME = 'name'
WHERE   
exists (
        select
                            1
        from
              ais1003 b
            , ais8101 c
        where
            b.prj_id = m.prj_id
            and c.mm_class_id = 1016
            and c.mm_obj_rmk2 = 'M0004'
            -- 'datastage'
            and c.mm_object_name = b.job_id::varchar
    )
) A  이것은 postgresql인데, col_order_no가 prj_id,file_id,src_table_id,attr_value 그룹으로 tag_seq_id 순서대로 순번이 나오게 할려면 어떻게 해야 되나?

## ChatGPT 응답:
> PostgreSQL provides several window functions that can help achieve your desired result. In your case, you want to assign sequential numbers within groups defined by the columns {% raw %}`prj_id`{% endraw %}, {% raw %}`file_id`{% endraw %}, {% raw %}`src_table_id`{% endraw %}, and {% raw %}`attr_value`{% endraw %}, ordered by the {% raw %}`tag_seq_id`{% endraw %} column. To achieve this, you can use the {% raw %}`ROW_NUMBER()`{% endraw %} window function.

Here's an example of how you can modify your query to include the {% raw %}`ROW_NUMBER()`{% endraw %} function for {% raw %}`col_order_no`{% endraw %}:

{% raw %}```sql
SELECT 
    prj_id,
    file_id,
    sql_id,
    table_id,
    col_id,
    table_name,
    col_name,
    col_order_no
FROM (
    SELECT
        m.prj_id,
        m.file_id,
        m.src_table_id AS sql_id,
        DENSE_RANK() OVER (
            ORDER BY
                m.prj_id,
                m.file_id,
                link_s1.ATTR_VALUE
        ) + 90000 AS table_id,
        m.src_col_id AS col_id,
        link_s1.ATTR_VALUE AS table_name,
        FLOW_S2.ATTR_VALUE AS col_name,
        ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) AS col_order_no
    FROM
        t_transform_296_test m
    JOIN ais0038_296_test link ON
        m.prj_id = link.PRJ_ID
        AND m.file_id = link.FILE_ID
        AND m.src_tag_seq_id = link.TAG_SEQ_ID
    JOIN ais0039_296_test link_s1 ON
        link.PRJ_ID = LINK_S1.PRJ_ID
        AND link.FILE_ID = LINK_S1.FILE_ID
        AND link.TAG_SEQ_ID = LINK_S1.TAG_SEQ_ID
        AND LINK_S1.ATTR_NAME = 'name'
    JOIN ais0038_296_test flow ON
        link.PRJ_ID = FLOW.PRJ_ID
        AND link.FILE_ID = flow.FILE_ID
        AND link.TAG_SEQ_ID = flow.PARENT_SEQ_ID
        AND flow.TAG_ID = 'contains_FlowVariable'
    JOIN ais0039_296_test flow_s1 ON
        flow.PRJ_ID = FLOW_S1.PRJ_ID
        AND flow.FILE_ID = FLOW_S1.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S1.TAG_SEQ_ID
        AND FLOW_S1.ATTR_NAME = 'xmi:id'
        AND m.prj_id = flow_s1.PRJ_ID
        AND m.file_id = flow_s1.FILE_ID
        AND flow_s1.attr_value = m.src_col_id
    JOIN ais0039_296_test flow_s2 ON
        flow.PRJ_ID = FLOW_S2.PRJ_ID
        AND flow.FILE_ID = FLOW_S2.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S2.TAG_SEQ_ID
        AND FLOW_S2.ATTR_NAME = 'name'
    WHERE   
        EXISTS (
            SELECT 1
            FROM ais1003 b, ais8101 c
            WHERE
                b.prj_id = m.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                AND c.mm_object_name = b.job_id::varchar
        )
) A;
```{% endraw %}

By adding the {% raw %}`ROW_NUMBER()`{% endraw %} function and specifying the {% raw %}`PARTITION BY`{% endraw %} clause with the desired grouping columns, and the {% raw %}`ORDER BY`{% endraw %} clause with the desired ordering column ({% raw %}`flow_s

## ChatGPT 응답:
> 2.TAG_ATTR_SEQ_ID`{% endraw %} in this case), you can assign sequential numbers within the groups as per your requirement.

## 사용자 입력:
> 중복이 있어서, Dense_rank()를 사용했는데, 순번이 1부터 시작하지 않아.

## ChatGPT 응답:
> If you want the sequential numbers to start from 1 within each group, you can use the {% raw %}`ROW_NUMBER()`{% endraw %} function instead of {% raw %}`DENSE_RANK()`{% endraw %}. Here's the modified query:

{% raw %}```sql
SELECT 
    prj_id,
    file_id,
    sql_id,
    table_id,
    col_id,
    table_name,
    col_name,
    col_order_no
FROM (
    SELECT
        m.prj_id,
        m.file_id,
        m.src_table_id AS sql_id,
        DENSE_RANK() OVER (
            ORDER BY
                m.prj_id,
                m.file_id,
                link_s1.ATTR_VALUE
        ) + 90000 AS table_id,
        m.src_col_id AS col_id,
        link_s1.ATTR_VALUE AS table_name,
        FLOW_S2.ATTR_VALUE AS col_name,
        ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) AS col_order_no
    FROM
        t_transform_296_test m
    JOIN ais0038_296_test link ON
        m.prj_id = link.PRJ_ID
        AND m.file_id = link.FILE_ID
        AND m.src_tag_seq_id = link.TAG_SEQ_ID
    JOIN ais0039_296_test link_s1 ON
        link.PRJ_ID = LINK_S1.PRJ_ID
        AND link.FILE_ID = LINK_S1.FILE_ID
        AND link.TAG_SEQ_ID = LINK_S1.TAG_SEQ_ID
        AND LINK_S1.ATTR_NAME = 'name'
    JOIN ais0038_296_test flow ON
        link.PRJ_ID = FLOW.PRJ_ID
        AND link.FILE_ID = flow.FILE_ID
        AND link.TAG_SEQ_ID = flow.PARENT_SEQ_ID
        AND flow.TAG_ID = 'contains_FlowVariable'
    JOIN ais0039_296_test flow_s1 ON
        flow.PRJ_ID = FLOW_S1.PRJ_ID
        AND flow.FILE_ID = FLOW_S1.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S1.TAG_SEQ_ID
        AND FLOW_S1.ATTR_NAME = 'xmi:id'
        AND m.prj_id = flow_s1.PRJ_ID
        AND m.file_id = flow_s1.FILE_ID
        AND flow_s1.attr_value = m.src_col_id
    JOIN ais0039_296_test flow_s2 ON
        flow.PRJ_ID = FLOW_S2.PRJ_ID
        AND flow.FILE_ID = FLOW_S2.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S2.TAG_SEQ_ID
        AND FLOW_S2.ATTR_NAME = 'name'
    WHERE   
        EXISTS (
            SELECT 1
            FROM ais1003 b, ais8101 c
            WHERE
                b.prj_id = m.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                AND c.mm_object_name = b.job_id::varchar
        )
) A;
```{% endraw %}

By using {% raw %}`ROW_NUMBER()`{% endraw %} instead of {% raw %}`DENSE_RANK()`{% endraw %}, the sequential numbers will start from 1 within each group.

## 사용자 입력:
>         ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) AS col_order_no 이부분을 중복이 있어서 바꿀려고 하는 거야.

## ChatGPT 응답:
> If you want to assign sequential numbers starting from 1 within each group while handling duplicates, you can modify the {% raw %}`ROW_NUMBER()`{% endraw %} function to include a subquery that removes duplicates. Here's an updated version of the query:

{% raw %}```sql
SELECT 
    prj_id,
    file_id,
    sql_id,
    table_id,
    col_id,
    table_name,
    col_name,
    col_order_no
FROM (
    SELECT
        m.prj_id,
        m.file_id,
        m.src_table_id AS sql_id,
        DENSE_RANK() OVER (
            ORDER BY
                m.prj_id,
                m.file_id,
                link_s1.ATTR_VALUE
        ) + 90000 AS table_id,
        m.src_col_id AS col_id,
        link_s1.ATTR_VALUE AS table_name,
        FLOW_S2.ATTR_VALUE AS col_name,
        ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) - 
        ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE,
                flow_s2.TAG_ATTR_SEQ_ID
            ORDER BY
                m.src_col_id
        ) + 1 AS col_order_no
    FROM
        t_transform_296_test m
    JOIN ais0038_296_test link ON
        m.prj_id = link.PRJ_ID
        AND m.file_id = link.FILE_ID
        AND m.src_tag_seq_id = link.TAG_SEQ_ID
    JOIN ais0039_296_test link_s1 ON
        link.PRJ_ID = LINK_S1.PRJ_ID
        AND link.FILE_ID = LINK_S1.FILE_ID
        AND link.TAG_SEQ_ID = LINK_S1.TAG_SEQ_ID
        AND LINK_S1.ATTR_NAME = 'name'
    JOIN

## ChatGPT 응답:
> I apologize for the confusion. If you want to modify the `ROW_NUMBER()` function to handle duplicates and assign sequential numbers starting from 1 within each group, you can use the `DENSE_RANK()` function instead. Here's the modified query:

```{% endraw %}sql
SELECT 
    prj_id,
    file_id,
    sql_id,
    table_id,
    col_id,
    table_name,
    col_name,
    col_order_no
FROM (
    SELECT
        m.prj_id,
        m.file_id,
        m.src_table_id AS sql_id,
        DENSE_RANK() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) + 90000 AS table_id,
        m.src_col_id AS col_id,
        link_s1.ATTR_VALUE AS table_name,
        FLOW_S2.ATTR_VALUE AS col_name,
        ROW_NUMBER() OVER (
            PARTITION BY
                m.prj_id,
                m.file_id,
                m.src_table_id,
                link_s1.ATTR_VALUE
            ORDER BY
                flow_s2.TAG_ATTR_SEQ_ID
        ) AS col_order_no
    FROM
        t_transform_296_test m
    JOIN ais0038_296_test link ON
        m.prj_id = link.PRJ_ID
        AND m.file_id = link.FILE_ID
        AND m.src_tag_seq_id = link.TAG_SEQ_ID
    JOIN ais0039_296_test link_s1 ON
        link.PRJ_ID = LINK_S1.PRJ_ID
        AND link.FILE_ID = LINK_S1.FILE_ID
        AND link.TAG_SEQ_ID = LINK_S1.TAG_SEQ_ID
        AND LINK_S1.ATTR_NAME = 'name'
    JOIN ais0038_296_test flow ON
        link.PRJ_ID = FLOW.PRJ_ID
        AND link.FILE_ID = flow.FILE_ID
        AND link.TAG_SEQ_ID = flow.PARENT_SEQ_ID
        AND flow.TAG_ID = 'contains_FlowVariable'
    JOIN ais0039_296_test flow_s1 ON
        flow.PRJ_ID = FLOW_S1.PRJ_ID
        AND flow.FILE_ID = FLOW_S1.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S1.TAG_SEQ_ID
        AND FLOW_S1.ATTR_NAME = 'xmi:id'
        AND m.prj_id = flow_s1.PRJ_ID
        AND m.file_id = flow_s1.FILE_ID
        AND flow_s1.attr_value = m.src_col_id
    JOIN ais0039_296_test flow_s2 ON
        flow.PRJ_ID = FLOW_S2.PRJ_ID
        AND flow.FILE_ID = FLOW_S2.FILE_ID
        AND flow.TAG_SEQ_ID = FLOW_S2.TAG_SEQ_ID
        AND FLOW_S2.ATTR_NAME = 'name'
    WHERE   
        EXISTS (
            SELECT 1
            FROM ais1003 b, ais8101 c
            WHERE
                b.prj_id = m.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                AND c.mm_object_name = b.job_id::varchar
        )
) A;
{% raw %}``{% endraw %}{% raw %}`

In this modified query, both the `{% endraw %}table_id{% raw %}` and `{% endraw %}col_order_no{% raw %}` columns are generated using the `{% endraw %}DENSE_RANK(){% raw %}` function. This function assigns a unique rank to each group within the specified partition, including duplicates. Adding 90000 to `{% endraw %}DENSE_RANK(){% raw %}` for `{% endraw %}table_id` ensures that the numbering starts from 

## 사용자 입력:
> python에서 제일 좋은 sql parser 라이브러리 추천해줘

## ChatGPT 응답:
> Python has several SQL parsing libraries available. Here are a few popular options:

1. **sqlparse**: sqlparse is a widely-used SQL parsing library for Python. It can parse SQL queries and provide structured representations of the parsed statements. It supports various SQL dialects and provides helpful functions for formatting and analyzing SQL code.

   GitHub Repository: [https://github.com/andialbrecht/sqlparse](https://github.com/andialbrecht/sqlparse)

2. **pycparser**: pycparser is a complete C99 parser written in pure Python. While it is primarily focused on parsing C code, it can also handle SQL queries since SQL shares some similarities with C syntax. If your SQL queries are relatively simple, pycparser can be a viable option.

   GitHub Repository: [https://github.com/eliben/pycparser](https://github.com/eliben/pycparser)

3. **antlr4**: ANTLR (ANother Tool for Language Recognition) is a powerful parser generator that supports multiple programming languages. It provides a Python runtime library, antlr4-python3-runtime, which can be used to parse SQL queries using ANTLR grammar files for SQL.

   Official Website: [https://www.antlr.org/](https://www.antlr.org/)

These libraries offer different features and levels of complexity, so you can choose the one that best fits your specific needs and requirements. It's recommended to explore their documentation and examples to determine which library suits your use case the most.

