---
layout: post
title: "Convert Query to ANSI"
date: 2023-05-19
tags: [to, Convert, ANSI, Query]
categories: blog
---

## 사용자 입력:
> CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS
/* ibm.datastage-source#1 */
SELECT
    *
FROM
    (
    SELECT
        DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , DAT.owner_name
        , DAT.table_name
        , upper(DAT.table_name) AS caps_table_name
        ,(
        SELECT
            CASE
                WHEN strpos(
                    x.ATTR_VALUE
                , 'ConnectorPX'
                ) > 0 THEN 'tbl'
                ELSE 'obj'
            END
        FROM
            ais0039_$${{prj_id}} x
        WHERE
            x.PRJ_ID = DAT.prj_id
            AND x.file_id = DAT.file_id
            AND x.TAG_SEQ_ID = dat.TAG_SEQ_ID
            AND x.ATTR_NAME = 'stageType'
    ) AS sql_obj_type
        , DAT.col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_name AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , '[owner_undefined]' AS call_owner_name
        , DAT.call_table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id as call_table_name
        , upper(DAT.call_table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id as call_caps_table_name
        , 'obj' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , DAT.owner_name AS unique_owner_name
        , '[owner_undefined]' AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2400 AS data_maker
        , NULL MAPPING_KIND
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS call_col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS adj_col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
        (
        SELECT
            t1.prj_id
            , t1.FILE_ID
            , opin.tag_seq_id
            , opin.sql_id AS sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,s4.attr_value) +10000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s3.ATTR_VALUE AS col_name
            ,(
            SELECT
                CASE
                    WHEN strpos(second_part
                    , '.') > 0
                THEN substring(second_part
                    , 1
                    , strpos(second_part
                    , '.') - 1)
                    ELSE NULL
                END AS column1
            FROM
                (
                SELECT
                    reverse(split_part(reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1)) AS second_part
                FROM
                    (
                    SELECT
                        1 ) AS dummy_table
                    ) AS subquery_alias
            ) owner_name
            ,(
            SELECT
                substring(
                        second_part
                , strpos(
                            second_part
                , '.'
                        ) + 1
                    ) AS column2
            FROM
                (
                SELECT
                    reverse(
                                split_part(
                                    reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) table_name
            , s6.ATTR_VALUE AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY s9.prj_id,s9.file_id,s9.attr_value) +20000 AS call_table_id
            , s2.ATTR_VALUE AS call_col_id
            , s8.ATTR_VALUE AS call_col_name
            , s9.ATTR_VALUE AS call_table_name
        FROM
              ais0038_$${{prj_id}} t1
            , ais0039_$${{prj_id}} s2
            , ais0039_$${{prj_id}} s3
            , ais0039_$${{prj_id}} s4
            , ais0038_$${{prj_id}} t2
            , ais0039_$${{prj_id}} s6
            , ais0039_$${{prj_id}} s7
            , ais0039_$${{prj_id}} s8
            , ais0039_$${{prj_id}} s9
            , t_opin_$${{prj_id}} opin
        WHERE
            t1.prj_id = ##{{prj_id}}
            AND t1.prj_id = opin.prj_id
            AND t1.file_id = opin.file_id
            AND s7.attr_value = opin.opin_id
            AND s7.prj_id = opin.prj_id
            AND s7.file_id = opin.file_id
            AND t1.prj_id = opin.prj_id
            AND t1.file_id = opin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
            AND s2.ATTR_NAME = 'xmi:id'
            AND t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
            AND s3.ATTR_NAME = 'columnReference'
            AND t1.PRJ_ID = s4.PRJ_ID
            AND t1.FILE_ID = s4.FILE_ID
            AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID
            AND s4.ATTR_NAME = 'tableDef'
            AND t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
            AND t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
            AND s6.ATTR_NAME = 'xmi:id'
            AND t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
            AND s7.ATTR_NAME = 'from_OutputPin'
            AND t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
            AND s8.ATTR_NAME = 'name'
            AND t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
            AND s9.ATTR_NAME = 'name'
            AND EXISTS (
            SELECT
                1
            FROM
                ais1003 b
                , ais8101 c
            WHERE
                b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
            )
    ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
; 이 쿼리는 postgresql인데, ansi표준 join으로 바꿔줘.

## ChatGPT 응답:
> ANSI 표준에 따라서, 지정된 쿼리를 JOIN 문으로 바꿀 수 있습니다. PostgreSQL도 ANSI SQL을 지원하므로, 아래와 같이 쿼리를 수정했습니다.

```
CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS
/* ibm.datastage-source#1 */
SELECT
    *
FROM
    (
    SELECT
        DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , DAT.owner_name
        , DAT.table_name
        , upper(DAT.table_name) AS caps_table_name
        ,(
        SELECT
            CASE
                WHEN strpos(
                    x.ATTR_VALUE
                , 'ConnectorPX'
                ) > 0 THEN 'tbl'
                ELSE 'obj'
            END
        FROM
            ais0039_$${{prj_id}} x
        WHERE
            x.PRJ_ID = DAT.prj_id
            AND x.file_id = DAT.file_id
            AND x.TAG_SEQ_ID = dat.TAG_SEQ_ID
            AND x.ATTR_NAME = 'stageType'
    ) AS sql_obj_type
        , DAT.col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_name AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , '[owner_undefined]' AS call_owner_name
        , DAT.call_table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id as call_table_name
        , upper(DAT.call_table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id as call_caps_table_name
        , 'obj' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , DAT.owner_name AS unique_owner_name
        , '[owner_undefined]' AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2400 AS data_maker
        , NULL MAPPING_KIND
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT

## ChatGPT 응답:
> .col_id
    ) AS call_col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS adj_col_order_no
        , ROW_NUMBER() OVER(
        PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
        DAT.col_id
    ) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
        (
        SELECT
            t1.prj_id
            , t1.FILE_ID
            , opin.tag_seq_id
            , opin.sql_id AS sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,s4.attr_value) +10000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s3.ATTR_VALUE AS col_name
            ,(
            SELECT
                CASE
                    WHEN strpos(second_part
                    , '.') > 0
                THEN substring(second_part
                    , 1
                    , strpos(second_part
                    , '.') - 1)
                    ELSE NULL
                END AS column1
            FROM
                (
                SELECT
                    reverse(split_part(reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1)) AS second_part
                FROM
                    (
                    SELECT
                        1 ) AS dummy_table
                    ) AS subquery_alias
            ) owner_name
            ,(
            SELECT
                substring(
                        second_part
                , strpos(
                            second_part
                , '.'
                        ) + 1
                    ) AS column2
            FROM
                (
                SELECT
                    reverse(
                                split_part(
                                    reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) table_name
            , s6.ATTR_VALUE AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY s9.prj_id,s9.file_id,s9.attr_value) +20000 AS call_table_id
            , s2.ATTR_VALUE AS call_col_id
            , s8.ATTR_VALUE AS call_col_name
            , s9.ATTR_VALUE AS call_table_name
        FROM
              ais0038_$${{prj_id}} t1
        JOIN ais0039_$${{prj_id}} s2 ON t1.PRJ_ID = s2.PRJ_ID AND t1.FILE_ID = s2.FILE_ID AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID AND s2.ATTR_NAME = 'xmi:id'
        JOIN ais0039_$${{prj_id}} s3 ON t1.PRJ_ID = s3.PRJ_ID AND t1.FILE_ID = s3.FILE_ID AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID AND s3.ATTR_NAME = 'columnReference'
        JOIN ais0039_$${{prj_id}} s4 ON t1.PRJ_ID = s4.PRJ_ID AND t1.FILE_ID = s4.FILE_ID AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID AND s4.ATTR_NAME = 'tableDef'
        JOIN ais0038_$${{prj_id}} t2 ON t1.PRJ_ID = t2.PRJ_ID AND t1.FILE_ID = t2

## ChatGPT 응답:
> .FILE_ID AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        JOIN ais0039_$${{prj_id}} s6 ON t2.PRJ_ID = s6.PRJ_ID AND t2.FILE_ID = s6.FILE_ID AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID AND s6.ATTR_NAME = 'xmi:id'
        JOIN ais0039_$${{prj_id}} s7 ON t2.PRJ_ID = s7.PRJ_ID AND t2.FILE_ID = s7.FILE_ID AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID AND s7.ATTR_NAME = 'from_OutputPin'
        JOIN ais0039_$${{prj_id}} s8 ON t1.PRJ_ID = s8.PRJ_ID AND t1.FILE_ID = s8.FILE_ID AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID AND s8.ATTR_NAME = 'name'
        JOIN ais0039_$${{prj_id}} s9 ON t2.PRJ_ID = s9.PRJ_ID AND t2.FILE_ID = s9.FILE_ID AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID AND s9.ATTR_NAME = 'name'
        JOIN t_opin_$${{prj_id}} opin ON t1.prj_id = opin.prj_id AND t1.file_id = opin.file_id AND s7.attr_value = opin.opin_id
        WHERE
            t1.prj_id = ##{{prj_id}}
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
            )
    ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
;

## 사용자 입력:
> CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS
/* ibm.datastage-source-file#1 */
SELECT
    *
FROM
    (
    SELECT
          DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , DAT.owner_name
        , DAT.table_name
        , upper(DAT.table_name) AS caps_table_name
        , 'fil' AS sql_obj_type
        , DAT.col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_name AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , '[owner_undefined]' AS call_owner_name
        , DAT.call_table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS call_table_name
        , upper(DAT.call_table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS call_caps_table_name
        , 'obj' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , DAT.owner_name AS unique_owner_name
        , '[owner_undefined]' AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2400 AS data_maker
        , NULL MAPPING_KIND
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS call_col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS adj_col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
            (
        SELECT
            t1.prj_id
            , t1.FILE_ID
            , opin.sql_id AS sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,opin.table_name) + 30000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s3.ATTR_VALUE col_name
            , '[owner_undefined]' AS owner_name
            , opin.table_name AS table_name
            , s6.ATTR_VALUE AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,s9.ATTR_VALUE) + 40000 AS call_table_id
            , s2.ATTR_VALUE AS call_col_id
            , s8.ATTR_VALUE AS call_col_name
            , s9.ATTR_VALUE AS call_table_name
        FROM
              ais0038_$${{prj_id}} t1
            , ais0039_$${{prj_id}} s2
            , ais0039_$${{prj_id}} s3
            , ais0038_$${{prj_id}} t2
            , ais0039_$${{prj_id}} s6
            , ais0039_$${{prj_id}} s7
            , ais0039_$${{prj_id}} s8
            , ais0039_$${{prj_id}} s9
            , t_opin_file_$${{prj_id}} opin
        WHERE
                t1.prj_id = ##{{prj_id}}
            AND t1.prj_id = opin.prj_id
            AND t1.file_id = opin.file_id
            AND s7.attr_value = opin.opin_id
            AND s7.prj_id = opin.prj_id
            AND s7.file_id = opin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
            AND s2.ATTR_NAME = 'xmi:id'
            AND t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
            AND s3.ATTR_NAME = 'columnReference'
            AND t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
            AND t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
            AND s6.ATTR_NAME = 'xmi:id'
            AND t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
            AND s7.ATTR_NAME = 'from_OutputPin'
            AND t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
            AND s8.ATTR_NAME = 'name'
            AND t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
            AND s9.ATTR_NAME = 'name'
            AND EXISTS (
            SELECT
                            1
            FROM
                            ais1003 b
                , ais8101 c
            WHERE
                            b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
                    )
            ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
;  이 쿼리는 postgresql인데, ansi표준 join으로 바꿔줘.

## ChatGPT 응답:
> Sure, here's your query in ANSI standard JOIN syntax:

```sql
CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS
/* ibm.datastage-source-file#1 */
SELECT
    *
FROM
    (
    SELECT
          DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , DAT.owner_name
        , DAT.table_name
        , upper(DAT.table_name) AS caps_table_name
        , 'fil' AS sql_obj_type
        , DAT.col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_name AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , '[owner_undefined]' AS call_owner_name
        , DAT.call_table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS call_table_name
        , upper(DAT.call_table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS call_caps_table_name
        , 'obj' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , DAT.owner_name AS unique_owner_name
        , '[owner_undefined]' AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2400 AS data_maker
        , NULL MAPPING_KIND
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS call_col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS adj_col_order_no
        , ROW_NUMBER() OVER(
                PARTITION BY DAT.prj_id
        , DAT.File_id
        , DAT.sql_id
        , DAT.table_id
    ORDER BY
                DAT.col_id
            ) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
            (


## ChatGPT 응답:
>         SELECT
            t1.prj_id
            , t1.FILE_ID
            , opin.sql_id AS sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,opin.table_name) + 30000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s3.ATTR_VALUE col_name
            , '[owner_undefined]' AS owner_name
            , opin.table_name AS table_name
            , s6.ATTR_VALUE AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY opin.prj_id,opin.file_id,s9.ATTR_VALUE) + 40000 AS call_table_id
            , s2.ATTR_VALUE AS call_col_id
            , s8.ATTR_VALUE AS call_col_name
            , s9.ATTR_VALUE AS call_table_name
        FROM ais0038_$${{prj_id}} t1
        INNER JOIN ais0039_$${{prj_id}} s2 ON t1.PRJ_ID = s2.PRJ_ID AND t1.FILE_ID = s2.FILE_ID AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s3 ON t1.PRJ_ID = s3.PRJ_ID AND t1.FILE_ID = s3.FILE_ID AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
        INNER JOIN ais0038_$${{prj_id}} t2 ON t1.PRJ_ID = t2.PRJ_ID AND t1.FILE_ID = t2.FILE_ID AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s6 ON t2.PRJ_ID = s6.PRJ_ID AND t2.FILE_ID = s6.FILE_ID AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s7 ON t2.PRJ_ID = s7.PRJ_ID AND t2.FILE_ID = s7.FILE_ID AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s8 ON t1.PRJ_ID = s8.PRJ_ID AND t1.FILE_ID = s8.FILE_ID AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s9 ON t2.PRJ_ID = s9.PRJ_ID AND t2.FILE_ID = s9.FILE_ID AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
        INNER JOIN t_opin_file_$${{prj_id}} opin ON t1.prj_id = opin.prj_id AND t1.file_id = opin.file_id AND s7.attr_value = opin.opin_id
        WHERE
                t1.prj_id = ##{{prj_id}}
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND s2.ATTR_NAME = 'xmi:id'
            AND s3.ATTR_NAME = 'columnReference'
            AND s6.ATTR_NAME = 'xmi:id'
            AND s7.ATTR_NAME = 'from_OutputPin'
            AND s8.ATTR_NAME = 'name'
            AND s9.ATTR_NAME = 'name'
            AND EXISTS (
            SELECT
                            1
            FROM ais1003 b
            INNER JOIN ais8101 c ON c.mm_object_name = b.job_id::varchar
            WHERE
                            b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 =

## ChatGPT 응답:
>  'M0004' -- 'datastage'
            )
    ) DAT
) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
;

## 사용자 입력:
> CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS    
/* ibm.datastage-target#2 */
SELECT
    *
FROM
    (
    SELECT
        DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , '[owner_undefined]' AS owner_name
        , DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name
        , upper(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name
        , 'obj' AS sql_obj_type
        , DAT.col_name AS col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_expr AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , DAT.call_owner_name AS call_owner_name
        , DAT.call_table_name AS call_table_name
        , upper(DAT.call_table_name) AS call_caps_table_name
        , (
        SELECT
            CASE
                WHEN strpos(
                            x.ATTR_VALUE
                , 'ConnectorPX'
                ) > 0 THEN 'tbl'
                ELSE 'obj'
            END
        FROM
            ais0039_$${{prj_id}} x
        WHERE
            x.PRJ_ID = DAT.prj_id
            AND x.file_id = DAT.file_id
            AND x.TAG_SEQ_ID = dat.TAG_SEQ_ID
            AND x.ATTR_NAME = 'stageType'
    ) AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , '[owner_undefined]' AS unique_owner_name
        , DAT.call_owner_name AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2401 AS data_maker
        , NULL MAPPING_KIND
        , CAST(DAT.col_order_no AS NUMERIC) AS col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
        (
        SELECT
            t1.prj_id
            , t1.FILE_ID
            , ipin.tag_seq_id
            , ipin.sql_id AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s4.attr_value) +60000 AS call_table_id
            , s2.ATTR_VALUE AS  call_col_id
            , s3.ATTR_VALUE AS call_col_name
            , (
            SELECT
                CASE
                    WHEN strpos(second_part
                    , '.') > 0
                THEN substring(second_part
                    , 1
                    , strpos(second_part
                    , '.') - 1)
                    ELSE NULL
                END AS column1
            FROM
                (
                SELECT
                    reverse(
                                        split_part(
                                            reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) call_owner_name
            , (
            SELECT
                substring(
                                second_part
                , strpos(
                                    second_part
                , '.'
                        ) + 1
                    ) AS column2
            FROM
                (
                SELECT
                    reverse(
                                        split_part(
                                            reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) call_table_name
            , s6.ATTR_VALUE AS sql_id
            , DENSE_RANK() OVER (ORDER BY s9.prj_id,s9.file_id,s9.attr_value) +50000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s8.ATTR_VALUE AS col_name
            , (
            SELECT
                VALUE_S1.ATTR_VALUE
            FROM
                ais0038_$${{prj_id}} value
                , ais0039_$${{prj_id}} value_s1
            WHERE
                t1.PRJ_ID = value.PRJ_ID
                AND t1.FILE_ID = value.FILE_ID
                AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                AND value.PRJ_ID = VALUE_S1.PRJ_ID
                AND value.FILE_ID = VALUE_S1.FILE_ID
                AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
                AND VALUE_S1.ATTR_NAME = 'expression'
            ) AS col_expr
            , s9.ATTR_VALUE AS table_name
            , s10.ATTR_VALUE AS col_order_no
        FROM
              ais0038_$${{prj_id}} t1
            , ais0039_$${{prj_id}} s2
            , ais0039_$${{prj_id}} s3
            , ais0039_$${{prj_id}} s4
            , ais0038_$${{prj_id}} t2
            , ais0039_$${{prj_id}} s6
            , ais0039_$${{prj_id}} s7
            , ais0039_$${{prj_id}} s8
            , ais0039_$${{prj_id}} s9
            , ais0039_$${{prj_id}} s10
            , t_ipin_$${{prj_id}} ipin
        WHERE
                t1.prj_id = ##{{prj_id}}
            AND t1.prj_id = ipin.prj_id
            AND t1.file_id = ipin.file_id
            AND s7.attr_value = ipin.ipin_id
            AND s7.prj_id = ipin.prj_id
            AND s7.file_id = ipin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
            AND s2.ATTR_NAME = 'xmi:id'
            AND t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
            AND s3.ATTR_NAME = 'columnReference'
            AND t1.PRJ_ID = s4.PRJ_ID
            AND t1.FILE_ID = s4.FILE_ID
            AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID
            AND s4.ATTR_NAME = 'tableDef'
            AND t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
            AND t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
            AND s6.ATTR_NAME = 'xmi:id'
            AND t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
            AND s7.ATTR_NAME = 'to_InputPin'
            AND t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
            AND s8.ATTR_NAME = 'name'
            AND t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
            AND s9.ATTR_NAME = 'name'
            AND t1.PRJ_ID = s10.PRJ_ID
            AND t1.FILE_ID = s10.FILE_ID
            AND t1.TAG_SEQ_ID = s10.TAG_SEQ_ID
            AND s10.ATTR_NAME = 'sequenceNumber'
            AND EXISTS (
            SELECT
                1
            FROM
                ais1003 b
                , ais8101 c
            WHERE
                b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
            )
    ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
;  이 쿼리는 postgresql인데, ansi표준 join으로 바꿔줘.

## ChatGPT 응답:
> 네, 이 쿼리를 ANSI 표준 조인으로 변환해보겠습니다.

```sql
CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS    
SELECT
    A.*
FROM
    (
    SELECT
        DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , '[owner_undefined]' AS owner_name
        , DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name
        , upper(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name
        , 'obj' AS sql_obj_type
        , DAT.col_name AS col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_expr AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , DAT.call_owner_name AS call_owner_name
        , DAT.call_table_name AS call_table_name
        , upper(DAT.call_table_name) AS call_caps_table_name
        , (
        SELECT
            CASE
                WHEN strpos(
                            x.ATTR_VALUE
                , 'ConnectorPX'
                ) > 0 THEN 'tbl'
                ELSE 'obj'
            END
        FROM
            ais0039_$${{prj_id}} x
        WHERE
            x.PRJ_ID = DAT.prj_id
            AND x.file_id = DAT.file_id
            AND x.TAG_SEQ_ID = dat.TAG_SEQ_ID
            AND x.ATTR_NAME = 'stageType'
    ) AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , '[owner_undefined]' AS unique_owner_name
        , DAT.call_owner_name AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2401 AS data_maker
        , NULL MAPPING_KIND
        , CAST(DAT.col_order_no AS NUMERIC) AS col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
        (
        SELECT
            t1.prj_id
            , t1.FILE

## ChatGPT 응답:
> _ID
            , ipin.tag_seq_id
            , ipin.sql_id AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s4.attr_value) +60000 AS call_table_id
            , s2.ATTR_VALUE AS  call_col_id
            , s3.ATTR_VALUE AS call_col_name
            , (
            SELECT
                CASE
                    WHEN strpos(second_part
                    , '.') > 0
                THEN substring(second_part
                    , 1
                    , strpos(second_part
                    , '.') - 1)
                    ELSE NULL
                END AS column1
            FROM
                (
                SELECT
                    reverse(
                                        split_part(
                                            reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) call_owner_name
            , (
            SELECT
                substring(
                                second_part
                , strpos(
                                    second_part
                , '.'
                        ) + 1
                    ) AS column2
            FROM
                (
                SELECT
                    reverse(
                                        split_part(
                                            reverse(s4.ATTR_VALUE)
                    , E'\\'
                    , 1
                                )
                            ) AS second_part
                FROM
                    (
                    SELECT
                        1
                            ) AS dummy_table
                    ) AS subquery_alias
            ) call_table_name
            , s6.ATTR_VALUE AS sql_id
            , DENSE_RANK() OVER (ORDER BY s9.prj_id,s9.file_id,s9.attr_value) +50000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s8.ATTR_VALUE AS col_name
            , (
            SELECT
                VALUE_S1.ATTR_VALUE
            FROM
                ais0038_$${{prj_id}} value
                INNER JOIN ais0039_$${{prj_id}} value_s1
                ON value.PRJ_ID = VALUE_S1.PRJ_ID
                AND value.FILE_ID = VALUE_S1.FILE_ID
                AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
                AND VALUE_S1.ATTR_NAME = 'expression'
            WHERE
                t1.PRJ_ID = value.PRJ_ID
                AND t1.FILE_ID = value.FILE_ID
                AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
            ) AS col_expr
            , s9.ATTR_VALUE AS table_name
            , s10.ATTR_VALUE AS col_order_no
        FROM
            ais0038_$${{prj_id}} t1
            INNER JOIN ais0039_$${{prj_id}} s2
            ON t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
            AND s2.ATTR_NAME = 'xmi:id'
            INNER JOIN ais0039_$${{prj_id}} s3
            ON t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
            AND s3.ATTR_NAME = 'columnReference'
            INNER JOIN ais0039_$${{prj_id}} s4
            ON t1.PRJ_ID = s4.PRJ_ID
            AND t1.FILE_ID = s4.FILE_ID
            AND t1.TAG_SEQ_ID = s4.TAG_SEQ_ID
            AND s4.ATTR_NAME =

## ChatGPT 응답:
>  'tableDef'
            INNER JOIN ais0038_$${{prj_id}} t2
            ON t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
            INNER JOIN ais0039_$${{prj_id}} s6
            ON t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
            AND s6.ATTR_NAME = 'xmi:id'
            INNER JOIN ais0039_$${{prj_id}} s7
            ON t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
            AND s7.ATTR_NAME = 'to_InputPin'
            INNER JOIN ais0039_$${{prj_id}} s8
            ON t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
            AND s8.ATTR_NAME = 'name'
            INNER JOIN ais0039_$${{prj_id}} s9
            ON t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
            AND s9.ATTR_NAME = 'name'
            INNER JOIN ais0039_$${{prj_id}} s10
            ON t1.PRJ_ID = s10.PRJ_ID
            AND t1.FILE_ID = s10.FILE_ID
            AND t1.TAG_SEQ_ID = s10.TAG_SEQ_ID
            AND s10.ATTR_NAME = 'sequenceNumber'
            INNER JOIN t_ipin_$${{prj_id}} ipin
            ON t1.prj_id = ipin.prj_id
            AND t1.file_id = ipin.file_id
            AND s7.attr_value = ipin.ipin_id
            AND s7.prj_id = ipin.prj_id
            AND s7.file_id = ipin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND EXISTS (
            SELECT
                1
            FROM
                ais1003 b
                INNER JOIN ais8101 c
                ON c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
            WHERE
                b.prj_id = t1.prj_id
            )
    ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )


## ChatGPT 응답:
> ;
```
참고로 위의 SQL은 아래의 조건에 대한 데이터를 가져오는데 사용되었습니다:

1. `ais0038_$${{prj_id}}`와 `ais0039_$${{prj_id}}`에 대한 조인을 사용하여 여러 특성을 추출합니다.
2. 일부 중첩된 SELECT문은 복잡한 문자열 처리를 위해 사용되었습니다.
3. 몇 가지 중첩된 SELECT문은 다른 테이블과의 조인을 수행하여 특정 특성 값을 추출합니다.
4. `NOT EXISTS` 조건은 중복 데이터를 필터링하기 위해 사용되었습니다. 

이 SQL 문은 여러 테이블에서 데이터를 추출하고 가공하여 새로운 임시 테이블 `T_AIS0113_$${{prj_id}}`을 생성합니다. 이 테이블은 프로젝트 아이디를 기준으로 각각 다른 데이터를 포함합니다.

문의 사항이 있으면 언제든지 말씀해주세요.

## 사용자 입력:
> CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS    
/* ibm.datastage-target-file#2 */
SELECT
    *
FROM
    (
    SELECT
            DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , '[owner_undefined]' AS owner_name
        , DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name
        , upper(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name
        , 'obj' AS sql_obj_type
        , DAT.col_name AS col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_expr AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , DAT.call_owner_name AS call_owner_name
        , DAT.call_table_name AS call_table_name
        , upper(DAT.call_table_name) AS call_caps_table_name
        , 'fil' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , '[owner_undefined]' AS unique_owner_name
        , DAT.call_owner_name AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2401 AS data_maker
        , NULL MAPPING_KIND
        , CAST(DAT.col_order_no AS NUMERIC) AS col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
            (
        SELECT
             t1.prj_id
            , t1.FILE_ID
            , ipin.sql_id AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,ipin.table_name) + 80000 AS call_table_id
            , s2.TAG_ATTR_SEQ_ID call_col_id
            , s3.ATTR_VALUE call_col_name
            , '[owner_undefined]' call_owner_name
            , ipin.table_name AS call_table_name
            , s6.ATTR_VALUE AS sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s9.ATTR_VALUE) + 70000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s8.ATTR_VALUE AS col_name
            , (
            SELECT
                VALUE_S1.ATTR_VALUE
            FROM
                ais0038_$${{prj_id}} value
                , ais0039_$${{prj_id}} value_s1
            WHERE
                t1.PRJ_ID = value.PRJ_ID
                AND t1.FILE_ID = value.FILE_ID
                AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                AND value.PRJ_ID = VALUE_S1.PRJ_ID
                AND value.FILE_ID = VALUE_S1.FILE_ID
                AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
                AND VALUE_S1.ATTR_NAME = 'expression'
            ) AS col_expr
            , s9.ATTR_VALUE AS table_name
            , s10.ATTR_VALUE AS col_order_no
        FROM
              ais0038_$${{prj_id}} t1
            , ais0039_$${{prj_id}} s2
            , ais0039_$${{prj_id}} s3
            , ais0038_$${{prj_id}} t2
            , ais0039_$${{prj_id}} s6
            , ais0039_$${{prj_id}} s7
            , ais0039_$${{prj_id}} s8
            , ais0039_$${{prj_id}} s9
            , ais0039_$${{prj_id}} s10
            , t_ipin_file_$${{prj_id}} ipin
        WHERE
                t1.prj_id = ##{{prj_id}}
            AND t1.prj_id = ipin.prj_id
            AND t1.file_id = ipin.file_id
            AND s7.attr_value = ipin.ipin_id
            AND s7.prj_id = ipin.prj_id
            AND s7.file_id = ipin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
            AND s2.ATTR_NAME = 'xmi:id'
            AND t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
            AND s3.ATTR_NAME = 'columnReference'
            AND t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
            AND t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
            AND s6.ATTR_NAME = 'xmi:id'
            AND t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
            AND s7.ATTR_NAME = 'to_InputPin'
            AND t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
            AND s8.ATTR_NAME = 'name'
            AND t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
            AND s9.ATTR_NAME = 'name'
            AND t1.PRJ_ID = s10.PRJ_ID
            AND t1.FILE_ID = s10.FILE_ID
            AND t1.TAG_SEQ_ID = s10.TAG_SEQ_ID
            AND s10.ATTR_NAME = 'sequenceNumber'
            AND EXISTS (
            SELECT
                            1
            FROM
                            ais1003 b
                , ais8101 c
            WHERE
                    b.prj_id = t1.prj_id
                AND c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
                    )
            ) DAT
    ) A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
;  이 쿼리는 postgresql인데, ansi표준 join으로 바꿔줘.

## ChatGPT 응답:
> ANSI 표준 JOIN 형식으로 변경한 쿼리는 다음과 같습니다:

```sql
CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS    
/* ibm.datastage-target-file#2 */
SELECT
    *
FROM
    (
    SELECT
            DAT.prj_id
        , DAT.file_id
        , DAT.sql_id AS SQL_id
        , CAST(DAT.table_id AS NUMERIC)
        , CAST(DAT.col_id AS NUMERIC)
        , DAT.prj_id AS call_prj_id
        , DAT.file_id AS call_file_id
        , DAT.call_sql_id AS call_SQL_id
        , CAST(DAT.call_table_id AS NUMERIC) AS call_table_id
        , CAST(DAT.call_col_id AS NUMERIC) AS call_col_id
        , 0 AS obj_id
        , 0 AS func_id
        , '[owner_undefined]' AS owner_name
        , DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name
        , upper(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name
        , 'obj' AS sql_obj_type
        , DAT.col_name AS col_name
        , upper(DAT.col_name) AS caps_col_name
        , 'N' AS col_value_yn
        , DAT.col_expr AS col_expr
        , DAT.col_name AS col_name_org
        , upper(DAT.col_name) AS caps_col_name_org
        , 0 AS call_obj_id
        , 0 AS call_func_id
        , DAT.call_owner_name AS call_owner_name
        , DAT.call_table_name AS call_table_name
        , upper(DAT.call_table_name) AS call_caps_table_name
        , 'fil' AS call_sql_obj_type
        , DAT.call_col_name AS call_col_name
        , upper(DAT.call_col_name) AS call_caps_col_name
        , 'N' AS call_col_value_yn
        , '' AS call_col_expr
        , DAT.call_col_name AS call_col_name_org
        , upper(DAT.call_col_name) AS call_caps_col_name_org
        , '[owner_undefined]' AS unique_owner_name
        , DAT.call_owner_name AS call_unique_owner_name
        , '[owner_undefined]' AS unique_owner_tgt_srv_id
        , '[owner_undefined]' AS call_unique_owner_tgt_srv_id
        , 1 AS cond_mapping
        , 2401 AS data_maker
        , NULL MAPPING_KIND
        , CAST(DAT.col_order_no AS NUMERIC) AS col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no
        , CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no
        , '[owner_undefined]' AS SYSTEM_BIZ_ID
        , '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
            (
        SELECT
             t1.prj_id
            , t1.FILE_ID
            , ipin.sql_id AS call_sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,ipin.table_name) + 80000 AS call_table_id
            , s2.TAG_ATTR_SEQ_ID call_col_id
            , s3.ATTR_VALUE call_col_name
            , '[owner_undefined]' call_owner_name
            , ipin.table_name AS call_table_name
           

## ChatGPT 응답:
>  , s6.ATTR_VALUE AS sql_id
            , DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s9.ATTR_VALUE) + 70000 AS table_id
            , s2.ATTR_VALUE AS col_id
            , s8.ATTR_VALUE AS col_name
            , (
            SELECT
                VALUE_S1.ATTR_VALUE
            FROM
                ais0038_$${{prj_id}} AS value
            INNER JOIN ais0039_$${{prj_id}} AS value_s1
                ON value.PRJ_ID = VALUE_S1.PRJ_ID
                AND value.FILE_ID = VALUE_S1.FILE_ID
                AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
            WHERE
                t1.PRJ_ID = value.PRJ_ID
                AND t1.FILE_ID = value.FILE_ID
                AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                AND VALUE_S1.ATTR_NAME = 'expression'
            ) AS col_expr
            , s9.ATTR_VALUE AS table_name
            , s10.ATTR_VALUE AS col_order_no
        FROM
              ais0038_$${{prj_id}} AS t1
        INNER JOIN ais0039_$${{prj_id}} AS s2 ON t1.PRJ_ID = s2.PRJ_ID
                AND t1.FILE_ID = s2.FILE_ID
                AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s3 ON t1.PRJ_ID = s3.PRJ_ID
                AND t1.FILE_ID = s3.FILE_ID
                AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
        INNER JOIN ais0038_$${{prj_id}} AS t2 ON t1.PRJ_ID = t2.PRJ_ID
                AND t1.FILE_ID = t2.FILE_ID
                AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s6 ON t2.PRJ_ID = s6.PRJ_ID
                AND t2.FILE_ID = s6.FILE_ID
                AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s7 ON t2.PRJ_ID = s7.PRJ_ID
                AND t2.FILE_ID = s7.FILE_ID
                AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s8 ON t1.PRJ_ID = s8.PRJ_ID
                AND t1.FILE_ID = s8.FILE_ID
                AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s9 ON t2.PRJ_ID = s9.PRJ_ID
                AND t2.FILE_ID = s9.FILE_ID
                AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} AS s10 ON t1.PRJ_ID = s10.PRJ_ID
                AND t1.FILE_ID = s10.FILE_ID
                AND t1.TAG_SEQ_ID = s10.TAG_SEQ_ID
        INNER JOIN t_ipin_file_$${{prj_id}} AS ipin ON t1.prj_id = ipin.prj_id
                AND t1.file_id = ipin.file_id
        WHERE
                t1.prj_id = ##{{prj_id}}
            AND s7.attr_value = ipin.ipin_id
            AND

## ChatGPT 응답:
>  s7.prj_id = ipin.prj_id
            AND s7.file_id = ipin.file_id
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND s2.ATTR_NAME = 'xmi:id'
            AND s3.ATTR_NAME = 'columnReference'
            AND s6.ATTR_NAME = 'xmi:id'
            AND s7.ATTR_NAME = 'to_InputPin'
            AND s8.ATTR_NAME = 'name'
            AND s9.ATTR_NAME = 'name'
            AND s10.ATTR_NAME = 'sequenceNumber'
            AND EXISTS (
            SELECT
                            1
            FROM
                            ais1003 AS b
            INNER JOIN ais8101 AS c ON c.mm_class_id = 1016
                AND c.mm_obj_rmk2 = 'M0004'
                -- 'datastage'
                AND c.mm_object_name = b.job_id::varchar
            WHERE
                    b.prj_id = t1.prj_id
                    )
            ) AS DAT
    ) AS A
WHERE
    NOT EXISTS (
    SELECT
            'X'
    FROM
            AIS0113 AS A113
    WHERE
            A113.PRJ_ID = A.PRJ_ID
        AND A113.FILE_ID = A.FILE_ID
        AND A113.SQL_ID = CAST(A.SQL_ID AS NUMERIC)
        AND A113.TABLE_ID = A.TABLE_ID 
        AND A113.COL_ID = CAST(a.col_id AS NUMERIC)
        AND A113.CALL_PRJ_ID = A.CALL_PRJ_ID
        AND A113.CALL_FILE_ID = A.CALL_FILE_ID
        AND A113.CALL_SQL_ID = CAST(A.CALL_SQL_ID AS NUMERIC)
        AND A113.CALL_TABLE_ID = A.CALL_TABLE_ID
        AND A113.CALL_COL_ID = CAST(a.call_col_id AS NUMERIC)
    )
; 

I've replaced all instances of the old join syntax with the ANSI standard JOIN syntax. The change mainly involves replacing comma-separated table lists with explicit JOINs, adding the keyword ON before the join conditions.

## 사용자 입력:
> 위 쿼리를 가독성 있는 포맷으로 바꿔줘.

## ChatGPT 응답:
> Sure, I've rearranged and formatted the query to improve readability:

```SQL
CREATE TEMPORARY TABLE T_AIS0113_$${{prj_id}} AS    

SELECT
    *
FROM
(
    SELECT
        DAT.prj_id,
        DAT.file_id,
        DAT.sql_id AS SQL_id,
        CAST(DAT.table_id AS NUMERIC),
        CAST(DAT.col_id AS NUMERIC),
        DAT.prj_id AS call_prj_id,
        DAT.file_id AS call_file_id,
        DAT.call_sql_id AS call_SQL_id,
        CAST(DAT.call_table_id AS NUMERIC) AS call_table_id,
        CAST(DAT.call_col_id AS NUMERIC) AS call_col_id,
        0 AS obj_id,
        0 AS func_id,
        '[owner_undefined]' AS owner_name,
        DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name,
        UPPER(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name,
        'obj' AS sql_obj_type,
        DAT.col_name AS col_name,
        UPPER(DAT.col_name) AS caps_col_name,
        'N' AS col_value_yn,
        DAT.col_expr AS col_expr,
        DAT.col_name AS col_name_org,
        UPPER(DAT.col_name) AS caps_col_name_org,
        0 AS call_obj_id,
        0 AS call_func_id,
        DAT.call_owner_name AS call_owner_name,
        DAT.call_table_name AS call_table_name,
        UPPER(DAT.call_table_name) AS call_caps_table_name,
        'fil' AS call_sql_obj_type,
        DAT.call_col_name AS call_col_name,
        UPPER(DAT.call_col_name) AS call_caps_col_name,
        'N' AS call_col_value_yn,
        '' AS call_col_expr,
        DAT.call_col_name AS call_col_name_org,
        UPPER(DAT.call_col_name) AS call_caps_col_name_org,
        '[owner_undefined]' AS unique_owner_name,
        DAT.call_owner_name AS call_unique_owner_name,
        '[owner_undefined]' AS unique_owner_tgt_srv_id,
        '[owner_undefined]' AS call_unique_owner_tgt_srv_id,
        1 AS cond_mapping,
        2401 AS data_maker,
        NULL MAPPING_KIND,
        CAST(DAT.col_order_no AS NUMERIC) AS col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no,
        '[owner_undefined]' AS SYSTEM_BIZ_ID,
        '[owner_undefined]' AS CALL_SYSTEM_BIZ_ID
    FROM
    (
        SELECT
            t1.prj_id,
            t1.FILE_ID,
            ipin.sql_id AS call_sql_id,
            DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,ipin.table_name) + 80000 AS call_table_id,
            s2.TAG_ATTR_SEQ_ID call_col_id,
            s3.ATTR_VALUE call_col_name,
            '[owner_undefined]' call_owner_name,
            ipin.table_name AS call_table_name,
            s6.ATTR_VALUE AS sql_id,
            DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s9.ATTR_VALUE) + 70000 AS table_id,
            s2.ATTR_VALUE AS col_id,
            s8.ATTR_VALUE AS col_name,
            (
                SELECT
                    VALUE_S1.ATTR_VALUE

## ChatGPT 응답:
> 
                FROM
                    ais0038_$${{prj_id}} value
                INNER JOIN ais0039_$${{prj_id}} value_s1 ON value.PRJ_ID = VALUE_S1.PRJ_ID
                    AND value.FILE_ID = VALUE_S1.FILE_ID
                    AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
                WHERE
                    t1.PRJ_ID = value.PRJ_ID
                    AND t1.FILE_ID = value.FILE_ID
                    AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                    AND VALUE_S1.ATTR_NAME = 'expression'
            ) AS col_expr,
            s9.ATTR_VALUE AS table_name,
            s10.ATTR_VALUE AS col_order_no
        FROM
            ais0038_$${{prj_id}} t1
        INNER JOIN ais0039_$${{prj_id}} s2 ON t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s3 ON t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
        INNER JOIN ais0038_$${{prj_id}} t2 ON t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s6 ON t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s7 ON t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s8 ON t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s9 ON t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s10 ON t1.PRJ_ID = s10.PRJ_ID
            AND t1.FILE_ID = s10.FILE_ID
            AND t1.TAG_SEQ_ID = s10.TAG_SEQ_ID
        INNER JOIN t_ipin_file_$${{prj_id}} ipin ON s7.prj_id = ipin.prj_id
            AND s7.file_id = ipin.file_id
        WHERE
            t1.prj_id = ##{{prj_id}}
            AND t1.TAG_ID = 'contains_FlowVariable'
            AND s2.ATTR_NAME = 'xmi:id'
            AND s3.ATTR_NAME = 'columnReference'
            AND s6.ATTR_NAME = 'xmi:id'
            AND s7.ATTR_NAME = 'to_InputPin'
            AND s8.ATTR_NAME = 'name'
            AND s9.ATTR_NAME = 'name'
            AND s10.ATTR_NAME = 'sequenceNumber'
            AND EXISTS (
                SELECT 1
                FROM ais

## ChatGPT 응답:
> 0038_$${{prj_id}} contains
                WHERE contains.PRJ_ID = t1.PRJ_ID
                    AND contains.FILE_ID = t1.FILE_ID
                    AND contains.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                    AND contains.TAG_ID = 'contains_ParameterValue'
            )
            AND EXISTS (
                SELECT 1
                FROM ais0038_$${{prj_id}} contains2
                WHERE contains2.PRJ_ID = t1.PRJ_ID
                    AND contains2.FILE_ID = t1.FILE_ID
                    AND contains2.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                    AND contains2.TAG_ID = 'contains_ValueSpecification'
            )
            AND NOT EXISTS (
                SELECT 1
                FROM ais0038_$${{prj_id}} contains3
                WHERE contains3.PRJ_ID = t1.PRJ_ID
                    AND contains3.FILE_ID = t1.FILE_ID
                    AND contains3.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                    AND contains3.TAG_ID = 'contains_AddStructuralFeatureValueAction'
            )
    ) DAT
    UNION ALL
    SELECT
        DAT.prj_id,
        DAT.file_id,
        DAT.sql_id AS SQL_id,
        CAST(DAT.table_id AS NUMERIC),
        CAST(DAT.col_id AS NUMERIC),
        DAT.prj_id AS call_prj_id,
        DAT.file_id AS call_file_id,
        DAT.call_sql_id AS call_SQL_id,
        CAST(DAT.call_table_id AS NUMERIC) AS call_table_id,
        CAST(DAT.call_col_id AS NUMERIC) AS call_col_id,
        0 AS obj_id,
        0 AS func_id,
        '[owner_undefined]' AS owner_name,
        DAT.table_name || '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS table_name,
        UPPER(DAT.table_name)|| '_$QP' || DAT.prj_id || 'F' || DAT.file_id AS caps_table_name,
        'obj' AS sql_obj_type,
        DAT.col_name AS col_name,
        UPPER(DAT.col_name) AS caps_col_name,
        'N' AS col_value_yn,
        DAT.col_expr AS col_expr,
        DAT.col_name AS col_name_org,
        UPPER(DAT.col_name) AS caps_col_name_org,
        0 AS call_obj_id,
        0 AS call_func_id,
        DAT.call_owner_name AS call_owner_name,
        DAT.call_table_name AS call_table_name,
        UPPER(DAT.call_table_name) AS call_caps_table_name,
        'fil' AS call_sql_obj_type,
        DAT.call_col_name AS call_col_name,
        UPPER(DAT.call_col_name) AS call_caps_col_name,
        'N' AS call_col_value_yn,
        '' AS call_col_expr,
        DAT.call_col_name AS call_col_name_org,
        UPPER(DAT.call_col_name) AS call_caps_col_name_org,
        '[owner_undefined]' AS unique_owner_name,
        DAT.call_owner_name AS call_unique_owner_name,
        '[owner_undefined]' AS unique_owner_tgt_srv_id,
        '[owner_undefined]' AS call_unique_owner_tgt_srv_id,
        1 AS cond_mapping,
        2401 AS data_maker,
        NULL MAPPING_KIND,
        CAST(DAT.col_order_no AS NUMERIC) AS col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS call_col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS adj_col_order_no,
        CAST(DAT.col_order_no AS NUMERIC) AS call_adj_col_order_no,
        '[owner_undefined]' AS SYSTEM_BIZ_ID,
        '[owner_undefined

## ChatGPT 응답:
> ]' AS CALL_SYSTEM_BIZ_ID
    FROM
    (
        SELECT
            t1.prj_id,
            t1.FILE_ID,
            ipin.sql_id AS call_sql_id,
            DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,ipin.table_name) + 80000 AS call_table_id,
            s2.TAG_ATTR_SEQ_ID call_col_id,
            s3.ATTR_VALUE call_col_name,
            '[owner_undefined]' call_owner_name,
            ipin.table_name AS call_table_name,
            s6.ATTR_VALUE AS sql_id,
            DENSE_RANK() OVER (ORDER BY ipin.prj_id,ipin.file_id,s9.ATTR_VALUE) + 70000 AS table_id,
            s2.ATTR_VALUE AS col_id,
            s8.ATTR_VALUE AS col_name,
            (
                SELECT
                    VALUE_S1.ATTR_VALUE
                FROM
                    ais0038_$${{prj_id}} value
                INNER JOIN ais0039_$${{prj_id}} value_s1 ON value.PRJ_ID = VALUE_S1.PRJ_ID
                    AND value.FILE_ID = VALUE_S1.FILE_ID
                    AND value.TAG_SEQ_ID = VALUE_S1.TAG_SEQ_ID
                WHERE
                    t1.PRJ_ID = value.PRJ_ID
                    AND t1.FILE_ID = value.FILE_ID
                    AND value.PARENT_SEQ_ID = t1.TAG_SEQ_ID
                    AND VALUE_S1.ATTR_NAME = 'expression'
            ) AS col_expr,
            s9.ATTR_VALUE AS table_name,
            s10.ATTR_VALUE AS col_order_no
        FROM
            ais0038_$${{prj_id}} t1
        INNER JOIN ais0039_$${{prj_id}} s2 ON t1.PRJ_ID = s2.PRJ_ID
            AND t1.FILE_ID = s2.FILE_ID
            AND t1.TAG_SEQ_ID = s2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s3 ON t1.PRJ_ID = s3.PRJ_ID
            AND t1.FILE_ID = s3.FILE_ID
            AND t1.TAG_SEQ_ID = s3.TAG_SEQ_ID
        INNER JOIN ais0038_$${{prj_id}} t2 ON t1.PRJ_ID = t2.PRJ_ID
            AND t1.FILE_ID = t2.FILE_ID
            AND t1.PARENT_SEQ_ID = t2.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s6 ON t2.PRJ_ID = s6.PRJ_ID
            AND t2.FILE_ID = s6.FILE_ID
            AND t2.TAG_SEQ_ID = s6.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s7 ON t2.PRJ_ID = s7.PRJ_ID
            AND t2.FILE_ID = s7.FILE_ID
            AND t2.TAG_SEQ_ID = s7.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s8 ON t1.PRJ_ID = s8.PRJ_ID
            AND t1.FILE_ID = s8.FILE_ID
            AND t1.TAG_SEQ_ID = s8.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s9 ON t2.PRJ_ID = s9.PRJ_ID
            AND t2.FILE_ID = s9.FILE_ID
            AND t2.TAG_SEQ_ID = s9.TAG_SEQ_ID
        INNER JOIN ais0039_$${{prj_id}} s10 ON t1.PRJ_ID = s10.PRJ_ID
            AND t1

