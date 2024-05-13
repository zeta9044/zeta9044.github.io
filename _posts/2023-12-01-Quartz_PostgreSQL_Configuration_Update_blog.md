---
layout: post
title: "Quartz PostgreSQL Configuration Update"
date: 2023-12-01
tags: [Quartz, Update, Configuration, PostgreSQL]
categories: blog
---

## 사용자 입력:
> #============================================================================
# Configure Main Scheduler Properties  
#============================================================================
org.quartz.scheduler.instanceName = QTRACKScheduler
org.quartz.scheduler.instanceId = QTRACKRemoteScheduleServer
org.quartz.scheduler.skipUpdateCheck: true

#============================================================================
# Configure ThreadPool  
#============================================================================
org.quartz.threadPool.class = org.quartz.simpl.SimpleThreadPool
org.quartz.threadPool.threadCount = 5
org.quartz.threadPool.threadPriority = 5

#============================================================================
# Configure JobStore  
#============================================================================
org.quartz.jobStore.misfireThreshold = 60000
org.quartz.jobStore.class=org.quartz.impl.jdbcjobstore.JobStoreTX
org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.oracle.OracleDelegate
org.quartz.jobStore.useProperties=false
org.quartz.jobStore.dataSource=myDS
org.quartz.jobStore.tablePrefix=AIS_SCH_
org.quartz.jobStore.isClustered=true

#============================================================================
# Configure Datasources  
#============================================================================
org.quartz.dataSource.myDS.driver = oracle.jdbc.driver.OracleDriver
org.quartz.dataSource.myDS.URL = jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(HOST=192.168.0.160)(PORT=1521)(PROTOCOL=tcp))(CONNECT_DATA=(SERVICE_NAME=oracle)))
org.quartz.dataSource.myDS.maxConnections = 5
org.quartz.dataSource.myDS.validationQuery=select 0 from dual
org.quartz.dataSource.myDS.provider=hikaricp  이건   quartz의  oracle세팅인데, postgresql 14버전 세팅으로 바꿔봐.

## ChatGPT 응답:
> Sure, I'll help you modify the Quartz Scheduler configuration settings to work with PostgreSQL 14. Here's how the configuration would look:

```properties
#============================================================================
# Configure Main Scheduler Properties  
#============================================================================
org.quartz.scheduler.instanceName = QTRACKScheduler
org.quartz.scheduler.instanceId = QTRACKRemoteScheduleServer
org.quartz.scheduler.skipUpdateCheck: true

#============================================================================
# Configure ThreadPool  
#============================================================================
org.quartz.threadPool.class = org.quartz.simpl.SimpleThreadPool
org.quartz.threadPool.threadCount = 5
org.quartz.threadPool.threadPriority = 5

#============================================================================
# Configure JobStore  
#============================================================================
org.quartz.jobStore.misfireThreshold = 60000
org.quartz.jobStore.class=org.quartz.impl.jdbcjobstore.JobStoreTX
org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.StdJDBCDelegate  # Changed to standard JDBC delegate for PostgreSQL
org.quartz.jobStore.useProperties=false
org.quartz.jobStore.dataSource=myDS
org.quartz.jobStore.tablePrefix=AIS_SCH_
org.quartz.jobStore.isClustered=true

#============================================================================
# Configure Datasources for PostgreSQL  
#============================================================================
org.quartz.dataSource.myDS.driver = org.postgresql.Driver  # PostgreSQL driver
org.quartz.dataSource.myDS.URL = jdbc:postgresql://192.168.0.160:5432/yourDatabaseName  # PostgreSQL URL, update with your database name
org.quartz.dataSource.myDS.maxConnections = 5
org.quartz.dataSource.myDS.validationQuery=select 1  # Changed to a simple select for PostgreSQL
org.quartz.dataSource.myDS.provider=hikaricp
```

Make sure to replace `yourDatabaseName` with the actual name of your PostgreSQL database. Additionally, verify the IP address and port number (`192.168.0.160:5432`) to ensure they match your PostgreSQL server's settings.

## 사용자 입력:
> org.quartz.jobStore.driverDelegateClass이거 postgresql 용으로 안바꿔도 돼?

## ChatGPT 응답:
> 네, `org.quartz.jobStore.driverDelegateClass` 설정에 대해 설명드리겠습니다. Quartz 스케줄러에서 `driverDelegateClass`는 데이터베이스와의 상호작용을 관리하는 데 사용됩니다. PostgreSQL을 사용할 때 일반적으로 `org.quartz.impl.jdbcjobstore.StdJDBCDelegate`를 사용합니다. 이 클래스는 다양한 데이터베이스 시스템과 호환되며, PostgreSQL과도 잘 작동합니다.

따라서, Oracle 설정에서 사용된 `org.quartz.impl.jdbcjobstore.oracle.OracleDelegate` 대신에 `org.quartz.impl.jdbcjobstore.StdJDBCDelegate`를 사용하는 것이 적합합니다. 이미 제가 이전에 보낸 설정에서 이 부분을 수정해 드렸습니다.

```properties
org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.StdJDBCDelegate
```

이 변경을 통해 Quartz 스케줄러가 PostgreSQL 데이터베이스와 올바르게 통신할 수 있습니다. 기타 설정은 PostgreSQL 데이터베이스 환경에 맞게 조정해야 할 수도 있습니다.

## 사용자 입력:
> 아니야..전용으로 해줘. quartz 라이브러리 안에 postgresql용이 따로 있어.

## ChatGPT 응답:
> 알겠습니다, Quartz 스케줄러 설정에서 PostgreSQL 전용 `driverDelegateClass`를 사용하시려면, `org.quartz.impl.jdbcjobstore.PostgreSQLDelegate`를 사용하시면 됩니다. 이 클래스는 PostgreSQL 데이터베이스에 최적화된 특수 기능과 동작을 제공합니다.

설정 파일을 다음과 같이 수정하세요:

```properties
org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.PostgreSQLDelegate
```

이렇게 설정하면 Quartz 스케줄러가 PostgreSQL 데이터베이스와 더 효과적으로 상호 작용할 수 있습니다. PostgreSQL 전용 delegate를 사용함으로써 데이터베이스와의 통신이 더욱 원활해지고, PostgreSQL의 특정 기능을 더 잘 활용할 수 있습니다.

