---
layout: post
title: "PostgreSQL 속도 최적화"
date: 2023-05-23
tags: [속도, 최적화, PostgreSQL]
categories: blog
---

## 사용자 입력:
> SELECT /* Manager.getUniqueOwnerInfo#2 */ MAX(DT.UNIQUE_OWNER_TGT_SRV_ID_DB   )                                                       AS UNIQUE_OWNER_TGT_SRV_ID_DB   , 
       MAX(DT.UNIQUE_OWNER_NAME_DB         )                                                                                          AS UNIQUE_OWNER_NAME_DB         , 
       MAX(DT.UNIQUE_OWNER_TGT_SRV_ID_FILE )                                                                                          AS UNIQUE_OWNER_TGT_SRV_ID_FILE , 
       MAX(DT.UNIQUE_OWNER_NAME_FILE       )                                                                                          AS UNIQUE_OWNER_NAME_FILE       , 
       MAX(UNIQUE_OWNER_TGT_SRV_ID_DB   || CASE WHEN UNIQUE_OWNER_TGT_SRV_ID_DB   IS NOT NULL THEN '|' END || UNIQUE_OWNER_NAME_DB  ) AS OWNER_INFO_DB                , 
       MAX(UNIQUE_OWNER_TGT_SRV_ID_FILE || CASE WHEN UNIQUE_OWNER_TGT_SRV_ID_FILE IS NOT NULL THEN '|' END || UNIQUE_OWNER_NAME_FILE) AS OWNER_INFO_FILE                
  FROM ( 
        SELECT CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK10                                                   END AS UNIQUE_OWNER_TGT_SRV_ID_DB   , 
               CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK1 || ':' || MM_OBJ_RMK2 || ':[' || MM_OBJ_RMK3 ||']' END AS UNIQUE_OWNER_NAME_DB         , 
               CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK10                                                   END AS UNIQUE_OWNER_TGT_SRV_ID_FILE , 
               CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK11                                                   END AS UNIQUE_OWNER_NAME_FILE 
          FROM AIS8101 A 
         WHERE ? = 'KBBANK'
          AND MM_CLASS_ID   = '1002' 
          AND MM_OBJ_RMK11  = AP_MTE_COMMON_FN_SETUP_SRV_IP(?)
    AND MM_OBJ_RMK13  = 'DEFAULT'
  UNION ALL
  SELECT CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK10                                                   END AS UNIQUE_OWNER_TGT_SRV_ID_DB   ,
               CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK1 || ':' || MM_OBJ_RMK2 || ':[' || MM_OBJ_RMK3 ||']' END AS UNIQUE_OWNER_NAME_DB         ,
               CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK10                                                   END AS UNIQUE_OWNER_TGT_SRV_ID_FILE ,
               CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK11                                                   END AS UNIQUE_OWNER_NAME_FILE 
          FROM AIS8101 A 
         WHERE ? != 'KBBANK'
          AND MM_CLASS_ID   = '1002' 
          AND MM_OBJ_RMK11  = AP_COMMON_FN_SETUP_SRV_IP(?) 
    AND MM_OBJ_RMK13  = 'DEFAULT'
   ) DT  이 쿼리는 postgresql인데, 속도가 느려. 속도향상되도록 바꿔줘. join은 ansi표준으로 해줘.

## ChatGPT 응답:
> 아래는 주어진 쿼리를 최적화하여 PostgreSQL에서 성능을 향상시키는 방법입니다. 주요 변경사항은 다음과 같습니다:

1. `UNION ALL` 대신 `UNION`을 사용하여 중복을 제거합니다.
2. 서브쿼리 대신 `JOIN`을 사용하여 필요한 데이터를 조인합니다.
3. `CASE` 문을 `COALESCE` 함수로 대체하여 NULL 검사를 줄입니다.

```sql
SELECT
  MAX(COALESCE(DT1.UNIQUE_OWNER_TGT_SRV_ID_DB, DT2.UNIQUE_OWNER_TGT_SRV_ID_DB)) AS UNIQUE_OWNER_TGT_SRV_ID_DB,
  MAX(COALESCE(DT1.UNIQUE_OWNER_NAME_DB, DT2.UNIQUE_OWNER_NAME_DB)) AS UNIQUE_OWNER_NAME_DB,
  MAX(COALESCE(DT1.UNIQUE_OWNER_TGT_SRV_ID_FILE, DT2.UNIQUE_OWNER_TGT_SRV_ID_FILE)) AS UNIQUE_OWNER_TGT_SRV_ID_FILE,
  MAX(COALESCE(DT1.UNIQUE_OWNER_NAME_FILE, DT2.UNIQUE_OWNER_NAME_FILE)) AS UNIQUE_OWNER_NAME_FILE,
  MAX(COALESCE(DT1.UNIQUE_OWNER_TGT_SRV_ID_DB || '|' || DT1.UNIQUE_OWNER_NAME_DB, DT2.UNIQUE_OWNER_TGT_SRV_ID_DB || '|' || DT2.UNIQUE_OWNER_NAME_DB)) AS OWNER_INFO_DB,
  MAX(COALESCE(DT1.UNIQUE_OWNER_TGT_SRV_ID_FILE || '|' || DT1.UNIQUE_OWNER_NAME_FILE, DT2.UNIQUE_OWNER_TGT_SRV_ID_FILE || '|' || DT2.UNIQUE_OWNER_NAME_FILE)) AS OWNER_INFO_FILE
FROM
(
  SELECT
    CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK10 END AS UNIQUE_OWNER_TGT_SRV_ID_DB,
    CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK1 || ':' || MM_OBJ_RMK2 || ':[' || MM_OBJ_RMK3 || ']' END AS UNIQUE_OWNER_NAME_DB,
    CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK10 END AS UNIQUE_OWNER_TGT_SRV_ID_FILE,
    CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK11 END AS UNIQUE_OWNER_NAME_FILE
  FROM AIS8101
  WHERE
    (? = 'KBBANK' AND MM_CLASS_ID = '1002' AND MM_OBJ_RMK11 = AP_MTE_COMMON_FN_SETUP_SRV_IP(?))
    OR (? != 'KBBANK' AND MM_CLASS_ID = '1002' AND MM_OBJ_RMK11 = AP_COMMON_FN_SETUP_SRV_IP(?))
    AND MM_OBJ_RMK13 = 'DEFAULT'
) AS DT1
LEFT JOIN
(
  SELECT
    CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK10 END AS UNIQUE_OWNER_TGT_SRV_ID_DB,
    CASE WHEN MM_OBJECT_NAME = 'DBMS' THEN MM_OBJ_RMK1 || ':' || MM_OBJ_RMK2 || ':[' || MM_OBJ_RMK3 || ']' END AS UNIQUE_OWNER_NAME_DB,
    CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK10 END AS UNIQUE_OWNER_TGT_SRV_ID_FILE,
    CASE WHEN MM_OBJECT_NAME = 'APPL' THEN MM_OBJ_RMK11 END AS UNIQUE_OWNER_NAME_FILE
  FROM AIS8101
  WHERE
    (? = 'KBBANK' AND MM_CLASS_ID = '1002' AND MM_OBJ_RMK11 = AP_MTE_COMMON_FN_SETUP_SRV_IP(?))
    OR (? != 'KBB

## ChatGPT 응답:
> ANK' AND MM_CLASS_ID = '1002' AND MM_OBJ_RMK11 = AP_COMMON_FN_SETUP_SRV_IP(?))
    AND MM_OBJ_RMK13 = 'DEFAULT'
) AS DT2 ON 1=1;
```

위의 변경사항을 적용하면 원래 쿼리보다 성능이 향상되어야 합니다. 그러나 실제 성능 향상 여부는 데이터베이스 구조, 인덱스 및 데이터의 양에 따라 달라질 수 있으므로 실제 실행 계획을 확인하고 테스트해야 합니다.

