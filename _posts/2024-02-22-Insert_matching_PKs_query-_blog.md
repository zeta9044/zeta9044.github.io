---
layout: post
title: "Insert matching PKs query."
date: 2024-02-22
tags: [matching, Insert, PKs, query]
categories: blog
---

## 사용자 입력:
> FROM테이블과 TO테이블이 있어. 각 테이블의 PK컬럼이름은   FROM은 fromkey, TO는 tokey라고 해. 그러면,   FROM테이블과 TO테이블의 pk가 일치하는 것만 TO테이블에 insert하는 쿼리을 만들어. 만드는 제약사항은 ANSI 표준으로 만들어야 해.

## ChatGPT 응답:
> ANSI 표준 SQL을 사용하여 `FROM` 테이블과 `TO` 테이블 사이에 PK가 일치하는 레코드만 `TO` 테이블에 삽입하는 쿼리를 만들려면 `INSERT INTO ... SELECT ... FROM ... WHERE ...` 구문을 사용할 수 있습니다. 이 경우, `FROM` 테이블의 PK 컬럼 이름은 `fromkey`이고, `TO` 테이블의 PK 컬럼 이름은 `tokey`라고 했으므로, 해당 조건을 충족하는 쿼리는 다음과 같이 작성할 수 있습니다.

```sql
INSERT INTO TO (tokey, column2, column3, ...)
SELECT f.fromkey, f.column2, f.column3, ...
FROM FROM f
INNER JOIN TO t ON f.fromkey = t.tokey
WHERE NOT EXISTS (
    SELECT 1 FROM TO WHERE tokey = f.fromkey
);
```

이 쿼리는 다음 단계로 작동합니다:

1. `FROM` 테이블(`f`로 별칭 지정)과 `TO` 테이블(`t`로 별칭 지정)을 `INNER JOIN`을 사용하여 `fromkey`와 `tokey`가 일치하는 레코드를 찾습니다.
2. `WHERE NOT EXISTS` 절은 `TO` 테이블에 이미 해당 `tokey`가 존재하는지 확인합니다. 만약 `TO` 테이블에 `tokey`가 이미 존재한다면, 그 레코드는 삽입 대상에서 제외됩니다.
3. 최종적으로 선택된 레코드는 `TO` 테이블에 삽입됩니다. 삽입할 때 `tokey`를 포함한 필요한 모든 컬럼을 명시해야 합니다. `column2`, `column3`, `...`는 `TO` 테이블에 삽입하고자 하는 다른 컬럼들을 나타냅니다.

주의할 점은 `FROM`과 `TO`가 예약어일 수 있기 때문에, 실제 테이블 이름이 이러한 예약어와 충돌하지 않도록 테이블 이름을 선택하거나, 테이블 이름을 따옴표로 묶어서 사용하는 것이 좋습니다(예: `"FROM"`). ANSI SQL 표준을 엄격히 준수하는 경우, 데이터베이스 시스템에 따라 구문이 약간 달라질 수 있으므로, 실제 적용 시에는 데이터베이스 문서를 참조하는 것이 좋습니다.

## 사용자 입력:
> 그러면, FROM테이블명은 SRC로, TO테이블은 TGT로 변경해.

