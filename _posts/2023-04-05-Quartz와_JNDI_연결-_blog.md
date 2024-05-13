---
layout: post
title: "Quartz와 JNDI 연결."
date: 2023-04-05
tags: [JNDI, 연결]
categories: blog
---

## 사용자 입력:
> jndi를 쓰게 되면, quartz가 바라보는 db연결 driver는 어디껄 쓰나?

## ChatGPT 응답:
> Quartz는 일반적으로 JNDI(Java Naming and Directory Interface)를 사용하여 데이터베이스에 연결합니다. JNDI는 미리 정의된 데이터베이스 연결 풀에 대한 참조를 제공하므로 Quartz는 데이터베이스 연결에 필요한 모든 정보를 알 필요가 없습니다.

JNDI를 사용할 때는 일반적으로 서버의 컨텍스트 XML 파일에 데이터베이스 드라이버 및 연결 정보를 구성하게 됩니다. 따라서 Quartz가 바라보는 데이터베이스 드라이버는 JNDI에서 구성한 드라이버와 동일하게 설정해야 합니다.

JNDI를 사용하는 방법에 대한 자세한 내용은 서버 또는 애플리케이션 서버의 문서를 참조하시기 바랍니다.

