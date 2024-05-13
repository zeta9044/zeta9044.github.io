---
layout: post
title: "Oracle 문자셋 쿼리"
date: 2023-04-12
tags: [Oracle, 쿼리, 문자셋]
categories: blog
---

## 사용자 입력:
> oracle 문자셋을 쿼리로 아는 법 알려줘.

## ChatGPT 응답:
> 

Oracle에서 문자셋을 확인하는 쿼리는 다음과 같습니다.

```
SELECT value
FROM nls_database_parameters
WHERE parameter = 'NLS_CHARACTERSET';
```

위 쿼리를 실행하면 현재 데이터베이스의 문자셋을 확인할 수 있습니다.

## 사용자 입력:
> SELECT
	/* QtrackSchedule.xml.findinsertJobId */
	DISTINCT CASE
		WHEN DT.JOB_CNT > 1 THEN '-9'
		ELSE DT.JOB_ID
	END AS JOB_ID
FROM
	(
	SELECT
		A.JOB_ID ,
		COUNT(A.JOB_ID) OVER() AS JOB_CNT
	FROM
		AIS1003 A ,
		AIS1901 B
	WHERE
		A.PRJ_ID = '296'
		AND B.PRJ_ID = A.PRJ_ID
		AND A.MAP_TYPE IN ('02', '03', '04')
		AND B.SCHEDULE_NAME = 'IBM_DataStage_스케줄' ) DT; 이 쿼리는 postgresql인데, oracle 19c용 쿼리로 바꿔봐.

## ChatGPT 응답:
> 다음은 PostgreSQL용으로 작성된 쿼리를 Oracle 19c에 맞게 변환한 것입니다.

```
SELECT
    DISTINCT CASE
        WHEN DT.JOB_CNT > 1 THEN '-9'
        ELSE DT.JOB_ID
    END AS JOB_ID
FROM
    (
    SELECT
        A.JOB_ID,
        COUNT(A.JOB_ID) OVER() AS JOB_CNT
    FROM
        AIS1003 A,
        AIS1901 B
    WHERE
        A.PRJ_ID = '296'
        AND B.PRJ_ID = A.PRJ_ID
        AND A.MAP_TYPE IN ('02', '03', '04')
        AND B.SCHEDULE_NAME = 'IBM_DataStage_스케줄'
    ) DT;
```

Oracle에서는 `OVER()`를 사용할 때 `PARTITION BY`를 명시해야 합니다. 위의 쿼리에서는 `PARTITION BY`를 사용하지 않았으므로, Oracle에서도 동일하게 실행됩니다.

## 사용자 입력:
> 위 쿼리를 oracle에서 실행하면, SQL Error [932] [42000]: ORA-00932: 일관성 없는 데이터 유형: CHAR이(가) 필요하지만 NUMBER임



Error position: line: 13 pos: 79 이런 에러 발생.

## ChatGPT 응답:
> 오라클에서는 숫자와 문자열 간에 암시적 변환이 발생하지 않기 때문에, `CASE` 함수 내에서 `DT.JOB_ID`와 '-9'가 서로 다른 데이터 유형으로 인식됩니다. 따라서 `DT.JOB_ID`를 문자열로 변환하여 일관된 데이터 유형을 사용해야 합니다.

변경된 쿼리는 다음과 같습니다.

```
SELECT
    DISTINCT CASE
        WHEN DT.JOB_CNT > 1 THEN '-9'
        ELSE TO_CHAR(DT.JOB_ID)
    END AS JOB_ID
FROM
    (
    SELECT
        A.JOB_ID,
        COUNT(A.JOB_ID) OVER() AS JOB_CNT
    FROM
        AIS1003 A,
        AIS1901 B
    WHERE
        A.PRJ_ID = '296'
        AND B.PRJ_ID = A.PRJ_ID
        AND A.MAP_TYPE IN ('02', '03', '04')
        AND B.SCHEDULE_NAME = 'IBM_DataStage_스케줄'
    ) DT;
```

`TO_CHAR()` 함수를 사용하여 `DT.JOB_ID`를 문자열로 변환하였습니다. 이제 쿼리가 오류 없이 실행될 것입니다.

