---
layout: post
title: "MyBatis SQL Query."
date: 2023-02-22
tags: [SQL, Query, MyBatis]
categories: blog
---

## 사용자 입력:
>   <select id="findAnalysisManagements" resultMap="AnalysisManagementMap">
    WITH RECURSIVE BIZLIST(BIZ_ID, UPPER_BIZ_ID) AS   /* QtrackAnalysisManageMent.xml.findAnalysisManagements */
(
  SELECT BIZ_ID, UPPER_BIZ_ID 
    FROM VW_AIS1000 AIS
   WHERE DEL_YN = 'N'
   <if test="(id != '***' and id != 'ALL') or (bizId != '***' and bizId != 0) ">
     AND BIZ_ID IN ( #{bizId} ,(SELECT BIZ_ID 
                                  FROM VW_AIS1000
                                 WHERE BIZ_ENG_NAME = #{id}
                                   AND UPPER_BIZ_ID IS NULL
                               )
                   )
   UNION ALL
  SELECT AIS.BIZ_ID, AIS.UPPER_BIZ_ID 
    FROM VW_AIS1000 AIS, BIZLIST BIZ
   WHERE AIS.BIZ_ID = BIZ.UPPER_BIZ_ID
   </if>
)
    SELECT /* QtrackAnalysisManageMent.findAnalysisManagements */
          A.MM_OBJECT_ID     AS OBJECT_ID
        , A.MM_OBJ_NO1        AS PGM_STATUS
        , A.MM_OBJECT_NAME    AS ORG_FILE_NAME
        , A.MM_OBJ_RMK15      AS ORG_FILE_PATH
        , A.MM_OBJ_IND5       AS FILE_ANLY_YN
        , A.MM_OBJ_RMK5       AS FILE_ANLY_NAME
        , A.MM_OBJ_RMK2       AS LANGTYPE
        , A.MM_OBJ_RMK3       AS BIZ_CODE
        , (SELECT DAT.BIZ_ALIAS
             FROM VW_BIZINFO DAT
            WHERE DAT.BIZ_ID = A.MM_OBJ_RMK3
            LIMIT 1 )         AS BIZ_NAME
        , A.MM_OBJ_RMK4       AS BIZ_PATH
        , A.MM_OBJ_RMK16      AS EXCLUSION_PATH
        , A.MM_OBJ_RMK17      AS PRJ_ID
        , A.MM_OBJ_RMK18      AS FILE_ID
        , A.MM_OBJ_RMK19      AS TS_PRJ_ID
        , A.MM_OBJ_RMK20      AS TS_PRJ_VER_ID
        , A.MM_OBJ_RMK21      AS MIN_FILE_ID
        , A.MM_OBJ_RMK22      AS MAX_FILE_ID
        , A.MM_OBJ_RMK23      AS FILE_TOT_CNT
        , A.MM_OBJ_RMK24      AS FILE_NANLY_CNT
        , A.MM_OBJ_RMK25      AS FILE_YANLY_CNT
        , A.MM_OBJ_RMK26      AS FILE_TOT_CNT_UI
        , A.MM_PROCESS_DT     AS FETCH_DATE
        , A.MM_CREATE_DT      AS CREATE_DATE
        , A.MM_CUSR           AS CREATE_USR
        , CASE WHEN A.MM_OBJ_RMK19 IS NULL THEN
            (SELECT CASE WHEN COUNT(1) = 0 THEN 1 ELSE 0 END AS CHK
              FROM VW_AIS0010 X
             WHERE X.PRJ_ID::text  = A.MM_OBJ_RMK17
               AND X.SRC_FILE_FULL_PATH = A.MM_OBJ_RMK15
               AND X.FILE_ID::text            = A.MM_OBJ_RMK18
             LIMIT 1
            )
          ELSE
            (SELECT CASE WHEN COUNT(1) = 0 THEN 1 ELSE 0 END AS CHK   
              FROM VW_AIS0010 X, AIS8000 X1
             WHERE X.PRJ_ID         = X1.PRJ_ID
               AND X.FILE_ID        = X1.FILE_ID
               AND X1.PRJ_ID::text        = A.MM_OBJ_RMK17
               AND X1.TS_PRJ_ID     = A.MM_OBJ_RMK19

               AND X.FILE_ID::text BETWEEN A.MM_OBJ_RMK21 AND A.MM_OBJ_RMK22
             LIMIT 1
             )
           END                AS PROCESS_CHK
     FROM AIS8101 A
     WHERE A.MM_CLASS_ID = 3002
       <if test="fileName != null and fileName != ''">
       AND UPPER(A.MM_OBJECT_NAME) LIKE '%' || UPPER(#{fileName}) || '%'
       </if>
    <if test="extCd != null and extCd != ''">
       AND A.MM_OBJ_RMK1 = #{extCd}
       </if>
    <if test="analYN != null and analYN != ''">
        AND A.MM_OBJ_IND5 LIKE '%' || #{analYN} || '%'
       </if>
    <if test="programStatus != null and programStatus != ''">
       AND A.MM_OBJ_NO1 = #{programStatus}
       </if>
    <if test="langTypes != null and langTypes != ''">
       AND REPLACE(SUBSTR(A.MM_OBJ_RMK2,INSTR(A.MM_OBJ_RMK2,'[')+1),']','') = #{langTypes}
       </if>
    <if test="prjId != null and prjId != '' and prjId != -1">
       AND A.MM_OBJ_RMK17 = #{prjId}
       </if>
    <if test="id != null and id != '***' and id != 'ALL' ">
       AND A.MM_OBJ_RMK17 IN (SELECT PRJ_ID::text
                           FROM  AIS1001
                          WHERE define_owner_name like #{id} || '%'
                          UNION ALL
                         SELECT DISTINCT A1.REL_PRJ_ID::text
                           FROM  AIS1001 A2, AIS1002 A1
                          WHERE define_owner_name like #{id} || '%'
                            AND A1.PRJ_ID = A2.PRJ_ID
                       )
        </if>
        AND A.MM_OBJ_RMK17 IN (
                               SELECT PRJ_ID::text
                                 FROM AIS1001
                                WHERE DEL_YN = 'N'
                                  AND UPPER_BIZ_ID IN ( SELECT BIZ_ID
                                                          FROM BIZLIST AIS
                                                      )
                                UNION
                               SELECT A1.REL_PRJ_ID::text AS PRJ_ID
                                 FROM AIS1002 A1,
                                      AIS1001 A2,
                                      BIZLIST A3
                                WHERE A1.PRJ_ID = A2.PRJ_ID
                                  AND A2.UPPER_BIZ_ID = A3.BIZ_ID
                              )
  </select>   이 쿼리문을 oracle 쿼리로 바꿔줘

## ChatGPT 응답:
> This code is a MyBatis SQL query, a Java-based persistence framework that uses XML files to define SQL statements. The query selects data from the "AIS8101" table and joins it with the "VW_AIS1000" and "VW_BIZINFO" views using recursive SQL to traverse the hierarchy defined by the "BIZ_ID" and "UPPER_BIZ_ID" columns.

The query takes several input parameters, such as "fileName," "extCd," "analYN," "programStatus," "langTypes," "prjId," and "id," which are used to filter the results. Some of these parameters are optional, and the query uses conditional logic to include or exclude the corresponding filter conditions based on whether they are provided.

The resulting dataset includes several columns, such as "OBJECT_ID," "PGM_STATUS," "ORG_FILE_NAME," "ORG_FILE_PATH," "FILE_ANLY_YN," and many others, that provide information about the analysis management objects in the system.

## 사용자 입력:
> convert oracle query

## ChatGPT 응답:
> The provided code is a MyBatis mapper XML file for executing a SQL query against a PostgreSQL database. To convert it to an Oracle query, the following changes should be made:

1. Replace the recursive `WITH` clause with a hierarchical query using the `CONNECT BY` clause.
2. Replace `LIMIT 1` with `ROWNUM = 1`.
3. Replace `BETWEEN` with `>=` and `<=`.
4. Use the `REGEXP_LIKE` function instead of the `LIKE` operator for pattern matching.

Here's the converted query:

```
<select id="findAnalysisManagements" resultMap="AnalysisManagementMap">
    SELECT /* QtrackAnalysisManageMent.findAnalysisManagements */
          A.MM_OBJECT_ID     AS OBJECT_ID
        , A.MM_OBJ_NO1        AS PGM_STATUS
        , A.MM_OBJECT_NAME    AS ORG_FILE_NAME
        , A.MM_OBJ_RMK15      AS ORG_FILE_PATH
        , A.MM_OBJ_IND5       AS FILE_ANLY_YN
        , A.MM_OBJ_RMK5       AS FILE_ANLY_NAME
        , A.MM_OBJ_RMK2       AS LANGTYPE
        , A.MM_OBJ_RMK3       AS BIZ_CODE
        , (SELECT DAT.BIZ_ALIAS
             FROM VW_BIZINFO DAT
            WHERE DAT.BIZ_ID = A.MM_OBJ_RMK3
            AND ROWNUM = 1)    AS BIZ_NAME
        , A.MM_OBJ_RMK4       AS BIZ_PATH
        , A.MM_OBJ_RMK16      AS EXCLUSION_PATH
        , A.MM_OBJ_RMK17      AS PRJ_ID
        , A.MM_OBJ_RMK18      AS FILE_ID
        , A.MM_OBJ_RMK19      AS TS_PRJ_ID
        , A.MM_OBJ_RMK20      AS TS_PRJ_VER_ID
        , A.MM_OBJ_RMK21      AS MIN_FILE_ID
        , A.MM_OBJ_RMK22      AS MAX_FILE_ID
        , A.MM_OBJ_RMK23      AS FILE_TOT_CNT
        , A.MM_OBJ_RMK24      AS FILE_NANLY_CNT
        , A.MM_OBJ_RMK25      AS FILE_YANLY_CNT
        , A.MM_OBJ_RMK26      AS FILE_TOT_CNT_UI
        , A.MM_PROCESS_DT     AS FETCH_DATE
        , A.MM_CREATE_DT      AS CREATE_DATE
        , A.MM_CUSR           AS CREATE_USR
        , CASE WHEN A.MM_OBJ_RMK19 IS NULL THEN
            (SELECT CASE WHEN COUNT(1) = 0 THEN 1 ELSE 0 END AS CHK
              FROM VW_AIS0010 X
             WHERE X.PRJ_ID = TO_NUMBER(A.MM_OBJ_RMK17)
               AND X.SRC_FILE_FULL_PATH = A.MM_OBJ_RMK15
               AND X.FILE_ID = TO_NUMBER(A.MM_OBJ_RMK18)
             AND ROWNUM = 1
            )
          ELSE
            (SELECT CASE WHEN COUNT(1) = 0 THEN 1 ELSE 0 END AS CHK   
              FROM VW_AIS0010 X, AIS8000 X1
             WHERE X.PRJ_ID = X1.PRJ_ID
               AND X.FILE_ID = X1.FILE_ID
               AND X1.PRJ_ID = TO_NUMBER(A.MM_OBJ_RMK17)
               AND X1.TS_PRJ_ID = A.MM_OBJ_RMK19
               AND TO_NUMBER(X.FILE_ID) >= TO_NUMBER(A.MM_OBJ_RMK21)
               AND TO_NUMBER(X.FILE_ID) <= TO_NUMBER(A.MM_OBJ_RMK22)
             AND ROWNUM = 1
             )
           END                AS PROCESS_CHK


## 사용자 입력:
> WITH RECURSIVE 는 오라클에서 지원하지 않나?

## ChatGPT 응답:
> WITH RECURSIVE는 오라클에서 지원되지 않는 기능입니다. 이 기능은 주로 PostgreSQL, MySQL 등의 데이터베이스 시스템에서 사용됩니다. 

오라클에서는 WITH 절을 사용하여 공통 테이블 표현식(Common Table Expression, CTE)을 구현할 수 있습니다. CTE는 쿼리에서 재사용 가능한 임시 테이블로서, 복잡한 쿼리를 단순화하고 가독성을 높이는 데 유용합니다. 하지만, CTE를 사용하여 계층적인 쿼리를 작성하는 것은 제한적입니다. 대신 CONNECT BY PRIOR 절을 사용하여 계층적인 쿼리를 작성할 수 있습니다.

