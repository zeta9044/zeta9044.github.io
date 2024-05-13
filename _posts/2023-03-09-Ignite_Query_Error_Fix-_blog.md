---
layout: post
title: "Ignite Query Error Fix."
tags: [Fix, Ignite, Error, Query]
---

## 사용자 입력:
> javax.cache.CacheException: Failed to execute query. Add module 'ignite-indexing' to the classpath of all Ignite nodes or configure any query engine.

## ChatGPT 응답:
> The exception you are seeing indicates that Ignite is unable to execute a cache query due to the absence of the 'ignite-indexing' module in the classpath of all the Ignite nodes. This module provides support for indexing and querying data stored in Ignite caches.

To resolve this issue, you have two options:

1. Add the 'ignite-indexing' module to the classpath of all Ignite nodes:

   You can download the 'ignite-indexing' module from the Apache Ignite website (https://ignite.apache.org/download.cgi#binaries) and add it to the classpath of all the Ignite nodes. This can be done by adding the following line to the 'IGNITE_HOME/config/default-config.xml' file:

   {% raw %}```xml
   <property name="peerClassLoadingEnabled" value="true"/>
   ```{% endraw %}
   
   This enables peer class loading, which allows nodes to share classes and resources with each other.

2. Configure a query engine:

   If you do not want to add the 'ignite-indexing' module to the classpath of all Ignite nodes, you can configure a query engine instead. Ignite provides two query engines: the SQL engine and the Lucene engine. The SQL engine provides support for SQL queries, while the Lucene engine provides support for full-text searches.

   To configure a query engine, you need to add the corresponding dependency to your project's classpath and configure the query engine in the 'IGNITE_HOME/config/default-config.xml' file. Here's an example configuration for the SQL engine:

   {% raw %}```xml
   <property name="cacheConfiguration">
       <list>
           <bean class="org.apache.ignite.configuration.CacheConfiguration">
               <property name="name" value="myCache"/>
               <property name="queryEntities">
                   <list>
                       <bean class="org.apache.ignite.cache.QueryEntity">
                           <property name="keyType" value="java.lang.Integer"/>
                           <property name="valueType" value="com.example.MyValue"/>
                           <property name="fields">
                               <map>
                                   <entry key="id" value="java.lang.Integer"/>
                                   <entry key="name" value="java.lang.String"/>
                               </map>
                           </property>
                           <property name="indexes">
                               <list>
                                   <bean class="org.apache.ignite.cache.QueryIndex">
                                       <constructor-arg value="name"/>
                                   </bean>
                               </list>
                           </property>
                       </bean>
                   </list>
               </property>
           </bean>
       </list>
   </property>
   ```{% endraw %}
   
   This configuration defines a cache named 'myCache' with a single query entity that maps keys of type 'java.lang.Integer' to values of type 'com.example.MyValue'. The query entity has two fields ('id' and 'name') and one index ('name'). Once you have defined the cache configuration, you can use the Ignite SQL API to execute SQL queries on the cache.

## 사용자 입력:
>             IgniteConfiguration igniteCfg = new IgniteConfiguration();
            igniteCfg.setClientMode(true);
            Ignite ignite = Ignition.start(igniteCfg)
            IgniteCache<String, EdgeSerializable> edgeCache = ignite.getOrCreateCache("edge");
            SqlFieldsQuery qry =  new SqlFieldsQuery("select * from \"edge\".EDGESERIALIZABLE");
            QueryCursor<List<?>> cursor  = edgeCache.query(qry);

            int resultHandlerCnt = 0;
            int resultHandlerCnt2 = 0;

            for (List<?> row : cursor) {
                graph.addEdge(row.get(1));

                resultHandlerCnt++;
                resultHandlerCnt2++;
                if (resultHandlerCnt2 % 10000 == 0 || resultHandlerCnt2 == 1) {
                    logger.debug("edgeSize / resultCount : {} / {}", graph.getEdges().size(), resultHandlerCnt2);
                    /* 흐름데이터 적재 로그 갱신 */
                    qtrackHashService.updateHashLog("02", (int) Math.floor((double) resultHandlerCnt2 / (halfHashCnt > resultHandlerCnt2 ? halfHashCnt : resultHandlerCnt2 + 1) * 50));
                }
            }

            cursor.close();
            ignite.close();  위 코드에서 sql query를 쓰지 않고 EdgeSeriable을 graph에 add하는 프로그램으로 바꿔줘

## ChatGPT 응답:
> Sure, here's an updated code snippet that avoids using SQL queries and instead adds {% raw %}`EdgeSerializable`{% endraw %} objects to a graph:

{% raw %}```
IgniteConfiguration igniteCfg = new IgniteConfiguration();
igniteCfg.setClientMode(true);
Ignite ignite = Ignition.start(igniteCfg);

IgniteCache<String, EdgeSerializable> edgeCache = ignite.getOrCreateCache("edge");
int resultHandlerCnt2 = 0;

for (Cache.Entry<String, EdgeSerializable> entry : edgeCache) {
    EdgeSerializable edge = entry.getValue();
    graph.addEdge(edge);

    resultHandlerCnt2++;
    if (resultHandlerCnt2 % 10000 == 0 || resultHandlerCnt2 == 1) {
        logger.debug("edgeSize / resultCount : {} / {}", graph.getEdges().size(), resultHandlerCnt2);
        /* 흐름데이터 적재 로그 갱신 */
        qtrackHashService.updateHashLog("02", (int) Math.floor((double) resultHandlerCnt2 / (halfHashCnt > resultHandlerCnt2 ? halfHashCnt : resultHandlerCnt2 + 1) * 50));
    }
}

ignite.close();
```{% endraw %}

This code retrieves all entries from the {% raw %}`edgeCache`{% endraw %} cache and adds each {% raw %}`EdgeSerializable`{% endraw %} object to the {% raw %}`graph`{% endraw %}. Note that this code assumes that the {% raw %}`graph`{% endraw %} object has already been initialized and that the {% raw %}`EdgeSerializable`{% endraw %} class has a constructor that takes a {% raw %}`Cache.Entry<String, EdgeSerializable>`{% endraw %} object as its argument. You may need to modify this code to fit your specific use case.

## 사용자 입력:
> 2023-03-09 10:49:25.305 ERROR 10428 --- [ taskExecutor-1] .a.i.SimpleAsyncUncaughtExceptionHandler : Unexpected error occurred invoking async method: public void com.datastreams.qtrack.common.HashInitializer.initializeHash(org.springframework.context.event.ContextRefreshedEvent)

groovy.lang.MissingMethodException: No signature of method: com.datastreams.qtrack.graph.bean.Graph.addEdge() is applicable for argument types: (com.datastreams.qtrack.graph.bean.EdgeSerializable) values: [EdgeSerializable{seqId=880340, condMapping=1, src_caps_col_name='SALES_AMT', src_caps_col_name_org='SALES_AMT', src_col_order_no=7, src_col_value_yn='N', src_owner_tgt_srv_id='DIP_DIP_DW', src_owner_name='HDI_DW', src_caps_table_name='TWF_SST_CNTRY_GAME_TEMP', src_table_name_org='null', src_table_name_view='TWF_SST_CNTRY_GAME_TEMP', src_table_type=tbl, src_table_type_detail='tbl', table_id=0, tgt_caps_col_name='SALES_AMT', tgt_caps_col_name_org='SALES_AMT', tgt_col_order_no=7, tgt_col_value_yn='N', tgt_owner_tgt_srv_id='DIP_DIP_DW', tgt_owner_name='HDI_DW', tgt_caps_table_name='TWF_SST_CNTRY_GAME', tgt_table_name_org='null', tgt_table_name_view='TWF_SST_CNTRY_GAME', tgt_table_type=tbl, tgt_table_type_detail='tbl', call_table_id=0', src_system_biz_id=HDI_DW', tgt_system_biz_id=HDI_DW', src_system_id=HDI', tgt_system_id=HDI', src_system_nm=SGH-DI', tgt_system_nm=SGH-DI', src_biz_id=DW', tgt_biz_id=DW', src_biz_nm=DW', tgt_biz_nm=DW', src_db_instance_org=DIP', tgt_db_instance_org=DIP', src_schema_org=DW', tgt_schema_org=DW}]
Possible solutions: addEdge(com.datastreams.qtrack.graph.bean.Edge), addEdge(com.datastreams.qtrack.graph.bean.Node, com.datastreams.qtrack.graph.bean.Node), addEdge(com.datastreams.qtrack.graph.bean.Node, com.datastreams.qtrack.graph.bean.Node, com.datastreams.qtrack.graph.bean.Edge), getEdges()
	at org.codehaus.groovy.runtime.ScriptBytecodeAdapter.unwrap(ScriptBytecodeAdapter.java:58) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.PogoMetaClassSite.call(PogoMetaClassSite.java:54) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCall(CallSiteArray.java:48) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:113) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:125) ~[groovy-all-2.4.13.jar:2.4.13]
	at com.datastreams.qtrack.graph.service.GraphServiceImpl.enscureGraph(GraphServiceImpl.groovy:2600) ~[qtrack-server-3.2.1.jar:3.2.1-44525518]
	at com.datastreams.qtrack.graph.service.GraphServiceImpl.hashInit(GraphServiceImpl.groovy:2508) ~[qtrack-server-3.2.1.jar:3.2.1-44525518]
	at com.datastreams.qtrack.graph.service.GraphService$hashInit.call(Unknown Source) ~[na:na]
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCall(CallSiteArray.java:48) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:113) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:117) ~[groovy-all-2.4.13.jar:2.4.13]
	at com.datastreams.qtrack.common.HashInitializer.initializeHash(HashInitializer.groovy:18) ~[qtrack-server-3.2.1.jar:3.2.1-44525518]
	at com.datastreams.qtrack.common.HashInitializer$$FastClassBySpringCGLIB$$4b34e73.invoke(<generated>) ~[qtrack-server-3.2.1.jar:3.2.1-44525518]
	at org.springframework.cglib.proxy.MethodProxy.invoke(MethodProxy.java:204) ~[spring-core-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.invokeJoinpoint(CglibAopProxy.java:736) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:157) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.aop.interceptor.AsyncExecutionInterceptor$1.call(AsyncExecutionInterceptor.java:115) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at java.util.concurrent.FutureTask.run(FutureTask.java:266) [na:1.8.0_202]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [na:1.8.0_202]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [na:1.8.0_202]
	at java.lang.Thread.run(Thread.java:748) [na:1.8.0_202]

## ChatGPT 응답:
> The error message is indicating that the {% raw %}`addEdge`{% endraw %} method in the {% raw %}`com.datastreams.qtrack.graph.bean.Graph`{% endraw %} class is not applicable for the argument type {% raw %}`com.datastreams.qtrack.graph.bean.EdgeSerializable`{% endraw %}. 

One possible solution is to modify the {% raw %}`Graph`{% endraw %} class to accept {% raw %}`EdgeSerializable`{% endraw %} objects instead of {% raw %}`Edge`{% endraw %} objects. Alternatively, you could create a new {% raw %}`Edge`{% endraw %} object based on the {% raw %}`EdgeSerializable`{% endraw %} object and then pass that to the {% raw %}`addEdge`{% endraw %} method.

Here's an example of how you could modify the {% raw %}`Graph`{% endraw %} class to accept {% raw %}`EdgeSerializable`{% endraw %} objects:

{% raw %}```groovy
class Graph {
    List<EdgeSerializable> edges = []

    void addEdge(EdgeSerializable edge) {
        edges.add(edge)
    }

    // Other methods...
}
```{% endraw %}

Once you have made this modification, you should be able to pass {% raw %}`EdgeSerializable`{% endraw %} objects to the {% raw %}`addEdge`{% endraw %} method without any issues.

