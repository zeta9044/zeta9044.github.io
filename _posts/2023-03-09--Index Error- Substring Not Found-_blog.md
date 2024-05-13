---
layout: post
title: ""Index Error: Substring Not Found""
date: 2023-03-09
tags: [Index, Found, Not, Error, Substring]
categories: blog
---

## 사용자 입력:
> print('ddd'.index('x'))

## ChatGPT 응답:
> The code will raise a ValueError because the substring 'x' is not found in the string 'ddd'.

Here's the output of the code:

```
ValueError: substring not found
```

## 사용자 입력:
> python에서 문자열 포함여부 판단하는 예제

## ChatGPT 응답:
> 문자열 포함 여부를 판단하는 방법은 다양합니다. 여기에는 몇 가지 예제를 제공합니다.

1. `in` 연산자를 사용하는 방법:

```python
text = "Hello, World!"
if "Hello" in text:
    print("문자열에 'Hello'가 포함되어 있습니다.")
else:
    print("문자열에 'Hello'가 포함되어 있지 않습니다.")
```

2. `find()` 메서드를 사용하는 방법:

```python
text = "Hello, World!"
if text.find("Hello") != -1:
    print("문자열에 'Hello'가 포함되어 있습니다.")
else:
    print("문자열에 'Hello'가 포함되어 있지 않습니다.")
```

3. 정규식을 사용하는 방법:

```python
import re

text = "Hello, World!"
pattern = r"Hello"
if re.search(pattern, text):
    print("문자열에 'Hello'가 포함되어 있습니다.")
else:
    print("문자열에 'Hello'가 포함되어 있지 않습니다.")
```

위 예제들은 각각 `in` 연산자, `find()` 메서드, 정규식을 사용하여 문자열 포함 여부를 판단하는 방법을 보여줍니다. 이 중에서도 `in` 연산자를 사용하는 방법이 가장 간단하고 직관적입니다.

## 사용자 입력:
> 파이썬에서 2023-03-09 09:00:03,249 - db2xml - ERROR - make_was_xml - (cx_Oracle.DatabaseError) ORA-01036: illegal variable name/number
[SQL: select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            ais8800 a,
                            ais8801 b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = %s
                            and b.pgm_grp_nm= %s
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no]

## ChatGPT 응답:
> 이 코드는 에러 메시지로 보입니다. 이 에러는 cx_Oracle 라이브러리를 사용하여 Oracle 데이터베이스에 쿼리를 실행할 때 발생하는 것 같습니다. 이 에러는 "ORA-01036: illegal variable name/number"라는 오라클 데이터베이스에서 발생하는 에러 메시지입니다. 이 오류는 SQL 쿼리에 매개 변수 이름이나 번호가 잘못된 경우 발생합니다.

해결책으로는 SQL 쿼리에서 사용되는 매개 변수 `pgm_grp_id`나 `pgm_grp_nm`의 값이 잘못 전달되었을 가능성이 있습니다. 이 경우, 값을 수정하거나 변수 이름을 확인해야합니다. 또한 cx_Oracle 라이브러리에서 매개 변수를 처리하는 방식도 확인할 필요가 있을 수 있습니다. 

그 외에도 SQL 쿼리 내부적인 문제나 데이터베이스 구조의 문제가 있는 경우도 있으므로, 추가적인 정보가 필요할 수 있습니다.

## 사용자 입력:
> sqlalchemy를 써서 에러 생긴건가?

## ChatGPT 응답:
> 제가 본 정보만으로는 cx_Oracle 라이브러리를 사용하여 Oracle 데이터베이스에 직접 쿼리를 실행한 것 같습니다. SQLAlchemy는 파이썬에서 데이터베이스를 다루기 위한 유명한 라이브러리 중 하나이며, 이를 사용하여 데이터베이스 연결 및 쿼리 실행 등을 수행할 수 있습니다. 

만약 해당 코드에서 SQLAlchemy를 사용하였다면 SQLAlchemy에서 발생하는 에러 메시지가 출력되어야 합니다. 따라서 SQLAlchemy를 사용하는 부분이 있을 경우 해당 부분의 코드 및 에러 메시지를 확인해 보시는 것이 좋습니다.

## 사용자 입력:
> 위에 쿼리는 pandas에서 사용하는 건데...postgres,oracle 차이가 있나?

## ChatGPT 응답:
> 네, PostgreSQL과 Oracle은 둘 다 관계형 데이터베이스 관리 시스템(RDBMS)이지만 각각 고유한 문법을 가지고 있기 때문에 SQL 쿼리가 다를 수 있습니다. PostgreSQL과 Oracle 모두 SQL 표준에 기반을 두고 있지만, 두 데이터베이스는 각자 고유한 확장 기능 및 문법을 가지고 있습니다.

따라서 pandas에서 PostgreSQL 데이터베이스에 사용되는 SQL 쿼리는 Oracle 데이터베이스에 사용되는 SQL 쿼리와 다를 수 있습니다. 특히, PostgreSQL과 Oracle은 각각 다른 데이터베이스 엔진을 사용하기 때문에 쿼리 실행 시 발생할 수 있는 오류도 서로 다를 수 있습니다.

그러나 pandas는 SQLAlchemy를 사용하여 PostgreSQL 및 Oracle과 같은 여러 데이터베이스와 상호 작용할 수 있습니다. SQLAlchemy는 데이터베이스 종류에 상관없이 일관된 API를 제공하므로, 사용자는 데이터베이스별로 다른 쿼리 문법을 배우지 않아도 됩니다. 대신 SQLAlchemy가 데이터베이스별로 자동으로 적절한 SQL 쿼리를 생성하여 실행하도록 할 수 있습니다.

## 사용자 입력:
> 
def create_engine(args=None):
    if args is None or args.db_url is None:
        db_url = 'postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres'
    else:
        db_url = args.db_url
    return sa.create_engine(db_url)  여기에서 리턴되는 객체에서 db명을 알 수 있나?

## ChatGPT 응답:
> `create_engine()` 함수에서 리턴되는 객체는 SQLAlchemy의 `Engine` 클래스의 인스턴스입니다. `Engine` 클래스의 인스턴스는 데이터베이스 연결에 대한 정보를 가지고 있으며, 데이터베이스와 상호 작용하는 데 사용됩니다. 

그러나 `Engine` 클래스의 인스턴스는 데이터베이스 이름을 직접 제공하지 않습니다. 데이터베이스 이름은 데이터베이스 URL 문자열에 포함되어 있습니다. 따라서 데이터베이스 이름을 확인하려면 `db_url` 변수를 살펴보아야 합니다. 

예를 들어, 위의 코드에서 `db_url`은 PostgreSQL 데이터베이스에 대한 URL 문자열로 설정되어 있습니다. 이 문자열에서 데이터베이스 이름은 "postgres"입니다. 

따라서 `create_engine()` 함수에서 리턴된 `Engine` 객체를 사용하여 데이터베이스와 상호 작용하려면, 데이터베이스 이름이 필요하지 않습니다. 단지 `Engine` 객체를 사용하여 데이터베이스 쿼리를 실행하면 됩니다.

## 사용자 입력:
> import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리


def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = f"""select
                                    pgm_grp_id,
                                    pgm_grp_nm
                                from
                                    {ais8801_table_name}
                                where
                                    lia_type_cd = '0'
                                    and aval_ed_dt = '99991231235959'
                                order by pgm_grp_id,exec_ord_no"""
        program_query = f"""select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            {ais8800_table_name} a,
                            {ais8801_table_name} b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = %s
                            and b.pgm_grp_nm= %s
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no"""
        return program_group_query, program_query
    elif type == 'lia':
        program_query = f"""select
                                a.pgm_id,
                                a.pgm_grp_id,
                                a.exec_ord_no,
                                a.pgm_src
                            from
                                {ais8800_table_name} a
                            where
                                a.aval_ed_dt = '99991231235959'
                                and a.lia_type_cd != '0'
                            order by
                                a.pgm_grp_id,
                                a.exec_ord_no"""
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()


# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f""" select
                 pgm_grp_id
                ,pgm_grp_nm
                ,pgm_grp_desc
                ,lia_type_cd
                ,exec_ord_no
                ,reanly_exec_yn
                ,exec_cond_yn
                ,exec_pgm_id
                ,del_yn
                from
                    {ais8801_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query

# 프로그램메타쿼리


def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f"""select
                pgm_id
                ,pgm_grp_id
                ,lia_type_cd
                ,db_type_cd
                ,pgm_nm
                ,exec_ord_no
                ,src_type_cd
                ,sql_type_cd
                ,pgm_desc
                ,data_maker
                ,common_exec_yn
                ,use_type_cd
                ,del_yn
                from
                    {ais8800_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query

# 스키마 및 변환결과


def trans(s: str, args: argparse.Namespace):
    input = copy.copy(s)
    if args.transform_db is not None:
        input = tranform_postgres_to_oracle(input, get_transform_dic())
        input = transform_seq_to_oracle(input)
    if args.schema_name is not None:
        input = attach_schema_name(
            input, args.schema_name, read_qtrack_table_list())
    
    return input

# was xml 만들기


def make_was_xml(was_xml_path: str, was_group_query: str, was_program_query: str, engine: sa.engine, logger: logging.Logger, args: argparse.Namespace):

    logger.info('was xml 만들기 시작')
    was_rtde_group_df = None
    try:

        was_rtde_group_df = pd.read_sql_query(was_group_query, con=engine)

        for idx in range(len(was_rtde_group_df)):
            # pgm_grp_id별로 xml구성요소 읽기
            was_rtde_id_df = pd.read_sql_query(
                was_program_query, con=engine, params=tuple(was_rtde_group_df.values[idx]))
            temp_list = str(tuple(was_rtde_group_df.values[idx])[
                            0]).replace('\\', '/').split('/')
            file_name = temp_list[len(temp_list)-1]
            logger.debug(file_name+'%s', '생성...')

            # XML헤더/풋터
            XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""
            xml_doc_type = """<!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">\n"""
            xml_header = """<mapper namespace="%s" >""" % tuple(
                was_rtde_group_df.values[idx])[1]
            xml_footer = "</mapper>"

            # XML파일생성
            with open(was_xml_path+file_name, 'w', encoding='utf-8') as file:
                file.write(xml_header)
                for inner_idx in range(len(was_rtde_id_df)):
                    s = str(DecryptFile.decrypt(
                        was_rtde_id_df['pgm_src'][inner_idx]))
                    s = trans(s, args)
                    was_rtde_id_df.loc[inner_idx, 'pgm_src'] = alt_char(s)
                    file.write(was_rtde_id_df['pgm_src'][inner_idx])
                file.write(xml_footer)

            # XML파일 beautify
            with open(was_xml_path+file_name, 'r', encoding='utf-8') as file:
                element = ET.XML(file.read())
                ET.indent(element)
            with open(was_xml_path+file_name, 'w', encoding='utf-8') as file:
                file.write(XML_TYPE)
                file.write(xml_doc_type)
                file.write(ET.tostring(element, encoding='unicode'))
    except Exception as e:
        logger.exception(e)

    logger.info('was xml 만들기 종료')

    return len(was_rtde_group_df)

## ChatGPT 응답:
> `create_engine()` 함수에서 리턴되는 `Engine` 객체는 단순히 데이터베이스 연결 정보만 가지고 있으며, 데이터베이스 이름은 포함되어 있지 않습니다. 데이터베이스 이름은 데이터베이스 URL 문자열에 포함되어 있습니다.

따라서 `create_engine()` 함수에서 리턴된 `Engine` 객체에서는 데이터베이스 이름을 알 수 없습니다. 하지만 데이터베이스 URL 문자열에서 데이터베이스 이름을 직접 추출할 수 있습니다. 예를 들어 PostgreSQL 데이터베이스의 URL 문자열은 "postgresql://<username>:<password>@<host>:<port>/<database>" 형식으로 되어 있습니다. 여기에서 `<database>`가 데이터베이스 이름입니다. 따라서 `db_url` 변수에서 URL 문자열을 추출하여 데이터베이스 이름을 확인할 수 있습니다.

## 사용자 입력:
> 위에 제시한 코드를 sqlalchemy의 orm을 이용해서 바꿔줘

## ChatGPT 응답:
> 안녕하세요! 코드를 SQLAlchemy ORM으로 변경해드리겠습니다.

```python
from sqlalchemy import Column, Integer, String, create_engine
from sqlalchemy.orm import sessionmaker, relationship
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class Ais8801(Base):
    __tablename__ = 'ais8801'

    pgm_grp_id = Column(Integer, primary_key=True)
    pgm_grp_nm = Column(String)
    lia_type_cd = Column(String)
    aval_ed_dt = Column(String)
    exec_ord_no = Column(Integer)

class Ais8800(Base):
    __tablename__ = 'ais8800'

    pgm_id = Column(Integer, primary_key=True)
    pgm_grp_id = Column(Integer, ForeignKey('ais8801.pgm_grp_id'))
    pgm_grp = relationship("Ais8801")
    lia_type_cd = Column(String)
    db_type_cd = Column(String)
    pgm_nm = Column(String)
    exec_ord_no = Column(Integer)
    src_type_cd = Column(String)
    sql_type_cd = Column(String)
    pgm_desc = Column(String)
    data_maker = Column(String)
    common_exec_yn = Column(String)
    use_type_cd = Column(String)
    del_yn = Column(String)
    pgm_src = Column(String)

def create_session(db_url):
    engine = create_engine(db_url)
    Session = sessionmaker(bind=engine)
    return Session()

def rtde_program_query(type: str, session, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = session.query(Ais8801.pgm_grp_id, Ais8801.pgm_grp_nm).filter_by(lia_type_cd='0', aval_ed_dt='99991231235959').order_by(Ais8801.pgm_grp_id, Ais8801.exec_ord_no).statement
        program_query = session.query(Ais8800.pgm_id, Ais8800.pgm_grp_id, Ais8801.pgm_grp_nm, Ais8801.exec_ord_no, Ais8800.pgm_src).join(Ais8801, Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.aval_ed_dt == '99991231235959', Ais8800.lia_type_cd == '0', Ais8801.aval_ed_dt == '99991231235959', Ais8801.lia_type_cd == '0', Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.pgm_grp_id == bindparam('pgm_grp_id')).filter(Ais8801.pgm_grp_nm == bindparam('pgm_grp_nm')).order_by(Ais8801.exec_ord_no, Ais8800.pgm_grp_id, Ais8800.exec_ord_no).statement
        return program_group_query, program_query
    elif type == 'lia':
        program_query = session.query(Ais8800.pgm_id, Ais8800.pgm_grp_id, Ais8800.exec_ord_no, Ais8800.pgm_src).filter(Ais8800.aval_ed_dt == '99991231235959', Ais8800.lia_type_cd != '0').order_by(Ais8800.pgm_grp_id, Ais8800.exec_ord_no).statement
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()
```

`rtde_program_query` 함수에서는 SQLAlchemy ORM의 query 객체를 사용하여 데이터베

## 사용자 입력:
> 계속

## ChatGPT 응답:
> 이어서 쿼리를 구성합니다. SQLAlchemy ORM은 이를 가능하게 하는 매우 강력한 도구입니다. query 객체에서 `.statement` 속성을 사용하면 쿼리문을 확인할 수 있습니다. 이를 사용하여 기존의 문자열 쿼리를 변경합니다.

그리고 `create_session` 함수를 만들어 데이터베이스 연결 정보를 이용해 Session 객체를 생성하는 코드를 작성합니다. 이 함수를 호출하여 Session 객체를 생성하여 `session` 매개변수로 전달하면, 이 객체를 사용하여 query를 실행합니다.

마지막으로, 이전에 사용되던 `trans` 함수에서는 전역 변수를 사용하여 기존 문자열을 변경했지만, 이제는 ORM 객체에서 직접 값을 수정할 수 있습니다. 수정된 값을 다시 ORM 객체에 저장하여 저장소에 반영할 수 있습니다. 이렇게 하면 전역 변수를 사용하는 것보다 코드가 더 깔끔해집니다.

## 사용자 입력:
> pgm_desc가 oracle에서 clob타입인데, 위에 제시한 코드로 될까?

## ChatGPT 응답:
> `pgm_desc` 필드가 Oracle에서 CLOB 타입인 경우, SQLAlchemy ORM에서는 Text 타입으로 매핑됩니다. 이는 문자열 필드의 크기가 일반적인 DBMS에서 제한될 수 있기 때문입니다. 

따라서 `Ais8800` ORM 클래스의 `pgm_desc` 필드를 `Column(Text)`로 선언하면 됩니다. 아래는 해당 코드입니다.

```python
from sqlalchemy import Column, Integer, String, Text, create_engine
from sqlalchemy.orm import sessionmaker, relationship
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class Ais8801(Base):
    __tablename__ = 'ais8801'

    pgm_grp_id = Column(Integer, primary_key=True)
    pgm_grp_nm = Column(String)
    lia_type_cd = Column(String)
    aval_ed_dt = Column(String)
    exec_ord_no = Column(Integer)

class Ais8800(Base):
    __tablename__ = 'ais8800'

    pgm_id = Column(Integer, primary_key=True)
    pgm_grp_id = Column(Integer, ForeignKey('ais8801.pgm_grp_id'))
    pgm_grp = relationship("Ais8801")
    lia_type_cd = Column(String)
    db_type_cd = Column(String)
    pgm_nm = Column(String)
    exec_ord_no = Column(Integer)
    src_type_cd = Column(String)
    sql_type_cd = Column(String)
    pgm_desc = Column(Text)
    data_maker = Column(String)
    common_exec_yn = Column(String)
    use_type_cd = Column(String)
    del_yn = Column(String)
    pgm_src = Column(String)

def create_session(db_url):
    engine = create_engine(db_url)
    Session = sessionmaker(bind=engine)
    return Session()

def rtde_program_query(type: str, session, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = session.query(Ais8801.pgm_grp_id, Ais8801.pgm_grp_nm).filter_by(lia_type_cd='0', aval_ed_dt='99991231235959').order_by(Ais8801.pgm_grp_id, Ais8801.exec_ord_no).statement
        program_query = session.query(Ais8800.pgm_id, Ais8800.pgm_grp_id, Ais8801.pgm_grp_nm, Ais8801.exec_ord_no, Ais8800.pgm_src).join(Ais8801, Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.aval_ed_dt == '99991231235959', Ais8800.lia_type_cd == '0', Ais8801.aval_ed_dt == '99991231235959', Ais8801.lia_type_cd == '0', Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.pgm_grp_id == bindparam('pgm_grp_id')).filter(Ais8801.pgm_grp_nm == bindparam('pgm_grp_nm')).order_by(Ais8801.exec_ord_no, Ais8800.pgm_grp_id, Ais8800.exec_ord_no).statement
        return program_group_query, program_query
    elif type == 'lia':
        program_query = session.query(Ais8800.pgm_id, Ais8800.pgm_grp_id, Ais8800.exec_ord_no, Ais8800.pgm_src).filter(Ais8800

## 사용자 입력:
> 테이블을 클래스로 하나하나 지정하는 방법 말고 없을까?

## ChatGPT 응답:
> SQLAlchemy ORM에서는 `automap_base`를 사용하여 데이터베이스 스키마로부터 모델 클래스를 자동 생성할 수 있습니다. 이 방법을 사용하면 데이터베이스에 대한 정보를 모두 ORM 클래스에서 정의할 필요가 없습니다.

아래는 `automap_base`를 사용하여 모델 클래스를 자동 생성하는 예시 코드입니다.

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session

Base = automap_base()

engine = create_engine('postgresql://user:password@host:port/dbname')
Base.prepare(engine, reflect=True)

Ais8800 = Base.classes.ais8800
Ais8801 = Base.classes.ais8801

session = Session(engine)

program_group_query = session.query(Ais8801.pgm_grp_id, Ais8801.pgm_grp_nm).filter_by(lia_type_cd='0', aval_ed_dt='99991231235959').order_by(Ais8801.pgm_grp_id, Ais8801.exec_ord_no).statement
program_query = session.query(Ais8800.pgm_id, Ais8800.pgm_grp_id, Ais8801.pgm_grp_nm, Ais8801.exec_ord_no, Ais8800.pgm_src).join(Ais8801, Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.aval_ed_dt == '99991231235959', Ais8800.lia_type_cd == '0', Ais8801.aval_ed_dt == '99991231235959', Ais8801.lia_type_cd == '0', Ais8800.pgm_grp_id == Ais8801.pgm_grp_id).filter(Ais8800.pgm_grp_id == bindparam('pgm_grp_id')).filter(Ais8801.pgm_grp_nm == bindparam('pgm_grp_nm')).order_by(Ais8801.exec_ord_no, Ais8800.pgm_grp_id, Ais8800.exec_ord_no).statement
```

이렇게 하면 데이터베이스에 대한 정보를 모두 ORM 클래스에서 정의할 필요 없이, `Base` 객체를 사용하여 자동으로 생성된 모델 클래스를 이용하여 데이터베이스에 접근할 수 있습니다.

## 사용자 입력:
> automap_base는 sqlalchemy 버전이 뭐야?

## ChatGPT 응답:
> `automap_base`는 SQLAlchemy 0.9 이상 버전에서 사용할 수 있습니다. 이전 버전에서는 `sqlalchemy.ext.automap` 모듈이 제공되지 않으므로 `declarative_base`나 클래스를 직접 정의해야 합니다. 하지만 대부분의 버전에서는 사용 가능합니다. 현재 가장 최신 버전인 SQLAlchemy 1.4.0 버전에서도 사용 가능합니다.

## 사용자 입력:
> import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리


def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = f"""select
                                    pgm_grp_id,
                                    pgm_grp_nm
                                from
                                    {ais8801_table_name}
                                where
                                    lia_type_cd = '0'
                                    and aval_ed_dt = '99991231235959'
                                order by pgm_grp_id,exec_ord_no"""
        program_query = f"""select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            {ais8800_table_name} a,
                            {ais8801_table_name} b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = %s
                            and b.pgm_grp_nm= %s
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no"""
        return program_group_query, program_query
    elif type == 'lia':
        program_query = f"""select
                                a.pgm_id,
                                a.pgm_grp_id,
                                a.exec_ord_no,
                                a.pgm_src
                            from
                                {ais8800_table_name} a
                            where
                                a.aval_ed_dt = '99991231235959'
                                and a.lia_type_cd != '0'
                            order by
                                a.pgm_grp_id,
                                a.exec_ord_no"""
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()


# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f""" select
                 pgm_grp_id
                ,pgm_grp_nm
                ,pgm_grp_desc
                ,lia_type_cd
                ,exec_ord_no
                ,reanly_exec_yn
                ,exec_cond_yn
                ,exec_pgm_id
                ,del_yn
                from
                    {ais8801_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query

# 프로그램메타쿼리


def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f"""select
                pgm_id
                ,pgm_grp_id
                ,lia_type_cd
                ,db_type_cd
                ,pgm_nm
                ,exec_ord_no
                ,src_type_cd
                ,sql_type_cd
                ,pgm_desc
                ,data_maker
                ,common_exec_yn
                ,use_type_cd
                ,del_yn
                from
                    {ais8800_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query   이 코드를 sqlachemy의 ORM , automap_base를 이용해서 바꿔줘

## ChatGPT 응답:
> SQLAlchemy의 ORM, `automap_base`를 이용하여 코드를 변환하면 다음과 같습니다.

```python
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session

# create an automap base
Base = automap_base()

# reflect the tables
Base.prepare(engine, reflect=True)

# assign the tables to variables
AIS8800 = Base.classes.ais8800
AIS8801 = Base.classes.ais8801

# 프로그램쿼리
def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = Session().query(AIS8801.pgm_grp_id, AIS8801.pgm_grp_nm)\
            .filter(AIS8801.lia_type_cd == '0')\
            .filter(AIS8801.aval_ed_dt == '99991231235959')\
            .order_by(AIS8801.pgm_grp_id, AIS8801.exec_ord_no)\
            .statement
        program_query = Session().query(AIS8800.pgm_id, AIS8800.pgm_grp_id, AIS8801.pgm_grp_nm, AIS8801.exec_ord_no, AIS8800.pgm_src)\
            .filter(AIS8800.aval_ed_dt == '99991231235959')\
            .filter(AIS8800.lia_type_cd == '0')\
            .filter(AIS8801.aval_ed_dt == '99991231235959')\
            .filter(AIS8801.lia_type_cd == '0')\
            .filter(AIS8800.pgm_grp_id == AIS8801.pgm_grp_id)\
            .filter(AIS8800.pgm_grp_id == sa.bindparam('pgm_grp_id'))\
            .filter(AIS8801.pgm_grp_nm == sa.bindparam('pgm_grp_nm'))\
            .order_by(AIS8801.exec_ord_no, AIS8800.pgm_grp_id, AIS8800.exec_ord_no)\
            .statement
        return program_group_query, program_query
    elif type == 'lia':
        program_query = Session().query(AIS8800.pgm_id, AIS8800.pgm_grp_id, AIS8800.exec_ord_no, AIS8800.pgm_src)\
            .filter(AIS8800.aval_ed_dt == '99991231235959')\
            .filter(AIS8800.lia_type_cd != '0')\
            .order_by(AIS8800.pgm_grp_id, AIS8800.exec_ord_no)\
            .statement
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()

# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = Session().query(AIS8801.pgm_grp_id, AIS8801.pgm_grp_nm, AIS8801.pgm_grp_desc, AIS8801.lia_type_cd, AIS8801.exec_ord_no, AIS8801.reanly_exec_yn, AIS8801.exec_cond_yn, AIS8801.exec_pgm_id, AIS8801.del_yn)\
            .filter(AIS8801.lia_type_cd.op(type_val)('0'))\
            .filter(AIS8801.aval_ed_dt == '

## 사용자 입력:
> 계속

## ChatGPT 응답:
> ORM으로 변경된 코드입니다. SQLAlchemy의 `declarative_base`를 사용하여 테이블을 클래스로 정의하였습니다. 이후 `automap_base`로 테이블을 매핑하였습니다. `session.query`를 이용하여 쿼리를 실행하였습니다. 

```python
import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리

def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str, meta):
    if type == 'was':
        ProgramGroup = meta.tables[ais8801_table_name]
        Program = meta.tables[ais8800_table_name]

        program_group_query = Session().query(ProgramGroup.pgm_grp_id, ProgramGroup.pgm_grp_nm) \
            .filter(ProgramGroup.lia_type_cd == '0') \
            .filter(ProgramGroup.aval_ed_dt == '99991231235959') \
            .order_by(ProgramGroup.pgm_grp_id, ProgramGroup.exec_ord_no) \
            .statement

        program_query = Session().query(Program.pgm_id, Program.pgm_grp_id, ProgramGroup.pgm_grp_nm, ProgramGroup.exec_ord_no, Program.pgm_src) \
            .join(ProgramGroup, Program.pgm_grp_id == ProgramGroup.pgm_grp_id) \
            .filter(Program.aval_ed_dt == '99991231235959') \
            .filter(Program.lia_type_cd == '0') \
            .filter(ProgramGroup.aval_ed_dt == '99991231235959') \
            .filter(ProgramGroup.lia_type_cd == '0') \
            .filter(Program.pgm_grp_id == bindparam('pgm_grp_id')) \
            .filter(ProgramGroup.pgm_grp_nm == bindparam('pgm_grp_nm')) \
            .order_by(ProgramGroup.exec_ord_no, Program.pgm_grp_id, Program.exec_ord_no) \
            .statement

        return program_group_query, program_query

    elif type == 'lia':
        Program = meta.tables[ais8800_table_name]

        program_query = Session().query(Program.pgm_id, Program.pgm_grp_id, Program.exec_ord_no, Program.pgm_src) \
            .filter(Program.aval_ed_dt == '99991231235959') \
            .filter(Program.lia_type_cd != '0') \
            .order_by(Program.pgm_grp_id, Program.exec_ord_no) \
            .statement

        return program_query

    else:
        print("type은 was 또는 lia")
        exit()

# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str, meta):
    if type == 'was':
        ProgramGroup = meta.tables[ais8801_table_name]

        query = Session().query(ProgramGroup

## 사용자 입력:
> 계속

## ChatGPT 응답:
> 아래는 SQLAlchemy ORM과 automap_base를 사용하여 변환한 코드입니다.

```python
import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa
from sqlalchemy.orm import Session
from sqlalchemy.ext.automap import automap_base

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리
def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = f"""select
                                    pgm_grp_id,
                                    pgm_grp_nm
                                from
                                    {ais8801_table_name}
                                where
                                    lia_type_cd = '0'
                                    and aval_ed_dt = '99991231235959'
                                order by pgm_grp_id,exec_ord_no"""
        program_query = f"""select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            {ais8800_table_name} a,
                            {ais8801_table_name} b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = :pgm_grp_id
                            and b.pgm_grp_nm= :pgm_grp_nm
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no"""
        return program_group_query, program_query
    elif type == 'lia':
        program_query = f"""select
                                a.pgm_id,
                                a.pgm_grp_id,
                                a.exec_ord_no,
                                a.pgm_src
                            from
                                {ais8800_table_name} a
                            where
                                a.aval_ed_dt = '99991231235959'
                                and a.lia_type_cd != '0'
                            order by
                                a.pgm_grp_id,
                                a.exec_ord_no"""
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()

# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f""" select
                 pgm_grp_id
                ,pgm_grp_nm
                ,pgm_grp_desc
                ,lia_type_cd
                ,exec_ord_no
                ,reanly_exec_yn
                ,exec_cond_yn
                ,exec_pgm_id
                ,del_yn
                from
                    {ais8801_table_name}


## 사용자 입력:
> import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리


def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = f"""select
                                    pgm_grp_id,
                                    pgm_grp_nm
                                from
                                    {ais8801_table_name}
                                where
                                    lia_type_cd = '0'
                                    and aval_ed_dt = '99991231235959'
                                order by pgm_grp_id,exec_ord_no"""
        program_query = f"""select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            {ais8800_table_name} a,
                            {ais8801_table_name} b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = %s
                            and b.pgm_grp_nm= %s
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no"""
        return program_group_query, program_query
    elif type == 'lia':
        program_query = f"""select
                                a.pgm_id,
                                a.pgm_grp_id,
                                a.exec_ord_no,
                                a.pgm_src
                            from
                                {ais8800_table_name} a
                            where
                                a.aval_ed_dt = '99991231235959'
                                and a.lia_type_cd != '0'
                            order by
                                a.pgm_grp_id,
                                a.exec_ord_no"""
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()


# 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f""" select
                 pgm_grp_id
                ,pgm_grp_nm
                ,pgm_grp_desc
                ,lia_type_cd
                ,exec_ord_no
                ,reanly_exec_yn
                ,exec_cond_yn
                ,exec_pgm_id
                ,del_yn
                from
                    {ais8801_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query

# 프로그램메타쿼리


def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f"""select
                pgm_id
                ,pgm_grp_id
                ,lia_type_cd
                ,db_type_cd
                ,pgm_nm
                ,exec_ord_no
                ,src_type_cd
                ,sql_type_cd
                ,pgm_desc
                ,data_maker
                ,common_exec_yn
                ,use_type_cd
                ,del_yn
                from
                    {ais8800_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query   이 코드를 sqlalchemy의 automap_base를 가지고 바꿔줘

## ChatGPT 응답:
> SQLAlchemy의 `automap_base`를 사용하여 기존의 코드를 ORM으로 바꿔보겠습니다.

```python
import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 데이터베이스 연결 설정
DATABASE_URI = 'postgresql://user:password@localhost:5432/mydatabase'
engine = sa.create_engine(DATABASE_URI)
metadata = sa.MetaData(bind=engine)
Base = sa.ext.automap.automap_base()
Base.prepare(engine, reflect=True)

# 테이블 객체 추출
ais8801 = Base.classes.ais8801
ais8800 = Base.classes.ais8800

# 프로그램쿼리
def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = (
            metadata
            .session
            .query(ais8801.pgm_grp_id, ais8801.pgm_grp_nm)
            .filter(ais8801.lia_type_cd == '0', ais8801.aval_ed_dt == '99991231235959')
            .order_by(ais8801.pgm_grp_id, ais8801.exec_ord_no)
            .statement
        )

        program_query = (
            metadata
            .session
            .query(ais8800.pgm_id, ais8800.pgm_grp_id, ais8801.pgm_grp_nm, ais8801.exec_ord_no, ais8800.pgm_src)
            .join(ais8801, ais8800.pgm_grp_id == ais8801.pgm_grp_id)
            .filter(ais8800.aval_ed_dt == '99991231235959', ais8800.lia_type_cd == '0',
                    ais8801.aval_ed_dt == '99991231235959', ais8801.lia_type_cd == '0',
                    ais8800.pgm_grp_id == sa.bindparam('pgm_grp_id'), ais8801.pgm_grp_nm == sa.bindparam('pgm_grp_nm'))
            .order_by(ais8801.exec_ord_no, ais8800.pgm_grp_id, ais8800.exec_ord_no)
            .statement
        )
        return program_group_query, program_query

    elif type == 'lia':
        program_query = (
            metadata
            .session
            .query(ais8800.pgm_id, ais8800.pgm_grp_id, ais8800.exec_ord_no, ais8800.pgm_src)
            .filter(ais8800.aval_ed_dt == '99991231235959', ais8800.lia_type_cd != '0')
            .order_by(ais8800.pgm_grp_id, ais8800.exec_ord_no)
            .statement
        )
        return program_query

    else:
        print("type은 was 또는 lia")
        exit()


# 프로그램그룹메타쿼리
def rt

## 사용자 입력:
> import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 프로그램쿼리


def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = f"""select
                                    pgm_grp_id,
                                    pgm_grp_nm
                                from
                                    {ais8801_table_name}
                                where
                                    lia_type_cd = '0'
                                    and aval_ed_dt = '99991231235959'
                                order by pgm_grp_id,exec_ord_no"""
        program_query = f"""select
                            a.pgm_id,
                            a.pgm_grp_id,
                            b.pgm_grp_nm,
                            b.exec_ord_no,
                            a.pgm_src
                        from
                            {ais8800_table_name} a,
                            {ais8801_table_name} b
                        where
                            a.aval_ed_dt = '99991231235959'
                            and a.lia_type_cd = '0'
                            and b.aval_ed_dt = '99991231235959'
                            and b.lia_type_cd = '0'
                            and a.pgm_grp_id = b.pgm_grp_id
                            and a.pgm_grp_id = %s
                            and b.pgm_grp_nm= %s
                        order by
                            b.exec_ord_no,
                            a.pgm_grp_id,
                            a.exec_ord_no"""
        return program_group_query, program_query
    elif type == 'lia':
        program_query = f"""select
                                a.pgm_id,
                                a.pgm_grp_id,
                                a.exec_ord_no,
                                a.pgm_src
                            from
                                {ais8800_table_name} a
                            where
                                a.aval_ed_dt = '99991231235959'
                                and a.lia_type_cd != '0'
                            order by
                                a.pgm_grp_id,
                                a.exec_ord_no"""
        return program_query
    else:
        print("type은 was 또는 lia")
        exit()    이 코드를 SQLAlchemy의 automap_base를 사용하여 바꿔줘

## ChatGPT 응답:
> 안녕하세요, SQLAlchemy의 `automap_base`를 사용하여 코드를 변경해보겠습니다.

```python
import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa
from sqlalchemy.ext.automap import automap_base

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# 데이터베이스 연결 설정
engine = create_engine("postgresql://user:password@host:port/database")

# automap_base로 데이터베이스 테이블을 자동으로 매핑
Base = automap_base()
Base.prepare(engine, reflect=True)

# 테이블 객체 가져오기
ais8801_table = Base.classes.ais8801_table_name
ais8800_table = Base.classes.ais8800_table_name


# 프로그램쿼리
def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = (
            sa.session.query(ais8801_table.pgm_grp_id, ais8801_table.pgm_grp_nm)
            .filter_by(lia_type_cd='0')
            .filter_by(aval_ed_dt='99991231235959')
            .order_by(ais8801_table.pgm_grp_id, ais8801_table.exec_ord_no)
            .statement
        )
        
        program_query = (
            sa.session.query(
                ais8800_table.pgm_id, ais8800_table.pgm_grp_id,
                ais8801_table.pgm_grp_nm, ais8801_table.exec_ord_no,
                ais8800_table.pgm_src
            )
            .join(ais8801_table, ais8800_table.pgm_grp_id == ais8801_table.pgm_grp_id)
            .filter(ais8800_table.aval_ed_dt == '99991231235959')
            .filter(ais8800_table.lia_type_cd == '0')
            .filter(ais8801_table.aval_ed_dt == '99991231235959')
            .filter(ais8801_table.lia_type_cd == '0')
            .filter_by(pgm_grp_id='%s')
            .filter_by(pgm_grp_nm='%s')
            .order_by(ais8801_table.exec_ord_no, ais8800_table.pgm_grp_id, ais8800_table.exec_ord_no)
            .statement
        )
        
        return program_group_query, program_query
        
    elif type == 'lia':
        program_query = (
            sa.session.query(ais8800_table.pgm_id, ais8800_table.pgm_grp_id,
                             ais8800_table.exec_ord_no, ais8800_table.pgm_src)
            .filter(ais8800_table.aval_ed_dt == '99991231235959')
            .filter(ais8800_table.lia_type_cd != '0')
            .order_by(ais8800_table.pgm_grp_id, ais8800_table.exec_ord_no)
            .statement
        )
        return program_query
    else:
        print("type

## 사용자 입력:
> # 프로그램메타쿼리


def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f"""select
                pgm_id
                ,pgm_grp_id
                ,lia_type_cd
                ,db_type_cd
                ,pgm_nm
                ,exec_ord_no
                ,src_type_cd
                ,sql_type_cd
                ,pgm_desc
                ,data_maker
                ,common_exec_yn
                ,use_type_cd
                ,del_yn
                from
                    {ais8800_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query  을 sqlachemy의 automap_base를 사용하여 바꿔줘

## ChatGPT 응답:
> 안녕하세요, SQLAlchemy의 `automap_base()`를 사용하여 변환하겠습니다. 

```python
from sqlalchemy.ext.automap import automap_base

Base = automap_base()
metadata = sa.MetaData()

# DB 연결
engine = create_engine(DB_CONN_STR, encoding="utf-8")
Base.prepare(engine, reflect=True)
ProgramMeta = Base.classes.ais8800

def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()

    # pgm_id, pgm_grp_id, ... 이렇게 테이블 컬럼을 참조해도 되고
    # ProgramMeta.pgm_id, ProgramMeta.pgm_grp_id, ... 이렇게 클래스의 속성을 참조해도 됩니다.
    query = sa.select([
        ProgramMeta.pgm_id,
        ProgramMeta.pgm_grp_id,
        ProgramMeta.lia_type_cd,
        ProgramMeta.db_type_cd,
        ProgramMeta.pgm_nm,
        ProgramMeta.exec_ord_no,
        ProgramMeta.src_type_cd,
        ProgramMeta.sql_type_cd,
        ProgramMeta.pgm_desc,
        ProgramMeta.data_maker,
        ProgramMeta.common_exec_yn,
        ProgramMeta.use_type_cd,
        ProgramMeta.del_yn
    ]).where(
        sa.and_(
            ProgramMeta.lia_type_cd != '0',
            ProgramMeta.aval_ed_dt == '99991231235959',
            ProgramMeta.del_yn == 'N',
            ProgramMeta.lia_type_cd.op(type_val)('0')
        )
    ).order_by(ProgramMeta.pgm_grp_id, ProgramMeta.exec_ord_no)

    return query
```

## ChatGPT 응답:
> ```python
from sqlalchemy.ext.automap import automap_base

Base = automap_base()
metadata = sa.MetaData()

# DB 테이블을 metadata에 reflect
ais8800 = sa.Table('AIS8800', metadata, autoload=True, autoload_with=engine)
ais8801 = sa.Table('AIS8801', metadata, autoload=True, autoload_with=engine)

# automap_base를 이용하여 테이블 클래스 생성
ProgramMeta = Base.classes.AIS8800
GroupMeta = Base.classes.AIS8801

def rtde_program_meta_query(type: str, ais8800_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = session.query(ProgramMeta.pgm_id, ProgramMeta.pgm_grp_id, ProgramMeta.lia_type_cd, 
                          ProgramMeta.db_type_cd, ProgramMeta.pgm_nm, ProgramMeta.exec_ord_no, 
                          ProgramMeta.src_type_cd, ProgramMeta.sql_type_cd, ProgramMeta.pgm_desc, 
                          ProgramMeta.data_maker, ProgramMeta.common_exec_yn, ProgramMeta.use_type_cd, 
                          ProgramMeta.del_yn).filter(ProgramMeta.lia_type_cd != '0', ProgramMeta.aval_ed_dt == '99991231235959').order_by(ProgramMeta.pgm_grp_id, ProgramMeta.exec_ord_no).statement
    return query
```

## 사용자 입력:
> # 프로그램그룹메타쿼리
def rtde_group_meta_query(type: str, ais8801_table_name: str):
    if type == 'was':
        type_val = "="
    elif type == 'lia':
        type_val = "!="
    else:
        print("type은 was 또는 lia")
        exit()
    query = f""" select
                 pgm_grp_id
                ,pgm_grp_nm
                ,pgm_grp_desc
                ,lia_type_cd
                ,exec_ord_no
                ,reanly_exec_yn
                ,exec_cond_yn
                ,exec_pgm_id
                ,del_yn
                from
                    {ais8801_table_name}
                where
                    lia_type_cd {type_val} '0'
                    and aval_ed_dt = '99991231235959'
                order by pgm_grp_id,exec_ord_no"""
    return query  이 코드를 sqlachemy의 automap_base를 이용하여 바꿔줘

## ChatGPT 응답:
> SQLAlchemy의 `automap_base`를 사용하여 데이터베이스 테이블을 클래스로 매핑할 수 있습니다. 이를 위해서는 데이터베이스에 연결된 SQLAlchemy `engine` 객체와 매핑할 클래스의 기본 클래스인 `Base` 객체가 필요합니다. `Base` 객체를 생성한 후, `Base.prepare()` 메서드를 호출하여 데이터베이스 테이블을 클래스로 매핑할 수 있습니다. 

아래는 `ais8800` 테이블과 `ais8801` 테이블을 클래스로 매핑하는 예시입니다.

```python
from sqlalchemy.ext.automap import automap_base

# create engine and connect to the database
engine = create_engine('postgresql://user:password@host:port/dbname')

# create automap base
Base = automap_base()

# reflect the tables
Base.prepare(engine, reflect=True)

# map the classes to table names
Ais8800 = Base.classes.ais8800
Ais8801 = Base.classes.ais8801
```

`Base.classes` 속성을 사용하여 매핑된 클래스에 접근할 수 있습니다. 매핑된 클래스는 데이터베이스 테이블의 각 열에 해당하는 속성을 가지고 있습니다. 이제 `rtde_program_query()`, `rtde_program_meta_query()`, `rtde_group_meta_query()` 함수에서 사용되는 테이블 이름을 클래스로 변경하면 됩니다.

## ChatGPT 응답:
> 안녕하세요! automap_base를 사용하여 코드를 변경해드리겠습니다.

```python
from sqlalchemy.ext.automap import automap_base

Base = automap_base()
```

automap_base로 Base라는 객체를 생성한 후에,

```python
metadata = sa.MetaData()
metadata.reflect(bind=engine)
Base.prepare(metadata.bind, reflect=True)
```

이런식으로 metadata를 사용하여 DB의 정보를 받아와서 Base 객체와 매핑해줍니다. 여기서 engine은 create_engine 함수를 통해 만든 DB연결 객체입니다.

그리고 이후에는 Base 객체를 사용하여 자동 매핑된 테이블 클래스를 이용할 수 있습니다.

```python
ProgramMeta = Base.classes.program_meta
ProgramGroupMeta = Base.classes.program_group_meta
Program = Base.classes.program
```

이렇게 매핑된 클래스를 사용하면 됩니다. 전체 코드는 아래와 같습니다.

```python
import pandas as pd
import os
import math
import xml.etree.ElementTree as ET
from rtde.security import DecryptFile
from rtde.util import (convNoneToEmptyString, euc2utf, set_xmldir, make_cdata, alt_char, read_qtrack_table_list,
                       attach_schema_name, get_transform_dic, tranform_postgres_to_oracle, set_log, create_engine, transform_seq_to_oracle)
import logging
import logging.config
import concurrent.futures
import time
import argparse
import copy
import sqlalchemy as sa
from sqlalchemy.ext.automap import automap_base

# 상수선언
XML_TYPE = """<?xml version="1.0" encoding="UTF-8"?>\n"""

# automap_base로 매핑 클래스 생성
Base = automap_base()

# DB연결
engine = create_engine('postgresql://user:password@host:port/db_name')
metadata = sa.MetaData()
metadata.reflect(bind=engine)
Base.prepare(metadata.bind, reflect=True)

# 자동 매핑된 테이블 클래스 생성
ProgramMeta = Base.classes.program_meta
ProgramGroupMeta = Base.classes.program_group_meta
Program = Base.classes.program

# 프로그램쿼리
def rtde_program_query(type: str, ais8801_table_name: str, ais8800_table_name: str):
    if type == 'was':
        program_group_query = session.query(ProgramGroupMeta.pgm_grp_id, ProgramGroupMeta.pgm_grp_nm).\
            filter(ProgramGroupMeta.lia_type_cd == '0').\
            filter(ProgramGroupMeta.aval_ed_dt == '99991231235959').\
            order_by(ProgramGroupMeta.pgm_grp_id, ProgramGroupMeta.exec_ord_no)
        program_query = session.query(Program.pgm_id, Program.pgm_grp_id, ProgramGroupMeta.pgm_grp_nm, ProgramGroupMeta.exec_ord_no, Program.pgm_src).\
            join(ProgramGroupMeta, Program.pgm_grp_id == ProgramGroupMeta.pgm_grp_id).\
            filter(Program.aval_ed_dt == '99991231235959').\
            filter(Program.lia_type_cd == '0').\
            filter(ProgramGroupMeta.aval_ed_dt == '99991231235959').\
            filter(ProgramGroupMeta.lia_type_cd == '0').\
            filter(Program.pgm_grp_id == pgm_grp_id).\
            filter(ProgramGroupMeta.pgm_grp_nm == pgm_grp_nm).\
            order_by(ProgramGroupMeta.exec_ord_no, Program.pgm_grp_id

