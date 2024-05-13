---
layout: post
title: "SQL query for cell data."
date: 2023-03-27
tags: [SQL, for, query, data, cell]
categories: blog
---

## 사용자 입력:
>     <select id="findShellLineageCells" resultMap="dwCellMap">
        SELECT DISTINCT /*Mxgraph.findShellLineageCells*/
               (  SELECT A.SYSTEM_TGT_SRV_ID
                    FROM AIS1003 A, AIS1011 B
                   WHERE A.MAP_TYPE = '02'
                     AND A.PRJ_ID = DTT.PRJ_ID
                     AND B.JOB_ID = A.JOB_ID
                     AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
               ) AS SYSTEM_ID
             , DTT.PRJ_ID
             , DTT.FILE_ID
             , DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID
             <!--, NVL((SELECT B.TS_BLOCK_NM FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME-->
             , NVL((SELECT TO_CHAR(B.TS_PRJ_ID) FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME
             , (SELECT MAX(COLOR)
                  FROM AIS3010
                 WHERE SYSTEM_TGT_SRV_ID = (  SELECT A.SYSTEM_TGT_SRV_ID
                                                FROM AIS1003 A, AIS1011 B
                                               WHERE A.MAP_TYPE = '02'
                                                 AND A.PRJ_ID = DTT.PRJ_ID
                                                 AND B.JOB_ID = A.JOB_ID
                                                 AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
                                           )
               )                                                             AS COLOR
             , (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD AND ROWNUM = 1) AS LANG_CD
             , DTT.EXT_CD AS EXT_CD
             , (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND ROWNUM = 1 AND DTT.DATA_GUBUN = 'CALLED') AS CALLED_CHK
             , MAX(DTT.ISLEAF)  OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF
             , MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
         FROM (
                SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
                        'MYFILE' AS DATA_GUBUN,
                        0 AS ISLEAF, 0 AS OUTLEAF
                     , ORG_FILE_PATH
                  FROM VW_AIS0010_UI
                 WHERE PRJ_ID = #{prjId}
                   AND FILE_ID = #{fileId}
                UNION
                SELECT DISTINCT
                       DT.PRJ_ID
                     , DT.FILE_ID
                     , B.CAPS_SRC_FILE_NAME AS FILE_NAME
                     , B.EXT_CD AS EXT_CD
                     , DT.DATA_GUBUN
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.FILE_ID       = DT.FILE_ID
                           AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                           AND ROWNUM = 1) AS ISLEAF
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.CALL_FILE_ID  = DT.FILE_ID
                           AND D.FILE_ID      != DT.TEMP_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                           AND ROWNUM = 1) AS OUTLEAF
                     , B.ORG_FILE_PATH
                  FROM (
               SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID    ELSE DT.CALL_PRJ_ID    END AS PRJ_ID
                     , CASE WHEN T.NO = 1 THEN DT.FILE_ID   ELSE DT.CALL_FILE_ID   END AS FILE_ID
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 'MYFILE' ELSE 'CALLING' END AS DATA_GUBUN
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 0 ELSE CONN_ISLEAF END AS CONN_ISLEAF
                     , DT.FILE_ID         AS TEMP_FILE_ID
                     , DT.CALL_FILE_ID    AS TEMP_CALL_FILE_ID
                 FROM (
                        SELECT DISTINCT
                                A.PRJ_ID
                              , A.FILE_ID
                              , A.CALL_PRJ_ID
                              , A.CALL_FILE_ID
                              , A.CALL_TYPE
                              , CONNECT_BY_ROOT(A.FILE_ID) AS ROOT_FILE_ID
                              , CONNECT_BY_ISLEAF AS CONN_ISLEAF
                                   FROM AIS0083 A
                          START WITH A.PRJ_ID  = #{prjId}
                                 AND A.FILE_ID = #{fileId}
                       CONNECT BY nocycle PRIOR A.CALL_PRJ_ID = A.PRJ_ID
                           AND     PRIOR A.CALL_FILE_ID = A.FILE_ID
                            <if test="depth gt -1">
                            AND     LEVEL &lt;= #{depth}
                           </if>
                       ) DT
                     , COPY_T T
                WHERE T.NO IN(1,2)
                ) DT,
                VW_AIS0010_UI B
                WHERE B.PRJ_ID = DT.PRJ_ID
                AND B.FILE_ID = DT.FILE_ID
                AND (CASE WHEN  EXT_CD = 'SQL' AND B.CAPS_SRC_FILE_NAME LIKE '%' || (SELECT VAR_INIT_VAL
                                                                                     FROM AIS0014 A
                                                                                    WHERE A.PRJ_ID   = #{prjId}
                                                                                      AND A.FILE_ID  = #{fileId}
                                                                                      AND A.VAR_NAME = (SELECT MM_ABBR_NAME FROM AIS8101 WHERE MM_CLASS_ID = 1004)
                                                                                  ) || '%'
                                               THEN 1
                          WHEN EXT_CD != 'SQL' THEN 1
                          ELSE 0 END) = 1
                UNION
                SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID     ELSE DT.CALL_PRJ_ID     END AS PRJ_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_ID    ELSE DT.CALL_FILE_ID    END AS FILE_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_NAME  ELSE DT.CALL_FILE_NAME  END AS NAME
                      , CASE WHEN T.NO = 1 THEN DT.EXT_CD     ELSE DT.CALL_EXT_CD     END AS EXT_CD
                      , CASE WHEN T.NO = 1 THEN 'CALLED'     ELSE 'MYFILE'         END AS DATA_GUBUN
                      , 0 AS ISLEAF
                      , 0 AS OUTLEAF
                      , DT.ORG_FILE_PATH
                 FROM (
                        SELECT DISTINCT
                               A.PRJ_ID
                             , A.FILE_ID
                             , A.FILE_NAME
                             , A.CALL_PRJ_ID
                             , A.CALL_FILE_ID
                             , A.CALL_FILE_NAME
                             , A.CALL_TYPE
                             , A.EXT_CD
                             , A.CALL_EXT_CD
                             , A.ORG_FILE_PATH
                         FROM (
                                SELECT A.PRJ_ID, A.FILE_ID, B.CAPS_SRC_FILE_NAME AS FILE_NAME
                                      , A.CALL_PRJ_ID, A.CALL_FILE_ID, C.CAPS_SRC_FILE_NAME AS CALL_FILE_NAME
                                      , A.CALL_TYPE
                                      , B.EXT_CD
                                      , C.EXT_CD AS CALL_EXT_CD
                                      , B.ORG_FILE_PATH
                                  FROM AIS0083 A
                                      , VW_AIS0010_UI B
                                      , VW_AIS0010_UI C
                                  WHERE A.PRJ_ID = B.PRJ_ID
                                    AND A.FILE_ID = B.FILE_ID
                                    AND A.CALL_PRJ_ID = C.PRJ_ID
                                    AND A.CALL_FILE_ID = C.FILE_ID
                                    AND A.CALL_PRJ_ID = #{prjId}
                                    AND A.CALL_FILE_ID = #{fileId}
                              ) A
                        ) DT
                        , COPY_T T
                    WHERE T.NO IN (1,2)
               ) DTT
               ORDER BY DTT.FILE_ID
    </select> 

## ChatGPT 응답:
> This appears to be a SQL query written in XML syntax. The query selects various columns from several tables, including AIS1003, AIS1011, AIS8000, AIS3010, AIS2102, AIS0083, AIS0014, and AIS8101, as well as VW_AIS0010_UI, and generates a unique cell ID for each row based on the project ID and file ID. The query also includes several subqueries and functions to retrieve additional information such as the system ID, cell name, color, language code, and whether the cell is a leaf or has any called checks. The query uses several joins, unions, and conditions to filter and manipulate the data.

## 사용자 입력:
> 위 코드는 mybatis xml형식인데, oracle 쿼리이다. 이것을 postgresql로 바꿔줘

## ChatGPT 응답:
> 다음은 PostgreSQL용으로 변경한 쿼리입니다.

```sql
SELECT DISTINCT
    (SELECT A.SYSTEM_TGT_SRV_ID
        FROM AIS1003 A
        INNER JOIN AIS1011 B ON A.JOB_ID = B.JOB_ID
        WHERE A.MAP_TYPE = '02'
            AND A.PRJ_ID = DTT.PRJ_ID
            AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
    ) AS SYSTEM_ID,
    DTT.PRJ_ID,
    DTT.FILE_ID,
    DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID,
    COALESCE((SELECT TO_CHAR(B.TS_PRJ_ID) FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME,
    (SELECT MAX(COLOR) FROM AIS3010 WHERE SYSTEM_TGT_SRV_ID = (
        SELECT A.SYSTEM_TGT_SRV_ID
        FROM AIS1003 A
        INNER JOIN AIS1011 B ON A.JOB_ID = B.JOB_ID
        WHERE A.MAP_TYPE = '02'
            AND A.PRJ_ID = DTT.PRJ_ID
            AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
    )) AS COLOR,
    (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD AND ROW_NUMBER() OVER() = 1) AS LANG_CD,
    DTT.EXT_CD,
    (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND ROW_NUMBER() OVER() = 1 AND DTT.DATA_GUBUN = 'CALLED') AS CALLED_CHK,
    MAX(DTT.ISLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF,
    MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
FROM (
    SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
            'MYFILE' AS DATA_GUBUN,
            0 AS ISLEAF, 0 AS OUTLEAF,
            ORG_FILE_PATH
        FROM VW_AIS0010_UI
        WHERE PRJ_ID = #{prjId}
            AND FILE_ID = #{fileId}
    UNION
    SELECT DISTINCT
        DT.PRJ_ID,
        DT.FILE_ID,
        B.CAPS_SRC_FILE_NAME AS FILE_NAME,
        B.EXT_CD AS EXT_CD,
        DT.DATA_GUBUN,
        (
            SELECT COUNT(*)
            FROM AIS0083 D
            WHERE D.PRJ_ID = DT.PRJ_ID
                AND D.FILE_ID = DT.FILE_ID
                AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                AND DT.CONN_ISLEAF = 1
                AND ROW_NUMBER() OVER() = 1
        ) AS ISLEAF,
        (
            SELECT COUNT(*)
            FROM AIS0083 D
            WHERE D.PRJ_ID = DT.PRJ_ID
                AND D.CALL_FILE_ID = DT.FILE_ID
                AND D.FILE_ID != DT.TEMP_FILE_ID
                AND DT.CONN_ISLEAF = 1
                AND ROW_NUMBER() OVER() = 1
        ) AS OUTLEAF,
        B.ORG_FILE_PATH
    FROM (
        SELECT
            CASE WHEN T.NO = 1 THEN DT

## 사용자 입력:
>   <select id="findShellLineageCells" resultMap="dwCellMap">
        SELECT DISTINCT /* Mxgraph.xml.findShellLineageCells */
               (  SELECT A.SYSTEM_TGT_SRV_ID
                    FROM AIS1003 A, AIS1011 B
                   WHERE A.MAP_TYPE = '02'
                     AND A.PRJ_ID = DTT.PRJ_ID
                     AND B.JOB_ID = A.JOB_ID
                     AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
               ) AS SYSTEM_ID
             , DTT.PRJ_ID
             , DTT.FILE_ID
             , DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID
             
             , COALESCE((SELECT B.TS_PRJ_ID::text FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME
             , (SELECT MAX(COLOR)
                  FROM AIS3010
                 WHERE SYSTEM_TGT_SRV_ID = (  SELECT A.SYSTEM_TGT_SRV_ID
                                                FROM AIS1003 A, AIS1011 B
                                               WHERE A.MAP_TYPE = '02'
                                                 AND A.PRJ_ID = DTT.PRJ_ID
                                                 AND B.JOB_ID = A.JOB_ID
                                                 AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
                                           )
               )                                                             AS COLOR
             , (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD LIMIT 1) AS LANG_CD
             , DTT.EXT_CD AS EXT_CD
             , (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' LIMIT 1) AS CALLED_CHK
             , MAX(DTT.ISLEAF)  OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF
             , MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
         FROM (
                SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
                        'MYFILE' AS DATA_GUBUN,
                        0 AS ISLEAF, 0 AS OUTLEAF
                     , ORG_FILE_PATH
                  FROM VW_AIS0010_UI
                 WHERE PRJ_ID = #{prjId}
                   AND FILE_ID = #{fileId}
                UNION
                SELECT DISTINCT
                       DT.PRJ_ID
                     , DT.FILE_ID
                     , B.CAPS_SRC_FILE_NAME AS FILE_NAME
                     , B.EXT_CD AS EXT_CD
                     , DT.DATA_GUBUN
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.FILE_ID       = DT.FILE_ID
                           AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                         LIMIT 1) AS ISLEAF
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.CALL_FILE_ID  = DT.FILE_ID
                           AND D.FILE_ID      != DT.TEMP_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                         LIMIT 1) AS OUTLEAF
                     , B.ORG_FILE_PATH
                  FROM (
               SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID    ELSE DT.CALL_PRJ_ID    END AS PRJ_ID
                     , CASE WHEN T.NO = 1 THEN DT.FILE_ID   ELSE DT.CALL_FILE_ID   END AS FILE_ID
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 'MYFILE' ELSE 'CALLING' END AS DATA_GUBUN
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 0 ELSE CONN_ISLEAF END AS CONN_ISLEAF
                     , DT.FILE_ID         AS TEMP_FILE_ID
                     , DT.CALL_FILE_ID    AS TEMP_CALL_FILE_ID
                 FROM (
      WITH RECURSIVE RE_TABLE(PRJ_ID, FILE_ID, CALL_PRJ_ID, CALL_FILE_ID, CALL_TYPE, LV) as 
      (
       SELECT A.PRJ_ID
         , A.FILE_ID
         , A.CALL_PRJ_ID
         , A.CALL_FILE_ID
         , A.CALL_TYPE
         , 1 as LV
         FROM AIS0083 A
        WHERE A.PRJ_ID  = #{prjId}
       AND A.FILE_ID = #{fileId}
        UNION ALL
       SELECT A.PRJ_ID
         , A.FILE_ID
         , A.CALL_PRJ_ID
         , A.CALL_FILE_ID
         , A.CALL_TYPE
         , B.LV+1 as LV
        FROM AIS0083 A, RE_TABLE B
       WHERE A.PRJ_ID = B.CALL_PRJ_ID
         AND A.FILE_ID = B.CALL_FILE_ID
         <if test="depth gt -1">
         AND B.LV &lt; #{depth}
         </if>
      )
      SELECT DISTINCT B.*
          , #{fileId} AS ROOT_FILE_ID
          , CASE WHEN (SELECT count(*) 
                         FROM RE_TABLE A 
                        WHERE A.PRJ_ID=B.CALL_PRJ_ID
                          AND A.FILE_ID = B.CALL_FILE_ID
                          AND A.LV &gt; B.LV) &gt; 0
                 THEN 0 
                 ELSE 1 
                 END AS CONN_ISLEAF 
           FROM RE_TABLE B
                       ) DT
                     , COPY_T T
                WHERE T.NO IN(1,2)
                ) DT,
                VW_AIS0010_UI B
                WHERE B.PRJ_ID = DT.PRJ_ID
                AND B.FILE_ID = DT.FILE_ID
                AND (CASE WHEN  EXT_CD = 'SQL' AND B.CAPS_SRC_FILE_NAME LIKE '%' || (SELECT VAR_INIT_VAL
                                                                                     FROM AIS0014 A
                                                                                    WHERE A.PRJ_ID   = #{prjId}
                                                                                      AND A.FILE_ID  = #{fileId}
                                                                                      AND A.VAR_NAME = (SELECT MM_ABBR_NAME FROM AIS8101 WHERE MM_CLASS_ID = 1004)
                                                                                  ) || '%'
                                               THEN 1
                          WHEN EXT_CD != 'SQL' THEN 1
                          ELSE 0 END) = 1
                UNION
                SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID     ELSE DT.CALL_PRJ_ID     END AS PRJ_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_ID    ELSE DT.CALL_FILE_ID    END AS FILE_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_NAME  ELSE DT.CALL_FILE_NAME  END AS NAME
                      , CASE WHEN T.NO = 1 THEN DT.EXT_CD     ELSE DT.CALL_EXT_CD     END AS EXT_CD
                      , CASE WHEN T.NO = 1 THEN 'CALLED'     ELSE 'MYFILE'         END AS DATA_GUBUN
                      , 0 AS ISLEAF
                      , 0 AS OUTLEAF
                      , DT.ORG_FILE_PATH
                 FROM (
                        SELECT DISTINCT
                               A.PRJ_ID
                             , A.FILE_ID
                             , A.FILE_NAME
                             , A.CALL_PRJ_ID
                             , A.CALL_FILE_ID
                             , A.CALL_FILE_NAME
                             , A.CALL_TYPE
                             , A.EXT_CD
                             , A.CALL_EXT_CD
                             , A.ORG_FILE_PATH
                         FROM (
                                SELECT A.PRJ_ID, A.FILE_ID, B.CAPS_SRC_FILE_NAME AS FILE_NAME
                                      , A.CALL_PRJ_ID, A.CALL_FILE_ID, C.CAPS_SRC_FILE_NAME AS CALL_FILE_NAME
                                      , A.CALL_TYPE
                                      , B.EXT_CD
                                      , C.EXT_CD AS CALL_EXT_CD
                                      , B.ORG_FILE_PATH
                                  FROM AIS0083 A
                                      , VW_AIS0010_UI B
                                      , VW_AIS0010_UI C
                                  WHERE A.PRJ_ID = B.PRJ_ID
                                    AND A.FILE_ID = B.FILE_ID
                                    AND A.CALL_PRJ_ID = C.PRJ_ID
                                    AND A.CALL_FILE_ID = C.FILE_ID
                                    AND A.CALL_PRJ_ID = #{prjId}
                                    AND A.CALL_FILE_ID = #{fileId}
                              ) A
                        ) DT
                        , COPY_T T
                    WHERE T.NO IN (1,2)
               ) DTT
               ORDER BY DTT.FILE_ID
    </select>

## ChatGPT 응답:
> Here's the Postgresql version of the given Oracle query in Mybatis XML format:

```xml
<select id="findShellLineageCells" resultMap="dwCellMap">
  SELECT DISTINCT -- Mxgraph.xml.findShellLineageCells
         (SELECT A.SYSTEM_TGT_SRV_ID
          FROM AIS1003 A, AIS1011 B
          WHERE A.MAP_TYPE = '02'
            AND A.PRJ_ID = DTT.PRJ_ID
            AND B.JOB_ID = A.JOB_ID
            AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
         ) AS SYSTEM_ID,
         DTT.PRJ_ID,
         DTT.FILE_ID,
         CONCAT(DTT.PRJ_ID, 'F', DTT.FILE_ID) AS CELL_ID,
         COALESCE((SELECT B.TS_PRJ_ID::text FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME,
         (SELECT MAX(COLOR) FROM AIS3010 WHERE SYSTEM_TGT_SRV_ID = (SELECT A.SYSTEM_TGT_SRV_ID
                                                                     FROM AIS1003 A, AIS1011 B
                                                                     WHERE A.MAP_TYPE = '02'
                                                                       AND A.PRJ_ID = DTT.PRJ_ID
                                                                       AND B.JOB_ID = A.JOB_ID
                                                                       AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%')
         ) AS COLOR,
         (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD LIMIT 1) AS LANG_CD,
         DTT.EXT_CD,
         (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' LIMIT 1) AS CALLED_CHK,
         MAX(DTT.ISLEAF)  OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF,
         MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
  FROM (
         SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
                'MYFILE' AS DATA_GUBUN,
                0 AS ISLEAF, 0 AS OUTLEAF,
                ORG_FILE_PATH
         FROM VW_AIS0010_UI
         WHERE PRJ_ID = #{prjId}
           AND FILE_ID = #{fileId}
         UNION
         SELECT DISTINCT
                DT.PRJ_ID,
                DT.FILE_ID,
                B.CAPS_SRC_FILE_NAME AS FILE_NAME,
                B.EXT_CD AS EXT_CD,
                DT.DATA_GUBUN,
                (SELECT COUNT(*)
                 FROM AIS0083 D
                 WHERE D.PRJ_ID        = DT.PRJ_ID
                   AND D.FILE_ID       = DT.FILE_ID
                   AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                   AND DT.CONN_ISLEAF = 1
                 LIMIT 1) AS ISLEAF,
                (SELECT COUNT(*)
                 FROM AIS0083 D
                 WHERE D.PRJ_ID        = DT.PRJ_ID
                   AND D.CALL_FILE_ID  = DT.FILE_ID
                   AND D.FILE_ID      != DT.TEMP_FILE_ID
                   AND DT.CONN_ISLEAF = 1
                 LIMIT 1) AS OUTLEAF,
                B.ORG_FILE_PATH
         FROM (
                SELECT CASE WHEN T.NO = 1

## 사용자 입력:
> 계속

## ChatGPT 응답:
> postgresql 쿼리로 변환한 결과입니다.

```xml
<select id="findShellLineageCells" resultMap="dwCellMap">
    SELECT DISTINCT -- Mxgraph.xml.findShellLineageCells
        (
            SELECT A.SYSTEM_TGT_SRV_ID
            FROM AIS1003 A, AIS1011 B
            WHERE A.MAP_TYPE = '02'
                AND A.PRJ_ID = DTT.PRJ_ID
                AND B.JOB_ID = A.JOB_ID
                AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
        ) AS SYSTEM_ID,
        DTT.PRJ_ID,
        DTT.FILE_ID,
        DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID,
        COALESCE(
            (
                SELECT B.TS_PRJ_ID::text
                FROM AIS8000 B
                WHERE B.PRJ_ID = DTT.PRJ_ID
                    AND B.FILE_ID = DTT.FILE_ID
            ),
            DTT.NAME
        ) AS NAME,
        (
            SELECT MAX(COLOR)
            FROM AIS3010
            WHERE SYSTEM_TGT_SRV_ID = (
                SELECT A.SYSTEM_TGT_SRV_ID
                FROM AIS1003 A, AIS1011 B
                WHERE A.MAP_TYPE = '02'
                    AND A.PRJ_ID = DTT.PRJ_ID
                    AND B.JOB_ID = A.JOB_ID
                    AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
            )
        ) AS COLOR,
        (
            SELECT B.LANG_CD
            FROM AIS2102 B
            WHERE B.EXT_CD = DTT.EXT_CD
            LIMIT 1
        ) AS LANG_CD,
        DTT.EXT_CD AS EXT_CD,
        (
            SELECT COUNT(B.FILE_ID) AS CALLED_CHK
            FROM AIS0083 B
            WHERE B.CALL_PRJ_ID = DTT.PRJ_ID
                AND B.CALL_FILE_ID = DTT.FILE_ID
                AND DTT.DATA_GUBUN = 'CALLED'
            LIMIT 1
        ) AS CALLED_CHK,
        MAX(DTT.ISLEAF) OVER (PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF,
        MAX(DTT.OUTLEAF) OVER (PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
    FROM (
        SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
            'MYFILE' AS DATA_GUBUN,
            0 AS ISLEAF, 0 AS OUTLEAF,
            ORG_FILE_PATH
        FROM VW_AIS0010_UI
        WHERE PRJ_ID = #{prjId}
            AND FILE_ID = #{fileId}
        UNION
        SELECT DISTINCT
            DT.PRJ_ID,
            DT.FILE_ID,
            B.CAPS_SRC_FILE_NAME AS FILE_NAME,
            B.EXT_CD AS EXT_CD,
            DT.DATA_GUBUN,
            (
                SELECT COUNT(*)
                FROM AIS0083 D
                WHERE D.PRJ_ID = DT.PRJ_ID
                    AND D.FILE_ID = DT.FILE_ID
                    AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                    AND DT.CONN_ISLEAF = 1
                LIMIT 1
            ) AS ISLEAF,
            (
                SELECT COUNT(*)
                FROM AIS0083 D
                WHERE D.PRJ_ID = DT.PRJ_ID
                    AND D.CALL_FILE_ID = DT.FILE_ID
                    AND D.FILE_ID != DT.TEMP_FILE_ID
                   

## 사용자 입력:
> <select id="findShellLineageCells" resultMap="dwCellMap">
        SELECT DISTINCT /* Mxgraph.xml.findShellLineageCells */
               (  SELECT A.SYSTEM_TGT_SRV_ID
                    FROM AIS1003 A, AIS1011 B
                   WHERE A.MAP_TYPE = '02'
                     AND A.PRJ_ID = DTT.PRJ_ID
                     AND B.JOB_ID = A.JOB_ID
                     AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
               ) AS SYSTEM_ID
             , DTT.PRJ_ID
             , DTT.FILE_ID
             , DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID
             
             , COALESCE((SELECT B.TS_PRJ_ID::text FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME
             , (SELECT MAX(COLOR)
                  FROM AIS3010
                 WHERE SYSTEM_TGT_SRV_ID = (  SELECT A.SYSTEM_TGT_SRV_ID
                                                FROM AIS1003 A, AIS1011 B
                                               WHERE A.MAP_TYPE = '02'
                                                 AND A.PRJ_ID = DTT.PRJ_ID
                                                 AND B.JOB_ID = A.JOB_ID
                                                 AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
                                           )
               )                                                             AS COLOR
             , (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD LIMIT 1) AS LANG_CD
             , DTT.EXT_CD AS EXT_CD
             , (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' LIMIT 1) AS CALLED_CHK
             , MAX(DTT.ISLEAF)  OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF
             , MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
         FROM (
                SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
                        'MYFILE' AS DATA_GUBUN,
                        0 AS ISLEAF, 0 AS OUTLEAF
                     , ORG_FILE_PATH
                  FROM VW_AIS0010_UI
                 WHERE PRJ_ID = #{prjId}
                   AND FILE_ID = #{fileId}
                UNION
                SELECT DISTINCT
                       DT.PRJ_ID
                     , DT.FILE_ID
                     , B.CAPS_SRC_FILE_NAME AS FILE_NAME
                     , B.EXT_CD AS EXT_CD
                     , DT.DATA_GUBUN
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.FILE_ID       = DT.FILE_ID
                           AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                         LIMIT 1) AS ISLEAF
                     , (SELECT COUNT(*) CNT
                          FROM AIS0083 D
                         WHERE D.PRJ_ID        = DT.PRJ_ID
                           AND D.CALL_FILE_ID  = DT.FILE_ID
                           AND D.FILE_ID      != DT.TEMP_FILE_ID
                           AND DT.CONN_ISLEAF = 1
                         LIMIT 1) AS OUTLEAF
                     , B.ORG_FILE_PATH
                  FROM (
               SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID    ELSE DT.CALL_PRJ_ID    END AS PRJ_ID
                     , CASE WHEN T.NO = 1 THEN DT.FILE_ID   ELSE DT.CALL_FILE_ID   END AS FILE_ID
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 'MYFILE' ELSE 'CALLING' END AS DATA_GUBUN
                     , CASE WHEN T.NO = 1 AND DT.FILE_ID = DT.ROOT_FILE_ID THEN 0 ELSE CONN_ISLEAF END AS CONN_ISLEAF
                     , DT.FILE_ID         AS TEMP_FILE_ID
                     , DT.CALL_FILE_ID    AS TEMP_CALL_FILE_ID
                 FROM (
      WITH RECURSIVE RE_TABLE(PRJ_ID, FILE_ID, CALL_PRJ_ID, CALL_FILE_ID, CALL_TYPE, LV) as 
      (
       SELECT A.PRJ_ID
         , A.FILE_ID
         , A.CALL_PRJ_ID
         , A.CALL_FILE_ID
         , A.CALL_TYPE
         , 1 as LV
         FROM AIS0083 A
        WHERE A.PRJ_ID  = #{prjId}
       AND A.FILE_ID = #{fileId}
        UNION ALL
       SELECT A.PRJ_ID
         , A.FILE_ID
         , A.CALL_PRJ_ID
         , A.CALL_FILE_ID
         , A.CALL_TYPE
         , B.LV+1 as LV
        FROM AIS0083 A, RE_TABLE B
       WHERE A.PRJ_ID = B.CALL_PRJ_ID
         AND A.FILE_ID = B.CALL_FILE_ID
         <if test="depth gt -1">
         AND B.LV &lt; #{depth}
         </if>
      )
      SELECT DISTINCT B.*
          , #{fileId} AS ROOT_FILE_ID
          , CASE WHEN (SELECT count(*) 
                         FROM RE_TABLE A 
                        WHERE A.PRJ_ID=B.CALL_PRJ_ID
                          AND A.FILE_ID = B.CALL_FILE_ID
                          AND A.LV &gt; B.LV) &gt; 0
                 THEN 0 
                 ELSE 1 
                 END AS CONN_ISLEAF 
           FROM RE_TABLE B
                       ) DT
                     , COPY_T T
                WHERE T.NO IN(1,2)
                ) DT,
                VW_AIS0010_UI B
                WHERE B.PRJ_ID = DT.PRJ_ID
                AND B.FILE_ID = DT.FILE_ID
                AND (CASE WHEN  EXT_CD = 'SQL' AND B.CAPS_SRC_FILE_NAME LIKE '%' || (SELECT VAR_INIT_VAL
                                                                                     FROM AIS0014 A
                                                                                    WHERE A.PRJ_ID   = #{prjId}
                                                                                      AND A.FILE_ID  = #{fileId}
                                                                                      AND A.VAR_NAME = (SELECT MM_ABBR_NAME FROM AIS8101 WHERE MM_CLASS_ID = 1004)
                                                                                  ) || '%'
                                               THEN 1
                          WHEN EXT_CD != 'SQL' THEN 1
                          ELSE 0 END) = 1
                UNION
                SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID     ELSE DT.CALL_PRJ_ID     END AS PRJ_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_ID    ELSE DT.CALL_FILE_ID    END AS FILE_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_NAME  ELSE DT.CALL_FILE_NAME  END AS NAME
                      , CASE WHEN T.NO = 1 THEN DT.EXT_CD     ELSE DT.CALL_EXT_CD     END AS EXT_CD
                      , CASE WHEN T.NO = 1 THEN 'CALLED'     ELSE 'MYFILE'         END AS DATA_GUBUN
                      , 0 AS ISLEAF
                      , 0 AS OUTLEAF
                      , DT.ORG_FILE_PATH
                 FROM (
                        SELECT DISTINCT
                               A.PRJ_ID
                             , A.FILE_ID
                             , A.FILE_NAME
                             , A.CALL_PRJ_ID
                             , A.CALL_FILE_ID
                             , A.CALL_FILE_NAME
                             , A.CALL_TYPE
                             , A.EXT_CD
                             , A.CALL_EXT_CD
                             , A.ORG_FILE_PATH
                         FROM (
                                SELECT A.PRJ_ID, A.FILE_ID, B.CAPS_SRC_FILE_NAME AS FILE_NAME
                                      , A.CALL_PRJ_ID, A.CALL_FILE_ID, C.CAPS_SRC_FILE_NAME AS CALL_FILE_NAME
                                      , A.CALL_TYPE
                                      , B.EXT_CD
                                      , C.EXT_CD AS CALL_EXT_CD
                                      , B.ORG_FILE_PATH
                                  FROM AIS0083 A
                                      , VW_AIS0010_UI B
                                      , VW_AIS0010_UI C
                                  WHERE A.PRJ_ID = B.PRJ_ID
                                    AND A.FILE_ID = B.FILE_ID
                                    AND A.CALL_PRJ_ID = C.PRJ_ID
                                    AND A.CALL_FILE_ID = C.FILE_ID
                                    AND A.CALL_PRJ_ID = #{prjId}
                                    AND A.CALL_FILE_ID = #{fileId}
                              ) A
                        ) DT
                        , COPY_T T
                    WHERE T.NO IN (1,2)
               ) DTT
               ORDER BY DTT.FILE_ID
    </select>   이것은 mybatis xml이고 postgresql인데, oracle 쿼리로 바꿔줘

## ChatGPT 응답:
> 아래는 PostgreSQL 쿼리를 Oracle 쿼리로 변환한 것입니다.

```
SELECT DISTINCT
  (SELECT A.SYSTEM_TGT_SRV_ID
   FROM AIS1003 A, AIS1011 B
   WHERE A.MAP_TYPE = '02'
   AND A.PRJ_ID = DTT.PRJ_ID
   AND B.JOB_ID = A.JOB_ID
   AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
  ) AS SYSTEM_ID,
  DTT.PRJ_ID,
  DTT.FILE_ID,
  DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID,
  COALESCE((SELECT B.TS_PRJ_ID FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME,
  (SELECT MAX(COLOR)
   FROM AIS3010
   WHERE SYSTEM_TGT_SRV_ID = (SELECT A.SYSTEM_TGT_SRV_ID
                              FROM AIS1003 A, AIS1011 B
                              WHERE A.MAP_TYPE = '02'
                              AND A.PRJ_ID = DTT.PRJ_ID
                              AND B.JOB_ID = A.JOB_ID
                              AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
                             )
  ) AS COLOR,
  (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD AND ROWNUM <= 1) AS LANG_CD,
  DTT.EXT_CD,
  (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' AND ROWNUM <= 1) AS CALLED_CHK,
  MAX(DTT.ISLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF,
  MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
FROM (
  SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
         'MYFILE' AS DATA_GUBUN,
         0 AS ISLEAF, 0 AS OUTLEAF,
         ORG_FILE_PATH
  FROM VW_AIS0010_UI
  WHERE PRJ_ID = :prjId
  AND FILE_ID = :fileId
  UNION
  SELECT DISTINCT
         DT.PRJ_ID,
         DT.FILE_ID,
         B.CAPS_SRC_FILE_NAME AS FILE_NAME,
         B.EXT_CD AS EXT_CD,
         DT.DATA_GUBUN,
         (SELECT COUNT(*)
          FROM AIS0083 D
          WHERE D.PRJ_ID = DT.PRJ_ID
          AND D.FILE_ID = DT.FILE_ID
          AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
          AND DT.CONN_ISLEAF = 1
          AND ROWNUM <= 1
         ) AS ISLEAF,
         (SELECT COUNT(*)
          FROM AIS0083 D
          WHERE D.PRJ_ID = DT.PRJ_ID
          AND D.CALL_FILE_ID = DT.FILE_ID
          AND D.FILE_ID != DT.TEMP_FILE_ID
          AND DT.CONN_ISLEAF = 1
          AND ROWNUM <= 1
         ) AS OUTLEAF,
         B.ORG_FILE_PATH
  FROM (
    SELECT CASE
      WHEN T.NO = 1 THEN DT.PRJ

## 사용자 입력:
> 계소4ㄱ

## ChatGPT 응답:
> 죄송합니다. 이전에 전송된 메시지에 대한 답변을 찾을 수 없습니다. 메시지를 다시 보내주시면 최대한 빠르게 답변해드리겠습니다.

## ChatGPT 응답:
> 죄송합니다. "계소4ㄱ"라는 문장은 이해할 수 없는 내용으로 보입니다. 더 자세한 정보를 제공해 주시거나 다른 질문을 해 주세요.

## 사용자 입력:
> 계속

## ChatGPT 응답:
> SELECT DISTINCT /* Mxgraph.xml.findShellLineageCells */
     (SELECT A.SYSTEM_TGT_SRV_ID
      FROM AIS1003 A, AIS1011 B
      WHERE A.MAP_TYPE = '02'
        AND A.PRJ_ID = DTT.PRJ_ID
        AND B.JOB_ID = A.JOB_ID
        AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
     ) AS SYSTEM_ID
     , DTT.PRJ_ID
     , DTT.FILE_ID
     , DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID
     , COALESCE((SELECT B.TS_PRJ_ID || '' FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME
     , (SELECT MAX(COLOR)
        FROM AIS3010
        WHERE SYSTEM_TGT_SRV_ID = (SELECT A.SYSTEM_TGT_SRV_ID
                                   FROM AIS1003 A, AIS1011 B
                                   WHERE A.MAP_TYPE = '02'
                                     AND A.PRJ_ID = DTT.PRJ_ID
                                     AND B.JOB_ID = A.JOB_ID
                                     AND DTT.ORG_FILE_PATH LIKE '%' || '.source' || B.SRC_PATH || '%'
                                  )
       ) AS COLOR
     , (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD AND ROWNUM = 1) AS LANG_CD
     , DTT.EXT_CD AS EXT_CD
     , (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' AND ROWNUM = 1) AS CALLED_CHK
     , MAX(DTT.ISLEAF)  OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF
     , MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
FROM (
      SELECT PRJ_ID, FILE_ID, CAPS_SRC_FILE_NAME AS NAME, EXT_CD,
             'MYFILE' AS DATA_GUBUN,
             0 AS ISLEAF, 0 AS OUTLEAF
             , ORG_FILE_PATH
      FROM VW_AIS0010_UI
      WHERE PRJ_ID = #{prjId}
        AND FILE_ID = #{fileId}
      UNION
      SELECT DISTINCT
             DT.PRJ_ID
           , DT.FILE_ID
           , B.CAPS_SRC_FILE_NAME AS FILE_NAME
           , B.EXT_CD AS EXT_CD
           , DT.DATA_GUBUN
           , (SELECT COUNT(*)
              FROM AIS0083 D
              WHERE D.PRJ_ID        = DT.PRJ_ID
                AND D.FILE_ID       = DT.FILE_ID
                AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID
                AND DT.CONN_ISLEAF = 1
                AND ROWNUM = 1) AS ISLEAF
           , (SELECT COUNT(*)
              FROM AIS0083 D
              WHERE D.PRJ_ID        = DT.PRJ_ID
                AND D.CALL_FILE_ID  = DT.FILE_ID
                AND D.FILE_ID      != DT.TEMP_FILE_ID
                AND DT.CONN_ISLEAF = 1
                AND ROWNUM = 1) AS OUTLEAF
           , B.ORG_FILE_PATH
      FROM (
            SELECT CASE

## 사용자 입력:
> 끝까지 마무리해

## ChatGPT 응답:
> 다음은 Oracle SQL로 변환한 결과입니다.

```
SELECT DISTINCT 
  (
    SELECT A.SYSTEM_TGT_SRV_ID 
    FROM AIS1003 A, AIS1011 B 
    WHERE A.MAP_TYPE = '02' 
    AND A.PRJ_ID = DTT.PRJ_ID 
    AND B.JOB_ID = A.JOB_ID 
    AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
  ) AS SYSTEM_ID, 
  DTT.PRJ_ID, 
  DTT.FILE_ID, 
  DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID, 
  COALESCE(
    (
      SELECT B.TS_PRJ_ID 
      FROM AIS8000 B 
      WHERE B.PRJ_ID = DTT.PRJ_ID 
      AND B.FILE_ID = DTT.FILE_ID
    ), 
    DTT.NAME
  ) AS NAME, 
  (
    SELECT MAX(COLOR) 
    FROM AIS3010 
    WHERE SYSTEM_TGT_SRV_ID = (
      SELECT A.SYSTEM_TGT_SRV_ID 
      FROM AIS1003 A, AIS1011 B 
      WHERE A.MAP_TYPE = '02' 
      AND A.PRJ_ID = DTT.PRJ_ID 
      AND B.JOB_ID = A.JOB_ID 
      AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%'
    )
  ) AS COLOR, 
  (
    SELECT B.LANG_CD 
    FROM AIS2102 B 
    WHERE B.EXT_CD = DTT.EXT_CD 
    AND ROWNUM = 1
  ) AS LANG_CD, 
  DTT.EXT_CD AS EXT_CD, 
  (
    SELECT COUNT(B.FILE_ID) AS CALLED_CHK 
    FROM AIS0083 B 
    WHERE B.CALL_PRJ_ID = DTT.PRJ_ID 
    AND B.CALL_FILE_ID = DTT.FILE_ID 
    AND DTT.DATA_GUBUN = 'CALLED' 
    AND ROWNUM = 1
  ) AS CALLED_CHK, 
  MAX(DTT.ISLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF, 
  MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF 
FROM (
  SELECT 
    PRJ_ID, 
    FILE_ID, 
    CAPS_SRC_FILE_NAME AS NAME, 
    EXT_CD, 
    'MYFILE' AS DATA_GUBUN, 
    0 AS ISLEAF, 
    0 AS OUTLEAF, 
    ORG_FILE_PATH 
  FROM VW_AIS0010_UI 
  WHERE PRJ_ID = :prjId 
  AND FILE_ID = :fileId 
  UNION 
  SELECT DISTINCT 
    DT.PRJ_ID, 
    DT.FILE_ID, 
    B.CAPS_SRC_FILE_NAME AS FILE_NAME, 
    B.EXT_CD AS EXT_CD, 
    DT.DATA_GUBUN, 
    (
      SELECT COUNT(*) CNT 
      FROM AIS0083 D 
      WHERE D.PRJ_ID = DT.PRJ_ID 
      AND D.FILE_ID = DT.FILE_ID 
      AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID 
      AND DT.CONN_ISLEAF = 1 
      AND ROWNUM = 1
    ) AS ISLEAF, 
    (
      SELECT COUNT(*) CNT 
      FROM AIS0083 D 
      WHERE D.PRJ_ID = DT.PRJ_ID 
      AND D.CALL_FILE_ID

## 사용자 입력:
> 끝까지 마무리해줘.. 계속 짤리고 있어.

## ChatGPT 응답:
> 죄송합니다, 계속 짤려서 완전히 보이지 않는 부분이 있었나봅니다. 다시 한 번 끝까지 마무리해드리겠습니다.

```sql
SELECT DISTINCT
  (SELECT A.SYSTEM_TGT_SRV_ID
   FROM AIS1003 A, AIS1011 B
   WHERE A.MAP_TYPE = '02'
   AND A.PRJ_ID = DTT.PRJ_ID
   AND B.JOB_ID = A.JOB_ID
   AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%') AS SYSTEM_ID,
  DTT.PRJ_ID,
  DTT.FILE_ID,
  DTT.PRJ_ID || 'F' || DTT.FILE_ID AS CELL_ID,
  COALESCE((SELECT B.TS_PRJ_ID::text FROM AIS8000 B WHERE B.PRJ_ID = DTT.PRJ_ID AND B.FILE_ID = DTT.FILE_ID), DTT.NAME) AS NAME,
  (SELECT MAX(COLOR) FROM AIS3010 WHERE SYSTEM_TGT_SRV_ID = (SELECT A.SYSTEM_TGT_SRV_ID
   FROM AIS1003 A, AIS1011 B
   WHERE A.MAP_TYPE = '02'
   AND A.PRJ_ID = DTT.PRJ_ID
   AND B.JOB_ID = A.JOB_ID
   AND DTT.ORG_FILE_PATH LIKE '%.source%' || B.SRC_PATH || '%')) AS COLOR,
  (SELECT B.LANG_CD FROM AIS2102 B WHERE B.EXT_CD = DTT.EXT_CD LIMIT 1) AS LANG_CD,
  DTT.EXT_CD AS EXT_CD,
  (SELECT COUNT(B.FILE_ID) AS CALLED_CHK FROM AIS0083 B WHERE B.CALL_PRJ_ID = DTT.PRJ_ID AND B.CALL_FILE_ID = DTT.FILE_ID AND DTT.DATA_GUBUN = 'CALLED' LIMIT 1) AS CALLED_CHK,
  MAX(DTT.ISLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS ISLEAF,
  MAX(DTT.OUTLEAF) OVER(PARTITION BY DTT.PRJ_ID, DTT.FILE_ID) AS OUTLEAF
FROM (
  SELECT PRJ_ID,
  FILE_ID,
  CAPS_SRC_FILE_NAME AS NAME,
  EXT_CD,
  'MYFILE' AS DATA_GUBUN,
  0 AS ISLEAF,
  0 AS OUTLEAF,
  ORG_FILE_PATH
  FROM VW_AIS0010_UI
  WHERE PRJ_ID = #{prjId}
  AND FILE_ID = #{fileId}
  UNION
  SELECT DISTINCT
  DT.PRJ_ID,
  DT.FILE_ID,
  B.CAPS_SRC_FILE_NAME AS FILE_NAME,
  B.EXT_CD AS EXT_CD,
  DT.DATA_GUBUN,
  (SELECT COUNT(*) CNT FROM AIS0083 D WHERE D.PRJ_ID = DT.PRJ_ID AND D.FILE_ID = DT.FILE_ID AND D.CALL_FILE_ID != DT.TEMP_CALL_FILE_ID AND DT.CONN_ISLEAF = 1 LIMIT 1) AS ISLEAF,
  (SELECT COUNT(*) CNT FROM AIS0083 D WHERE D.PRJ_ID = DT.PRJ_ID AND D.CALL_FILE_ID = DT.FILE_ID AND D.FILE_ID != DT.TEMP_FILE_ID AND DT.CONN_ISLEAF = 1 LIMIT 1) AS OUTLEAF,
  B.ORG_FILE_PATH
  FROM (
    SELECT
    CASE WHEN T.NO = 1 THEN DT.PRJ_ID ELSE DT

## ChatGPT 응답:
> 네, 죄송합니다. 마지막 부분을 완료해드리겠습니다.

    ) DT
                    WHERE T.NO IN(1,2)
                ) DT,
                VW_AIS0010_UI B
                WHERE B.PRJ_ID = DT.PRJ_ID
                AND B.FILE_ID = DT.FILE_ID
                AND (CASE WHEN  EXT_CD = 'SQL' AND B.CAPS_SRC_FILE_NAME LIKE '%' || (SELECT VAR_INIT_VAL
                                                                                     FROM AIS0014 A
                                                                                    WHERE A.PRJ_ID   = #{prjId}
                                                                                      AND A.FILE_ID  = #{fileId}
                                                                                      AND A.VAR_NAME = (SELECT MM_ABBR_NAME FROM AIS8101 WHERE MM_CLASS_ID = 1004)
                                                                                  ) || '%'
                                               THEN 1
                          WHEN EXT_CD != 'SQL' THEN 1
                          ELSE 0 END) = 1
                UNION
                SELECT CASE WHEN T.NO = 1 THEN DT.PRJ_ID     ELSE DT.CALL_PRJ_ID     END AS PRJ_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_ID    ELSE DT.CALL_FILE_ID    END AS FILE_ID
                      , CASE WHEN T.NO = 1 THEN DT.FILE_NAME  ELSE DT.CALL_FILE_NAME  END AS NAME
                      , CASE WHEN T.NO = 1 THEN DT.EXT_CD     ELSE DT.CALL_EXT_CD     END AS EXT_CD
                      , CASE WHEN T.NO = 1 THEN 'CALLED'     ELSE 'MYFILE'         END AS DATA_GUBUN
                      , 0 AS ISLEAF
                      , 0 AS OUTLEAF
                      , DT.ORG_FILE_PATH
                 FROM (
                        SELECT DISTINCT
                               A.PRJ_ID
                             , A.FILE_ID
                             , A.FILE_NAME
                             , A.CALL_PRJ_ID
                             , A.CALL_FILE_ID
                             , A.CALL_FILE_NAME
                             , A.CALL_TYPE
                             , A.EXT_CD
                             , A.CALL_EXT_CD
                             , A.ORG_FILE_PATH
                         FROM (
                                SELECT A.PRJ_ID, A.FILE_ID, B.CAPS_SRC_FILE_NAME AS FILE_NAME
                                      , A.CALL_PRJ_ID, A.CALL_FILE_ID, C.CAPS_SRC_FILE_NAME AS CALL_FILE_NAME
                                      , A.CALL_TYPE
                                      , B.EXT_CD
                                      , C.EXT_CD AS CALL_EXT_CD
                                      , B.ORG_FILE_PATH
                                  FROM AIS0083 A
                                      , VW_AIS0010_UI B
                                      , VW_AIS0010_UI C
                                  WHERE A.PRJ_ID = B.PRJ_ID
                                    AND A.FILE_ID = B.FILE_ID
                                    AND A.CALL_PRJ_ID = C.PRJ_ID
                                    AND A.CALL_FILE_ID = C.FILE_ID
                                    AND A.CALL_PRJ_ID = #{prjId}
                                    AND A.CALL_FILE_ID = #{fileId}
                              ) A
                        ) DT
                        , COPY_T T
                    WHERE T.NO IN (1,2)
               ) DTT
               ORDER BY DTT.FILE_ID
    </select> 

위의 쿼리는 PostgreSQL에서 작동하는 것으로 보입니다. Oracle에서는 다음과 같이 작성할 수 있습니다.

    SELECT DISTINCT
           (SELECT A.SYSTEM_TGT_SRV_ID
            FROM AIS1003 A, AIS1011 B
            WHERE A.MAP_TYPE = '02'
            AND A.PRJ_ID = DTT.PRJ_ID
            AND B.JOB_ID = A.JOB_ID
            AND DTT.ORG_FILE

