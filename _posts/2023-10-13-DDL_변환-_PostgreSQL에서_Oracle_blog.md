---
layout: post
title: "DDL 변환: PostgreSQL에서 Oracle"
date: 2023-10-13
tags: [변환, DDL, Oracle]
categories: blog
---

## 사용자 입력:
> CREATE INDEX ais0038_$${{prj_id}}_idx03 ON ais0038_$${{prj_id}} USING btree (prj_id, file_id, parent_seq_id, tag_id);

CREATE UNIQUE INDEX ais0038_$${{prj_id}}_pk ON ais0038_$${{prj_id}} USING btree (prj_id, file_id, obj_id, func_id, tag_seq_id);

## ChatGPT 응답:
> It looks like you're creating indexes on a table whose name is dynamic and is dependent on a variable `prj_id`. 

Here's a brief breakdown of what you're doing:

1. **Creating a non-unique

## 사용자 입력:
> CREATE INDEX ais0038_$${{prj_id}}_idx03 ON ais0038_$${{prj_id}} USING btree (prj_id, file_id, parent_seq_id, tag_id);

CREATE UNIQUE INDEX ais0038_$${{prj_id}}_pk ON ais0038_$${{prj_id}} USING btree (prj_id, file_id, obj_id, func_id, tag_seq_id);  이 DDL문은 postgresql인데, oracle로 바꿔봐.

## ChatGPT 응답:
> 변환하겠습니다. PostgreSQL의 DDL문을 Oracle에 맞게 변경해보았습니다.

```sql
CREATE INDEX ais0038_&prj_id._idx03 ON ais0038_&prj_id. (prj_id, file_id, parent_seq_id, tag_id);

CREATE UNIQUE INDEX ais0038_&prj_id._pk ON ais0038_&prj_id. (prj_id, file_id, obj_id, func_id, tag_seq_id);
```

Oracle에서는 substitution variables를 사용할 때 `&` 기호를 사용합니다. 그리고 `USING btree` 절은 Oracle에서 필요하지 않기 때문에 제외하였습니다. 

만약 실제로 Oracle에서 사용하려면, 위 코드를 SQL*Plus나 다른 Oracle 관련 툴에서 실행하기 전에 `SET DEFINE '&'` 명령어를 실행하여 substitution variable을 활성화 해야합니다.

## 사용자 입력:
> ROW_NUMBER() OVER (
                PARTITION BY DAT.PRJ_ID
                , DAT.FILE_ID
                , DAT.SQL_ID
                , DAT.TABLE_ID
            ORDER BY
                DAT.COL_ID
            ) 이것도 바꿔봐.

## ChatGPT 응답:
> 제공해주신 SQL 구문은 `ROW_NUMBER()` 함수를 사용한 윈도우 함수입니다. 그대로의 구문은 이미 Oracle에서도 지원되기 때문에 별도의 큰 변환 작업이 필요하지 않습니다.

그러나, 만약 당신이 특별한 변환 요청이 없다면, 제공된 구문을 Oracle에서 그대로 사용할 수 있습니다:

```sql
ROW_NUMBER() OVER (
    PARTITION BY DAT.PRJ_ID, DAT.FILE_ID, DAT.SQL_ID, DAT.TABLE_ID
    ORDER BY DAT.COL_ID
)
```

만약 다른 요청이나, 특별한 변환을 원하신다면, 구체적으로 지시해주시면 감사하겠습니다!

## 사용자 입력:
> DENSE_RANK() OVER (
                    ORDER BY
                        OPIN.PRJ_ID
                        , OPIN.FILE_ID
                        , S4.ATTR_VALUE
                    ) 이것도 바꿔봐.

## ChatGPT 응답:
> 제공된 SQL 구문은 `DENSE_RANK()` 함수를 사용한 윈도우 함수입니다. 이 함수는 Oracle에서도 지원되므로, 별도의 큰 변환 작업이 필요하지 않습니다.

제공된 구문을 Oracle에서 그대로 사용할 수 있습니다:

```sql
DENSE_RANK() OVER (
    ORDER BY OPIN.PRJ_ID, OPIN.FILE_ID, S4.ATTR_VALUE
)
```

따라서, 위의 구문을 Oracle에서 그대로 사용하실 수 있습니다. 만약 다른 요청이나, 특별한 변환을 원하신다면, 구체적으로 지시해주시면 감사하겠습니다!

## 사용자 입력:
>                             CASE
                                WHEN STRPOS(
                                    x.ATTR_VALUE
                                    , 'ConnectorPX'
                                ) > 0
                                    THEN 'tbl'
                                ELSE 'obj'
                            END 이것도 바꿔봐.

## ChatGPT 응답:
> 제공된 SQL 구문은 PostgreSQL의 `STRPOS` 함수를 사용한 `CASE` 문입니다. PostgreSQL의 `STRPOS` 함수는 문자열 내에서 다른 문자열의 위치를 반환합니다. 이와 유사한 함수는 Oracle에서 `INSTR`로 제공됩니다.

PostgreSQL의 `STRPOS` 함수를 Oracle의 `INSTR` 함수로 변환하면 다음과 같습니다:

```sql
CASE
    WHEN INSTR(x.ATTR_VALUE, 'ConnectorPX') > 0 THEN 'tbl'
    ELSE 'obj'
END
```

이렇게 변환된 구문을 Oracle에서 사용할 수 있습니다.

