---
layout: post
title: "추출된 JSON 키 - 'src.kafka.bootstrap.servers'"
tags: [servers, JSON, src, bootstrap, 추출, 키, kafka]
---

## 사용자 입력:
> oracle 19C에서 clob컬럼에 json을 넣었어. 예시한 값은 {"connector.class":"io.confluent.connect.replicator.ReplicatorSourceConnector","dest.kafka.sasl.mechanism":"SCRAM-SHA-256","src.kafka.sasl.jaas.config":"org.apache.kafka.common.security.scram.ScramLoginModule required username=\"dtlk_app\" password=\"BpyfKTzz7ZkJZkDH\";","src.consumer.max.poll.interval.ms":"900000","tasks.max":"4","group.id":"connect-cluster","producer.override.security.protocol":"SASL_PLAINTEXT","src.kafka.security.protocol":"SASL_PLAINTEXT","dest.kafka.security.protocol":"SASL_PLAINTEXT","topic.config.sync":"false","src.consumer.max.poll.records":"100","src.consumer.sasl.jaas.config":"org.apache.kafka.common.security.scram.ScramLoginModule required username=\"dtlk_app\" password=\"BpyfKTzz7ZkJZkDH\";","confluent.topic.replication.factor":"3","value.converter":"io.confluent.connect.replicator.util.ByteArrayConverter","key.converter":"io.confluent.connect.replicator.util.ByteArrayConverter","topic.whitelist":"from_tvctc_etc_message","producer.override.compression.type":"lz4","offset.timestamps.commit":"false","src.consumer.sasl.mechanism":"SCRAM-SHA-256","dest.kafka.request.timeout.ms":"60000","header.converter":"io.confluent.connect.replicator.util.ByteArrayConverter","src.kafka.sasl.mechanism":"SCRAM-SHA-256","dest.kafka.session.timeout.ms":"30000","src.consumer.group.id":"cg_dtlk_from_tvctc_etc_message","dest.kafka.bootstrap.servers":"172.31.94.131:9092, 172.31.94.132:9092, 172.31.94.130:9092, 172.31.94.145:9092, 172.31.94.146:9092, 172.31.94.147:9092","name":"rp_from_tvctc_etc_message","src.consumer.security.protocol":"SASL_PLAINTEXT","producer.override.sasl.jaas.config":"org.apache.kafka.common.security.scram.ScramLoginModule required username=\"kafkaadm\" password=\"edp2021\";","producer.override.sasl.mechanism":"SCRAM-SHA-256","provenance.header.enable":"true","src.kafka.bootstrap.servers":"172.23.2.27:9092, 172.23.2.28:9092, 172.23.2.29:9092","dest.kafka.sasl.jaas.config":"org.apache.kafka.common.security.scram.ScramLoginModule required username=\"kafkaadm\" password=\"edp2021\";"} 이거야.. 그런데, 여기서, key가 "src.kafka.bootstrap.servers"인 value를 추출하는 쿼리를 만들어줘.

## ChatGPT 응답:
> Oracle 19C에서 CLOB 컬럼에 저장된 JSON 데이터에서 "src.kafka.bootstrap.servers" 키에 해당하는 값을 추출하려면 Oracle의 JSON 함수를 사용하여 JSON 데이터를 쿼리해야 합니다. 다음은 이 작업을 수행하는 쿼리입니다:

{% raw %}```sql
SELECT JSON_VALUE(your_clob_column, '$.src.kafka.bootstrap.servers') AS src_kafka_bootstrap_servers
FROM your_table_name;
```{% endraw %}

위 쿼리에서:

- {% raw %}`your_clob_column`{% endraw %}: JSON 데이터가 저장된 CLOB 컬럼의 이름을 나타냅니다.
- {% raw %}`your_table_name`{% endraw %}: 데이터를 저장하고 있는 테이블의 이름을 나타냅니다.

이 쿼리는 JSON 데이터에서 "src.kafka.bootstrap.servers" 키에 해당하는 값을 추출하고, "src_kafka_bootstrap_servers"라는 별칭을 사용하여 결과를 반환합니다.

실제 테이블 이름과 컬럼 이름을 적절하게 변경하여 사용하십시오.

## 사용자 입력:
> '$.src.kafka.bootstrap.servers' 표시가 맞아?

## ChatGPT 응답:
> 죄송합니다, 'JSON_VALUE' 함수에서 JSON 경로 문자열은 단순 문자열로 표현되어야 합니다. '$.src.kafka.bootstrap.servers'는 경로 문자열이 아닙니다. 올바른 JSON 경로 문자열은 다음과 같이 단순한 키 이름으로 표현되어야 합니다:

{% raw %}```sql
SELECT JSON_VALUE(your_clob_column, '$.src.kafka.bootstrap.servers') AS src_kafka_bootstrap_servers
FROM your_table_name;
```{% endraw %}

위의 SQL 쿼리에서는 '$.src.kafka.bootstrap.servers'를 사용하여 JSON 데이터에서 "src.kafka.bootstrap.servers" 키의 값을 추출합니다.

