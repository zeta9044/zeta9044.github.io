---
layout: post
title: "Oracle 다중 행 삽입"
date: 2023-03-20
tags: [행, 다중, Oracle, 삽입]
categories: blog
---

## 사용자 입력:
> oracle insert multi value 지원하나?

## ChatGPT 응답:
> 네, Oracle은 다중 행 삽입을 지원합니다. 

아래는 Oracle에서 다중 행 삽입을 수행하는 방법 중 하나인 INSERT ALL 문의 예입니다.

```
INSERT ALL
   INTO table_name (column1, column2, column3) VALUES ('value1', 'value2', 'value3')
   INTO table_name (column1, column2, column3) VALUES ('value4', 'value5', 'value6')
SELECT * FROM dual;
```

위 예제에서는 두 개의 행을 삽입하고 있습니다. 첫 번째 INTO 절은 첫 번째 행을 삽입하고, 두 번째 INTO 절은 두 번째 행을 삽입합니다. SELECT 절은 모든 INTO 절의 결과를 반환하기 때문에 필요합니다.

이 외에도 Oracle에서는 다른 방법으로도 다중 행 삽입을 수행할 수 있습니다. 예를 들어 INSERT INTO ... SELECT 문을 사용하여 다른 테이블의 데이터를 삽입할 수도 있습니다.

