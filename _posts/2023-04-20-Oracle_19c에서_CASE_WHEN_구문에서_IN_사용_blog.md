---
layout: post
title: "Oracle 19c에서 CASE WHEN 구문에서 IN 사용"
date: 2023-04-20
tags: [사용, Oracle, 구문, IN, WHEN, CASE]
categories: blog
---

## 사용자 입력:
> oracle19c에서 case when절에서 in 구문을 쓸 수 있나?

## ChatGPT 응답:
> 네, Oracle 19c에서는 `CASE WHEN` 구문에서 `IN` 연산자를 사용할 수 있습니다. `IN` 연산자는 주어진 목록의 값 중 하나와 일치하는지 여부를 확인하는 데 사용됩니다. 

다음은 `CASE WHEN` 구문에서 `IN` 연산자를 사용하는 예제입니다.

```SQL
SELECT
  CASE
    WHEN column1 IN ('value1', 'value2', 'value3') THEN 'Matched'
    ELSE 'Not matched'
  END AS result
FROM table1;
```

위 예제에서 `column1`의 값이 `'value1'`, `'value2'`, 또는 `'value3'` 중 하나와 일치하면 `'Matched'`가 반환되고, 그렇지 않으면 `'Not matched'`가 반환됩니다.

따라서 `IN` 연산자를 `CASE WHEN` 구문에서 사용하여 여러 값에 대한 조건을 지정할 수 있습니다.

## 사용자 입력:
> oracle19c에서 case when절에서 not in 구문을 쓸 수 있나?

## ChatGPT 응답:
> 네, Oracle 19c에서 CASE WHEN 절 내부에서 NOT IN 구문을 사용할 수 있습니다.

예를 들어, 다음과 같이 사용할 수 있습니다.

```
SELECT 
    CASE 
        WHEN column1 NOT IN (value1, value2) THEN 'A'
        ELSE 'B'
    END as result
FROM table1;
```

위의 쿼리는 column1 값이 value1이나 value2와 같지 않으면 'A'를 반환하고, 그렇지 않으면 'B'를 반환합니다. 이때 NOT IN 구문은 CASE WHEN 절 내부에서 사용되었습니다.

따라서 Oracle 19c에서는 CASE WHEN 절 내부에서 NOT IN 구문을 자유롭게 사용할 수 있습니다.

## 사용자 입력:
> SELECT 
    CASE 
        WHEN column1 NOT IN ('', value2) THEN 'A'
        ELSE 'B'
    END as result
FROM table1;
위에서 ''있는데, 논리적으로 not in 비교가 되나? oracle19c에서

## ChatGPT 응답:
> 네, Oracle 19c에서도 빈 문자열(`''`)을 포함한 NOT IN 비교가 가능합니다.

빈 문자열은 NULL이 아니기 때문에, `''`을 NOT IN 구문으로 비교하면 해당 컬럼의 값이 빈 문자열이 아닌 경우에는 'A'를 반환하고, 빈 문자열인 경우에는 'B'를 반환합니다.

따라서, 예시로 제시해주신 쿼리에서 `column1`의 값이 빈 문자열(`''`)이 아니고, `value2`와 같지 않으면 'A'를 반환하고, 빈 문자열인 경우에는 'B'를 반환합니다. 이는 논리적으로 올바른 비교입니다.

## 사용자 입력:
> SELECT 
    CASE 
        WHEN column1 NOT IN (null, value2) THEN 'A'
        ELSE 'B'
    END as result
FROM table1;
위에서 ''있는데, 논리적으로 not in 비교가 되나? oracle19c에서

## ChatGPT 응답:
> NULL 값은 비교 연산에서 특별한 의미를 가지며, 일반적으로 다른 값들과의 비교에서는 예외적인 동작을 보입니다. 

따라서 위의 쿼리에서 "column1 NOT IN (NULL, value2)" 비교는 항상 NULL 값을 반환하게 됩니다. 이는 논리적으로 "column1 <> value2"와 동일하지 않습니다.

따라서 CASE WHEN 절에서 NULL 값을 비교할 때는 주의해야 합니다. 만약 NULL 값을 포함한 비교가 필요하다면, IS NULL 또는 IS NOT NULL 연산자를 사용하거나 NVL 함수 등을 사용하여 NULL 값을 다른 값으로 대체하는 방법을 고려해야 합니다. 

따라서, 위의 쿼리는 논리적으로 작동하지 않습니다. NULL 값을 비교해야 한다면 다른 방법을 고려해야 합니다.

## 사용자 입력:
> 그러면, ''과 null 이 동일한 값인가?

## ChatGPT 응답:
> ''(빈 문자열)과 NULL은 서로 다른 값입니다.

''은 문자열로 간주되며, 길이가 0인 비어있는 문자열입니다. NULL은 값이 존재하지 않음을 나타내는 특별한 값입니다.

따라서, 비교 연산자를 사용할 때 ''과 NULL은 동일한 값으로 간주되지 않습니다. 예를 들어, 'abc' <> NULL은 NULL을 반환하고, 'abc' <> ''은 TRUE를 반환합니다. 

NULL 값을 처리할 때에는 항상 주의해야 하며, NULL 값을 비교해야 하는 경우에는 IS NULL 또는 IS NOT NULL 연산자를 사용해야 합니다.

## 사용자 입력:
> oracle19c에서 SELECT     
      CASE WHEN A.D_DB_NAME IN ('','NA') AND A.D_SCHEMA_NAME IN ('','NA') THEN 'NA1'
--            WHEN A.D_DB_NAME = 'NA' AND A.D_SCHEMA_NAME != 'NA' 
--                 THEN 'NA4' || ':' || 'NA' || ':' || A.D_SCHEMA_NAME 
            WHEN (A.D_DB_NAME NOT IN  ('','NA') or A.D_SCHEMA_NAME NOT IN ('','NA'))
            THEN 'NA2' || ':' || A.D_DB_NAME || ':' || A.D_SCHEMA_NAME 
            WHEN (A.D_DB_NAME != 'NA' OR A.D_SCHEMA_NAME != 'NA')
            THEN 'NA22' || ':' || A.D_DB_NAME || ':' || A.D_SCHEMA_NAME 
            ELSE 'NA3'
       END    AS col
 FROM (SELECT 'll' AS D_DB_NAME, 'ss' AS D_SCHEMA_NAME FROM dual) a;  쿼리의 값은?

## ChatGPT 응답:
> 위의 쿼리를 실행하면 'NA2:ll:ss'가 반환됩니다.

쿼리의 실행 결과는 다음과 같습니다.

1. A.D_DB_NAME 값이 '','NA'이며, A.D_SCHEMA_NAME 값이 '','NA'인 경우 'NA1'을 반환합니다. 그러나 이 경우는 해당하지 않습니다.

2. A.D_DB_NAME 값이 '','NA'이 아니거나 A.D_SCHEMA_NAME 값이 '','NA'이 아닌 경우 'NA2:' || A.D_DB_NAME || ':' || A.D_SCHEMA_NAME 을 반환합니다. 여기서 A.D_DB_NAME 값은 'll'이며, A.D_SCHEMA_NAME 값은 'ss'입니다. 따라서 결과는 'NA2:ll:ss'가 됩니다.

3. 위의 두 조건에 해당하지 않는 경우 'NA3'을 반환합니다. 그러나 이 경우도 해당하지 않습니다.

따라서 위의 쿼리의 결과는 'NA2:ll:ss'가 됩니다.

