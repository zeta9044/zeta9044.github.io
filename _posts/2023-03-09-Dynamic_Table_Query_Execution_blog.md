---
layout: post
title: "Dynamic Table Query Execution"
date: 2023-03-09
tags: [Dynamic, Table, Query, Execution]
categories: blog
---

## 사용자 입력:
> from sqlalchemy import create_engine, Column, Integer, String, DateTime,update
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime

# engine = create_engine("oracle://LIAUSR:LIAUSR@192.168.0.160:1521/oracle")
engine = create_engine("postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres")
Base = declarative_base()

class Ais8801(Base):
    __tablename__ = 'ais8801'
    pgm_grp_id = Column(String, primary_key=True)
    aval_ed_dt = Column(String, primary_key=True)
    aval_st_dt = Column(String)
    pgm_grp_nm = Column(String)
    pgm_grp_desc = Column(String)
    lia_type_cd = Column(String)
    exec_ord_no = Column(String)
    reanly_exec_yn = Column(String)
    exec_cond_yn = Column(String)
    exec_pgm_id = Column(String)
    mdfy_date = Column(DateTime)
    mdfy_user = Column(String)
    ip_addr = Column(String)
    del_yn = Column(String)
    parser_id = Column(String)


Session = sessionmaker(bind=engine)

ais8801_update_record = update(Ais8801).where(Ais8801.pgm_grp_id=='2', Ais8801.aval_ed_dt=='1').values(aval_ed_dt ='2')
ais8801_insert_record = Ais8801(pgm_grp_id='2',aval_ed_dt='1',aval_st_dt='1',pgm_grp_nm='1',pgm_grp_desc='1',lia_type_cd='1',exec_ord_no ='1'
                         ,reanly_exec_yn='1',exec_cond_yn='1',exec_pgm_id='1',mdfy_date=datetime.now(),mdfy_user='1',ip_addr='1',del_yn='1',parser_id='1')
with Session() as session:
    try:
        session.add(ais8801_insert_record)
        session.execute(ais8801_update_record)
    except:
        session.rollback()
        raise
    else:
        session.commit()
 위 프로그램을 args를 받아서 ais8801,ais8800 테이블명을 동적으로 바꿔서 실행할 수 있도록 바꿔줘.

## ChatGPT 응답:
> 

다음은 `ais8801`과 `ais8800` 테이블 모두에 대해 동적으로 실행될 수 있는 코드입니다. `execute_query` 함수는 테이블 이름과 실행할 쿼리 문자열을 인자로 받습니다. 이 함수는 쿼리 문자열에 테이블 이름을 대입한 후에 해당 쿼리를 실행합니다.

```python
from typing import Tuple
from sqlalchemy import create_engine, Column, Integer, String, DateTime, update
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime

engine = create_engine("postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres")
Base = declarative_base()


class Ais8801(Base):
    __tablename__ = 'ais8801'
    pgm_grp_id = Column(String, primary_key=True)
    aval_ed_dt = Column(String, primary_key=True)
    aval_st_dt = Column(String)
    pgm_grp_nm = Column(String)
    pgm_grp_desc = Column(String)
    lia_type_cd = Column(String)
    exec_ord_no = Column(String)
    reanly_exec_yn = Column(String)
    exec_cond_yn = Column(String)
    exec_pgm_id = Column(String)
    mdfy_date = Column(DateTime)
    mdfy_user = Column(String)
    ip_addr = Column(String)
    del_yn = Column(String)
    parser_id = Column(String)


class Ais8800(Base):
    __tablename__ = 'ais8800'
    pgm_grp_id = Column(String, primary_key=True)
    aval_ed_dt = Column(String, primary_key=True)
    aval_st_dt = Column(String)
    pgm_grp_nm = Column(String)
    pgm_grp_desc = Column(String)
    lia_type_cd = Column(String)
    exec_ord_no = Column(String)
    reanly_exec_yn = Column(String)
    exec_cond_yn = Column(String)
    exec_pgm_id = Column(String)
    mdfy_date = Column(DateTime)
    mdfy_user = Column(String)
    ip_addr = Column(String)
    del_yn = Column(String)
    parser_id = Column(String)


Session = sessionmaker(bind=engine)


def execute_query(table_name: str, query: str) -> None:
    session = Session()
    try:
        table_class = globals()[table_name.capitalize()]
        query = query.replace('ais8801', table_name)
        session.execute(query)
    except:
        session.rollback()
        raise
    else:
        session.commit()
    finally:
        session.close()


if __name__ == '__main__':
    table_name = 'ais8801'
    update_record = update(Ais8801).where(Ais8801.pgm_grp_id == '2', Ais8801.aval_ed_dt == '1').values(aval_ed_dt='2')
    insert_record = Ais8801(pgm_grp_id='2', aval_ed_dt='1', aval_st_dt='1', pgm_grp_nm='1', pgm_grp_desc='1',
                            lia_type_cd='1', exec_ord_no='1', reanly_exec_yn='1', exec_cond_yn='1', exec_pgm_id='1',
                            mdfy_date=datetime.now(), mdfy_user='1', ip_addr='1', del_yn='1', parser_id='1')
    execute_query(table_name, "INSERT INTO ais8801 VALUES ('2', '1', '1', '1', '1', '1', '1',

## 사용자 입력:
> from sqlalchemy import create_engine, Column, Integer, String, DateTime,update
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime

# engine = create_engine("oracle://LIAUSR:LIAUSR@192.168.0.160:1521/oracle")
engine = create_engine("postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres")
Base = declarative_base()

class Ais8801(Base):
    __tablename__ = 'ais8801'
    pgm_grp_id = Column(String, primary_key=True)
    aval_ed_dt = Column(String, primary_key=True)
    aval_st_dt = Column(String)
    pgm_grp_nm = Column(String)
    pgm_grp_desc = Column(String)
    lia_type_cd = Column(String)
    exec_ord_no = Column(String)
    reanly_exec_yn = Column(String)
    exec_cond_yn = Column(String)
    exec_pgm_id = Column(String)
    mdfy_date = Column(DateTime)
    mdfy_user = Column(String)
    ip_addr = Column(String)
    del_yn = Column(String)
    parser_id = Column(String)


Session = sessionmaker(bind=engine)

ais8801_update_record = update(Ais8801).where(Ais8801.pgm_grp_id=='2', Ais8801.aval_ed_dt=='1').values(aval_ed_dt ='2')
ais8801_insert_record = Ais8801(pgm_grp_id='2',aval_ed_dt='1',aval_st_dt='1',pgm_grp_nm='1',pgm_grp_desc='1',lia_type_cd='1',exec_ord_no ='1'
                         ,reanly_exec_yn='1',exec_cond_yn='1',exec_pgm_id='1',mdfy_date=datetime.now(),mdfy_user='1',ip_addr='1',del_yn='1',parser_id='1')
with Session() as session:
    try:
        session.add(ais8801_insert_record)
        session.execute(ais8801_update_record)
    except:
        session.rollback()
        raise
    else:
        session.commit()
 위 프로그램을 argparse를 이용하여 매개변수로 ais8801,ais8800 테이블명을 받아서 동적테이블명을 바꾸는 프로그램으로 만들어줘

## ChatGPT 응답:
> 아래는 argparse를 이용하여 ais8801,ais8800 테이블명을 받아서 동적테이블명을 바꾸는 프로그램입니다.

```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime,update
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime
import argparse

parser = argparse.ArgumentParser(description='Update or insert records into a database table.')
parser.add_argument('table_name', type=str, help='Name of the database table to update or insert records into.')
args = parser.parse_args()

# Create database engine and session
db_url = "postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres"
engine = create_engine(db_url)
Base = declarative_base()
Session = sessionmaker(bind=engine)

# Define the database table schema
class DynamicTable(Base):
    __tablename__ = args.table_name
    pgm_grp_id = Column(String, primary_key=True)
    aval_ed_dt = Column(String, primary_key=True)
    aval_st_dt = Column(String)
    pgm_grp_nm = Column(String)
    pgm_grp_desc = Column(String)
    lia_type_cd = Column(String)
    exec_ord_no = Column(String)
    reanly_exec_yn = Column(String)
    exec_cond_yn = Column(String)
    exec_pgm_id = Column(String)
    mdfy_date = Column(DateTime)
    mdfy_user = Column(String)
    ip_addr = Column(String)
    del_yn = Column(String)
    parser_id = Column(String)

# Define the update and insert operations
update_record = update(DynamicTable).where(DynamicTable.pgm_grp_id=='2', DynamicTable.aval_ed_dt=='1').values(aval_ed_dt ='2')
insert_record = DynamicTable(pgm_grp_id='2',aval_ed_dt='1',aval_st_dt='1',pgm_grp_nm='1',pgm_grp_desc='1',lia_type_cd='1',exec_ord_no ='1'
                         ,reanly_exec_yn='1',exec_cond_yn='1',exec_pgm_id='1',mdfy_date=datetime.now(),mdfy_user='1',ip_addr='1',del_yn='1',parser_id='1')

# Perform the update or insert operation in the database
with Session() as session:
    try:
        session.add(insert_record)
        session.execute(update_record)
    except:
        session.rollback()
        raise
    else:
        session.commit()
``` 

위의 프로그램에서, argparse를 사용하여 프로그램 실행 시 매개변수로 table_name을 입력받도록 하였습니다. 이렇게 하면 프로그램 실행 시 어떤 테이블을 대상으로 update 또는 insert 작업을 수행할지 선택할 수 있습니다. 그리고 table_name 변수를 이용하여 DynamicTable 클래스의 __tablename__ 변수를 동적으로 설정하였습니다.

나머지 코드는 원래 코드와 동일합니다. 위 프로그램을 실행할 때는 아래와 같이 실행하면 됩니다.

```python
python program_name.py ais8801
```

위 예시는 program_name.py라는 파일명으로 프로그램을 저장한 후, ais8801 테이블을 대상으로 실행하는 것을 가정한 것입니다. 만약 ais8800 테이블을 대상

## 사용자 입력:
> 계속

## ChatGPT 응답:
> 으로 실행하려면 아래와 같이 입력하면 됩니다.

```python
python program_name.py ais8800
```

이렇게 하면 프로그램 실행 시 매개변수로 받은 테이블명에 해당하는 테이블을 대상으로 update 또는 insert 작업을 수행할 수 있습니다.

## 사용자 입력:
> if __name__ == "__main__": 이게 없어도 매개변수를 받을 수 있나?

## ChatGPT 응답:
> `if __name__ == "__main__":` 구문은 파이썬에서 모듈을 실행할 때 main 함수를 실행하기 위한 관례적인 구문입니다. 이 구문은 현재 모듈이 다른 모듈에 의해 import 되어 사용될 때는 실행되지 않으며, 오직 해당 모듈이 직접 실행될 때만 실행됩니다. 

따라서, `if __name__ == "__main__":` 구문이 없어도 매개변수를 받는 프로그램을 만들 수 있습니다. 하지만, 모듈을 다른 곳에서 import하여 사용할 때도 프로그램이 실행되어버리는 문제가 발생할 수 있습니다. 

따라서, `if __name__ == "__main__":` 구문은 프로그램을 모듈 형태로 사용할 때를 대비하여 반드시 작성하는 것이 좋습니다. `if __name__ == "__main__":` 구문을 작성하면 프로그램을 모듈 형태로 사용할 때는 main 함수가 실행되지 않으며, 프로그램을 직접 실행할 때만 main 함수가 실행됩니다.

## 사용자 입력:
> if __name__ == "__main__": 을 넣어서 위에 프로그램을 완성해줘

## ChatGPT 응답:
> 아래는 `if __name__ == "__main__":` 구문을 추가하여 argparse를 이용하여 매개변수로 ais8801,ais8800 테이블명을 받아서 동적테이블명을 바꾸는 프로그램입니다.

```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime,update
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime
import argparse

def main(table_name):
    # Create database engine and session
    db_url = "postgresql://LIAUSR:LIAUSR@192.168.0.160:5432/postgres"
    engine = create_engine(db_url)
    Base = declarative_base()
    Session = sessionmaker(bind=engine)

    # Define the database table schema
    class DynamicTable(Base):
        __tablename__ = table_name
        pgm_grp_id = Column(String, primary_key=True)
        aval_ed_dt = Column(String, primary_key=True)
        aval_st_dt = Column(String)
        pgm_grp_nm = Column(String)
        pgm_grp_desc = Column(String)
        lia_type_cd = Column(String)
        exec_ord_no = Column(String)
        reanly_exec_yn = Column(String)
        exec_cond_yn = Column(String)
        exec_pgm_id = Column(String)
        mdfy_date = Column(DateTime)
        mdfy_user = Column(String)
        ip_addr = Column(String)
        del_yn = Column(String)
        parser_id = Column(String)

    # Define the update and insert operations
    update_record = update(DynamicTable).where(DynamicTable.pgm_grp_id=='2', DynamicTable.aval_ed_dt=='1').values(aval_ed_dt ='2')
    insert_record = DynamicTable(pgm_grp_id='2',aval_ed_dt='1',aval_st_dt='1',pgm_grp_nm='1',pgm_grp_desc='1',lia_type_cd='1',exec_ord_no ='1'
                             ,reanly_exec_yn='1',exec_cond_yn='1',exec_pgm_id='1',mdfy_date=datetime.now(),mdfy_user='1',ip_addr='1',del_yn='1',parser_id='1')

    # Perform the update or insert operation in the database
    with Session() as session:
        try:
            session.add(insert_record)
            session.execute(update_record)
        except:
            session.rollback()
            raise
        else:
            session.commit()

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Update or insert records into a database table.')
    parser.add_argument('table_name', type=str, help='Name of the database table to update or insert records into.')
    args = parser.parse_args()

    main(args.table_name)
```

위와 같이 `if __name__ == "__main__":` 구문을 이용하여 main 함수를 정의하고, argparse를 이용하여 매개변수로 테이블명을 입력받도록 하였습니다. 이렇게 하면 프로그램을 실행할 때 입력받은 테이블명에 해당하는 테이블을 대상으로 update 또는 insert 작업을 수행할 수 있습니다.

프로그램을 실행할 때는 아래와 같이 입력하면 됩니다.

```python
python program_name.py ais8801
```

위 예시는 program_name.py라는 파일명으로 프로그램을 저장한 후

## 사용자 입력:
> python에서 void 함수로 class선언을 공유할 수 있나?

## ChatGPT 응답:
> Python에서 함수는 일급 객체이므로, 함수를 인자로 전달하거나 반환값으로 사용할 수 있습니다. 따라서 void 함수로 클래스를 선언하고, 이를 다른 함수에서 사용할 수 있습니다.

아래는 예시 코드입니다.

```python
# Define void function to declare class
def create_class():
    class MyClass:
        def __init__(self, x, y):
            self.x = x
            self.y = y

        def add(self):
            return self.x + self.y

    # Return the class
    return MyClass

# Use the class in another function
def use_class():
    MyClass = create_class()
    instance = MyClass(1, 2)
    print(instance.add())

# Call the function
use_class()
```

위 코드에서는 create_class() 함수에서 MyClass 클래스를 선언하고, 이를 반환합니다. use_class() 함수에서는 create_class() 함수를 호출하여 반환된 MyClass 클래스를 사용합니다.

따라서, Python에서 void 함수로 클래스를 선언하고, 이를 다른 함수에서 사용하는 것은 가능합니다.

## 사용자 입력:
> postgresql truncate가 오래 걸리는 이유는?

## ChatGPT 응답:
> PostgreSQL에서 TRUNCATE는 테이블의 모든 데이터를 삭제하는 명령어입니다. TRUNCATE는 DELETE 명령어와 달리 테이블 전체를 한 번에 삭제하므로, 많은 데이터가 있는 테이블에서는 삭제 작업이 오래 걸릴 수 있습니다.

TRUNCATE가 오래 걸리는 이유는 여러 가지가 있을 수 있지만, 대표적인 이유는 다음과 같습니다.

1. 트랜잭션 로그
TRUNCATE는 테이블 전체를 삭제하는 작업이므로, 데이터가 많을 경우 트랜잭션 로그가 커지게 됩니다. 이로 인해 디스크 I/O 작업이 많아져서 작업 시간이 길어질 수 있습니다.

2. Lock
TRUNCATE는 테이블 전체를 삭제하는 작업이므로, 테이블의 lock이 발생합니다. 만약 다른 사용자가 테이블을 사용하고 있을 경우 lock 때문에 작업이 지연될 수 있습니다.

3. Index
TRUNCATE는 테이블의 모든 데이터를 삭제하므로, 인덱스도 함께 삭제됩니다. 인덱스를 재생성하는 작업 때문에 작업 시간이 더 길어질 수 있습니다.

따라서, TRUNCATE는 대용량 데이터를 삭제하는 경우 사용에 주의해야 합니다. TRUNCATE 대신 DELETE를 사용하여 조건에 맞는 데이터만 삭제하거나, DROP과 CREATE를 사용하여 테이블을 재생성하는 방법을 고려할 수도 있습니다.

## 사용자 입력:
> delete가 truncate보다 오래 걸리나?

## ChatGPT 응답:
> DELETE와 TRUNCATE는 모두 테이블의 데이터를 삭제하는 명령어입니다. 그러나 DELETE와 TRUNCATE는 내부적으로 다른 작업 방식을 사용하므로, 삭제 작업에 걸리는 시간이 다를 수 있습니다.

DELETE는 테이블의 각 행(row)을 하나씩 삭제하는 작업입니다. 이 때문에 DELETE는 TRUNCATE에 비해 더 많은 시간이 걸릴 수 있습니다. 또한 DELETE는 삭제된 데이터를 롤백할 수 있기 때문에 트랜잭션 로그 파일이 많아질 가능성이 있습니다.

반면 TRUNCATE는 테이블을 재생성하는 것과 같은 방식으로 테이블 전체를 삭제합니다. TRUNCATE는 데이터를 하나씩 삭제하지 않고 테이블 자체를 재구성하므로, DELETE에 비해 더 빠르게 작업을 수행할 수 있습니다. 또한 TRUNCATE는 롤백이 불가능하며, 작업 후 바로 공간을 반환하므로 트랜잭션 로그 파일의 크기가 작아질 가능성이 있습니다.

따라서, 작업 방식에 따라 DELETE와 TRUNCATE의 속도 차이가 발생할 수 있으며, 대용량 데이터를 삭제하는 경우 TRUNCATE를 사용하는 것이 DELETE보다 더 빠를 수 있습니다. 그러나 TRUNCATE는 데이터를 완전히 삭제하는 것이므로, 데이터의 복구가 불가능하다는 점에 주의해야 합니다.

