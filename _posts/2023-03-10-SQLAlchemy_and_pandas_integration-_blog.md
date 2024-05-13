---
layout: post
title: "SQLAlchemy and pandas integration."
date: 2023-03-10
tags: [and, SQLAlchemy, integration, pandas]
categories: blog
---

## 사용자 입력:
>             program_query = (
                session.query(
                    ais8800.pgm_id, ais8800.pgm_grp_id,
                    ais8801.pgm_grp_nm, ais8801.exec_ord_no,
                    ais8800.pgm_src
                )
                .join(ais8801, ais8800.pgm_grp_id == ais8801.pgm_grp_id)
                .filter(ais8800.aval_ed_dt == '99991231235959')
                .filter(ais8800.lia_type_cd == '0')
                .filter(ais8801.aval_ed_dt == '99991231235959')
                .filter(ais8801.lia_type_cd == '0')
                .filter(ais8800.pgm_grp_id == '%s')
                .filter(ais8801.pgm_grp_nm == '%s')
                .order_by(ais8801.exec_ord_no, ais8800.pgm_grp_id, ais8800.exec_ord_no)
                .statement
            ) 의 program_query가 pandas.read_sql_query(program_query,con=engine,params)에서 동작하는지 알려줘. 특히, %s에 params가 정확히 매핑되서 돌아가는 건지 알려줘

## ChatGPT 응답:
> 이 코드는 SQLAlchemy를 사용하여 SQL 쿼리를 작성하고, 이 쿼리를 pandas의 `read_sql_query` 함수로 전달하여 데이터베이스에서 결과를 가져오는 작업을 수행합니다.

`%s`는 SQLAlchemy의 파라미터화된 쿼리 기능을 사용하여 런타임에 값을 전달할 수 있는 placeholder입니다. `params` 변수는 이 placeholder를 대체하는 값을 포함하는 파라미터 딕셔너리입니다. 이 딕셔너리는 `read_sql_query` 함수의 `params` 매개변수로 전달됩니다.

따라서 `read_sql_query` 함수는 `program_query` 쿼리를 실행하고, `params` 딕셔너리의 값으로 `%s` placeholder를 대체하여 실행합니다. 이때 `params` 딕셔너리의 키는 `%s` placeholder의 위치에 따라 결정됩니다. 예를 들어, 쿼리에서 첫 번째 `%s` placeholder는 `params` 딕셔너리의 첫 번째 키로 대체됩니다.

따라서 이 코드는 `program_query` 쿼리를 실행하고, `params` 딕셔너리의 값으로 `%s` placeholder를 대체하여 실행합니다. 이때 `params` 딕셔너리의 키는 placeholder의 위치에 따라 결정됩니다. 하지만, 이 코드의 전체적인 동작을 정확하게 판단하기 위해서는 `params` 딕셔너리가 어떤 값을 가지고 있는지에 따라서 달라질 수 있습니다.

