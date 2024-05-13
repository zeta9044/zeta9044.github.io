---
layout: post
title: "ANSI Join 예시"
date: 2023-05-17
tags: [예시, Join, ANSI]
categories: blog
---

## 사용자 입력:
>      link
    ,link_s1
    ,flow
    ,flow_s1
    ,flow_s2 이렇게 5개 테이블이 있는데, link,link_s1,flow까지는 inner join 관계이고, flow를 기준으로 flow_s1, flow_s2는 조인관계가 없을 수도 있어, left join하려고 한다. 이렇게 할 때, 쿼리를 ansi표준으로 예시해줘

## ChatGPT 응답:
> 아래는 ANSI 표준 SQL을 사용하여 link, link_s1, flow 테이블을 inner join하고, flow_s1 및 flow_s2 테이블을 left join하는 쿼리의 예시입니다:

```sql
SELECT *
FROM link
INNER JOIN link_s1 ON link.link_id = link_s1.link_id
INNER JOIN flow ON link.link_id = flow.link_id
LEFT JOIN flow_s1 ON flow.flow_id = flow_s1.flow_id
LEFT JOIN flow_s2 ON flow.flow_id = flow_s2.flow_id;
```

이 예시에서는 `link`, `link_s1`, `flow` 테이블은 inner join으로 연결되어 있으며, `flow_s1` 및 `flow_s2` 테이블은 left join으로 연결됩니다. inner join은 매칭되는 값을 가진 행만 결과에 포함되고, left join은 매칭되는 값이 없어도 왼쪽 테이블의 모든 행이 결과에 포함됩니다.

