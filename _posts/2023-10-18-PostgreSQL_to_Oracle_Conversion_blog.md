---
layout: post
title: "PostgreSQL to Oracle Conversion"
date: 2023-10-18
tags: [Conversion, Oracle, PostgreSQL, to]
categories: blog
---

## 사용자 입력:
>    select a.cluster_nm, a.app_type, a.srv_ip, a.context_name
       , 'src.kafka.bootstrap.servers' as json_key
       , (a.context_value::json->'src.kafka.bootstrap.servers')::varchar as json_value_0
    from zq_kafka_info a
   where 1 = 1
     and a.app_type = 'WORKER'
     and a.context_value like '%"src.kafka.bootstrap.servers"%'; 이것은 postgresql쿼리인데,  oracle 19c 쿼리로 바꿔줘. 한글로 설명해.

## ChatGPT 응답:
> 해당 쿼리를 Oracle 19c 버전에 맞게 변환하려면 몇 가지 고려사항이 있습니다. PostgreSQL에서 사용하는 문법과 Oracle에서 사용하는 문법은 약간 다릅니다. 

1. PostgreSQL의 `::` 형변환 연산자는 Oracle에서는 `CAST` 함수로 대체됩니다.
2. Oracle에서 JSON 문법 처리는 `JSON_VALUE` 함수를 사용할 수 있습니다.
3. PostgreSQL의 `LIKE` 연산자는 Oracle에서도 동일하게 사용됩니다. 그러나 `%` 와 같은 와일드카드 문자도 동일하게 적용됩니다.

위의 정보를 바탕으로 주어진 PostgreSQL 쿼리를 Oracle 19c 쿼리로 변환하면 다음과 같습니다:

```sql
SELECT a.cluster_nm, a.app_type, a.srv_ip, a.context_name,
       'src.kafka.bootstrap.servers' AS json_key,
       CAST(JSON_VALUE(a.context_value, '$.src.kafka.bootstrap.servers') AS VARCHAR2(4000)) AS json_value_0
FROM zq_kafka_info a
WHERE a.app_type = 'WORKER'
AND INSTR(a.context_value, '"src.kafka.bootstrap.servers"') > 0;
```

설명:
- `::` 형변환 연산자 대신 `CAST` 함수를 사용했습니다.
- PostgreSQL의 `->` JSON 연산자는 Oracle에서 `JSON_VALUE` 함수로 대체되었습니다.
- `INSTR` 함수를 사용하여 `context_value` 내에 특정 문자열이 포함되어 있는지 확인했습니다. 이 함수는 첫 번째 문자열 내에서 두 번째 문자열이 나타나는 첫 번째 위치를 반환합니다. 0보다 큰 값이 반환되면 해당 문자열이 포함되어 있다는 것을 의미합니다.

이렇게 변환된 Oracle 쿼리는 Oracle 19c 환경에서 원하는 결과를 제공해야 합니다.

## 사용자 입력:
> 그런데,  postgresql쿼리와 달리 json_value_0 컬럼이 모두 null이야.

## ChatGPT 응답:
> `JSON_VALUE` 함수가 `null`을 반환하는 이유는 주어진 JSON 경로에 해당하는 값이 존재하지 않거나 JSON 문자열이 올바르지 않기 때문일 수 있습니다.

다음 접근 방식으로 문제를 해결해 봅시다:

1. **JSON 경로 확인**: Oracle에서의 JSON 경로 문법이 올바른지 확인합시다. `$`는 JSON 객체의 루트를 나타냅니다. 만약 `"src.kafka.bootstrap.servers"`가 JSON의 최상위에 위치한 키라면 지금의 경로는 올바르다고 할 수 있습니다.

2. **JSON 문자열 검증**: `zq_kafka_info` 테이블의 `context_value` 열에 저장된 JSON 문자열이 유효한지 확인해야 합니다. Oracle에서 JSON 문자열이 유효한지 확인하는 방법은 `IS JSON` 조건을 사용하는 것입니다.

```sql
SELECT a.context_value
FROM zq_kafka_info a
WHERE a.app_type = 'WORKER'
AND NOT (a.context_value IS JSON);
```

위의 쿼리를 사용하면 유효하지 않은 JSON 문자열을 찾을 수 있습니다. 만약 유효하지 않은 JSON 문자열이 있으면 해당 문자열을 수정하거나 무시해야 합니다.

3. **`JSON_VALUE`의 에러 처리**: `JSON_VALUE` 함수는 두 번째 인수로 `ERROR ON ERROR`를 추가하여 오류가 발생했을 때 에러 메시지를 반환하게 할 수 있습니다. 이를 사용하여 문제가 되는 행을 확인해 봅시다.

```sql
SELECT a.cluster_nm, a.app_type, a.srv_ip, a.context_name,
       'src.kafka.bootstrap.servers' AS json_key,
       CAST(JSON_VALUE(a.context_value, '$.src.kafka.bootstrap.servers' ERROR ON ERROR) AS VARCHAR2(4000)) AS json_value_0
FROM zq_kafka_info a
WHERE a.app_type = 'WORKER'
AND INSTR(a.context_value, '"src.kafka.bootstrap.servers"') > 0;
```

위의 쿼리를 실행하면 오류가 발생하는 행에 대한 에러 메시지를 얻을 수 있습니다. 이 정보를 통해 문제의 원인을 더 잘 파악할 수 있습니다.

## 사용자 입력:
> 네가 마지막에 제시한 쿼리를 실행하니, SQL Error [40462] [99999]: ORA-40462: JSON_VALUE가 값으로 평가되지 않았습니다 이런 오류가 발생해.

## ChatGPT 응답:
> ORA-40462 오류는 `JSON_VALUE` 함수가 해당 JSON 경로에 값을 찾을 수 없을 때 발생합니다. 이는 JSON 객체에 해당 경로에 해당하는 항목이 없거나 값이 `null`로 설정되어 있기 때문일 수 있습니다.

경로 문제를 해결하기 위해 몇 가지 사항을 확인해 봅시다:

1. **JSON 경로 문법**: Oracle의 `JSON_VALUE` 함수는 JSON 경로를 지정할 때 단순한 문자열 키를 사용할 경우 큰따옴표(")를 필요로 합니다. 따라서 경로를 `$.src.kafka.bootstrap.servers`에서 `$.["src.kafka.bootstrap.servers"]`로 수정해야 합니다.

2. **NULL 값 처리**: `JSON_VALUE` 함수는 기본적으로 경로에 해당하는 값이 `null`일 경우 `null`을 반환합니다. 이를 방지하려면 `NULL ON NULL` 옵션을 사용할 수 있습니다.

쿼리를 수정해 다음과 같이 실행해 보세요:

```sql
SELECT a.cluster_nm, a.app_type, a.srv_ip, a.context_name,
       'src.kafka.bootstrap.servers' AS json_key,
       CAST(JSON_VALUE(a.context_value, '$.["src.kafka.bootstrap.servers"]' NULL ON NULL ERROR ON ERROR) AS VARCHAR2(4000)) AS json_value_0
FROM zq_kafka_info a
WHERE a.app_type = 'WORKER'
AND INSTR(a.context_value, '"src.kafka.bootstrap.servers"') > 0;
```

위의 쿼리는 JSON 경로의 키를 큰따옴표로 감싸고, NULL 값 처리를 위해 `NULL ON NULL` 옵션을 추가했습니다. 이렇게 수정하면 오류 없이 원하는 결과를 얻을 수 있을 것입니다.

