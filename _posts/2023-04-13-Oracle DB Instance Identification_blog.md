---
layout: post
title: "Oracle DB Instance Identification"
date: 2023-04-13
tags: [DB, Instance, Oracle, Identification]
categories: blog
---

## 사용자 입력:
> 오라클에서 db instance name과 db name는 어떻게 알지?

## ChatGPT 응답:
> Oracle에서 DB instance name은 해당 인스턴스에 대한 식별자입니다. 이 이름은 보통 ORACLE_SID 환경 변수에 저장되며, 데이터베이스를 시작할 때 설정됩니다.

DB name은 데이터베이스의 실제 이름을 의미합니다. 이 이름은 데이터베이스를 만들 때 지정됩니다. 데이터베이스가 생성되면 이 이름은 데이터베이스 컨트롤 파일에 저장됩니다.

DB instance name과 DB name은 보통 같은 이름으로 설정됩니다. 하지만 경우에 따라서는 DB instance name과 DB name이 다를 수도 있습니다. 이 경우, DB instance name은 데이터베이스를 구동하는 서버에서 유일하게 식별되는 이름이며, DB name은 클라이언트가 연결할 때 사용되는 데이터베이스의 이름입니다.

