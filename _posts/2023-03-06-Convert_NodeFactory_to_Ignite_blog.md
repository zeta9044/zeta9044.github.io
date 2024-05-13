---
layout: post
title: "Convert NodeFactory to Ignite"
tags: [Ignite, Convert, to, NodeFactory]
---

## 사용자 입력:
> package com.datastreams.qtrack.graph.bean;

import java.util.*;

public class NodeFactory{

    private static final Map<Node, Node> MAP = new HashMap<Node, Node>();

    public static Node get(Node node) {
        Node cached = MAP.get(node);
        if(cached == null){
            cached = node;
            MAP.put(cached, cached);
        }
        return cached;
    }

    public static Node contains(Node node) {
        return MAP.get(node);
    }

    public static void clear(){
        MAP.clear();
    }

    public static int size(){
        return MAP.size();
    }
}   위 코드를 apapche ignite 코드로 바꿔줘

## ChatGPT 응답:
> 

Apache Ignite is an in-memory computing platform that provides distributed caching and processing capabilities. To convert the above code to Apache Ignite, you can use Ignite's Cache API to store and retrieve Node objects. Here's an example implementation:

{% raw %}```
import org.apache.ignite.Ignite;
import org.apache.ignite.Ignition;
import org.apache.ignite.cache.CacheAtomicityMode;
import org.apache.ignite.cache.CacheMode;
import org.apache.ignite.configuration.CacheConfiguration;

public class NodeFactory {

    private static Ignite ignite = Ignition.start();

    private static final String CACHE_NAME = "node-cache";

    public static Node get(Node node) {
        Node cached = ignite.<Node, Node>getOrCreateCache(getCacheConfiguration()).get(node);
        if (cached == null) {
            cached = node;
            ignite.<Node, Node>getOrCreateCache(getCacheConfiguration()).put(cached, cached);
        }
        return cached;
    }

    public static Node contains(Node node) {
        return ignite.<Node, Node>getOrCreateCache(getCacheConfiguration()).get(node);
    }

    public static void clear() {
        ignite.<Node, Node>getOrCreateCache(getCacheConfiguration()).clear();
    }

    public static int size() {
        return ignite.<Node, Node>getOrCreateCache(getCacheConfiguration()).size();
    }

    private static CacheConfiguration<Node, Node> getCacheConfiguration() {
        CacheConfiguration<Node, Node> cfg = new CacheConfiguration<>(CACHE_NAME);
        cfg.setCacheMode(CacheMode.PARTITIONED);
        cfg.setAtomicityMode(CacheAtomicityMode.ATOMIC);
        return cfg;
    }
}
```{% endraw %}

In this implementation, we create an Ignite instance and use it to create a distributed cache using Ignite's Cache API. We set the cache configuration to be partitioned and use an atomic cache mode. 

In the {% raw %}`get()`{% endraw %} method, we use the {% raw %}`getOrCreateCache()`{% endraw %} method to get the cache with the given cache configuration, and then use the {% raw %}`get()`{% endraw %} method to retrieve the cached node object. If the node object is not found in the cache, we add it to the cache using the {% raw %}`put()`{% endraw %} method and return the new node object.

The {% raw %}`contains()`{% endraw %} method also uses the {% raw %}`getOrCreateCache()`{% endraw %} method to get the cache and then uses the {% raw %}`get()`{% endraw %} method to retrieve the cached node object. This method is used to check whether a node object is already cached.

The {% raw %}`clear()`{% endraw %} method uses the {% raw %}`clear()`{% endraw %} method on the cache to remove all cached objects, and the {% raw %}`size()`{% endraw %} method returns the size of the cache using the {% raw %}`size()`{% endraw %} method on the cache.

Overall, this implementation provides similar functionality to the original implementation, but with the added benefit of being able to distribute the cache across multiple nodes in a cluster using Apache Ignite.

## 사용자 입력:
> 원격 클라이언트에서 네가 만든 node-cache를 호출하려면 어떻게 코딩해야 돼?

## ChatGPT 응답:
> 원격 클라이언트에서 NodeFactory에 있는 node-cache를 호출하려면 Apache Ignite를 사용하여 클러스터를 구성하고, 클러스터 내의 노드들이 NodeFactory를 공유하도록 설정해야 합니다. 그리고 클라이언트에서도 Apache Ignite를 사용하여 노드들과 연결해야 합니다.

아래는 간단한 예제 코드입니다.

{% raw %}```
import org.apache.ignite.Ignite;
import org.apache.ignite.IgniteException;
import org.apache.ignite.Ignition;
import org.apache.ignite.configuration.CacheConfiguration;
import org.apache.ignite.configuration.IgniteConfiguration;
import org.apache.ignite.spi.discovery.tcp.TcpDiscoverySpi;
import org.apache.ignite.spi.discovery.tcp.ipfinder.vm.TcpDiscoveryVmIpFinder;

import java.util.Collections;

public class RemoteClient {
    
    private static final String CACHE_NAME = "node-cache";

    public static void main(String[] args) {
        // Define Ignite configuration.
        IgniteConfiguration cfg = new IgniteConfiguration();
        // Set discovery SPI to static IP finder.
        TcpDiscoverySpi discoSpi = new TcpDiscoverySpi();
        TcpDiscoveryVmIpFinder ipFinder = new TcpDiscoveryVmIpFinder();
        ipFinder.setAddresses(Collections.singletonList("127.0.0.1:47500..47509"));
        discoSpi.setIpFinder(ipFinder);
        cfg.setDiscoverySpi(discoSpi);
        // Start Ignite.
        Ignite ignite = Ignition.start(cfg);

        // Define cache configuration.
        CacheConfiguration<Node, Node> cacheCfg = new CacheConfiguration<>(CACHE_NAME);
        // Set cache mode to partitioned.
        cacheCfg.setCacheMode(CacheMode.PARTITIONED);
        // Set atomicity mode to atomic.
        cacheCfg.setAtomicityMode(CacheAtomicityMode.ATOMIC);

        // Get node cache from the cluster.
        IgniteCache<Node, Node> nodeCache = ignite.getOrCreateCache(cacheCfg);

        // Use the node cache.
        Node node1 = new Node(1);
        Node cachedNode1 = nodeCache.get(node1);
        System.out.println(cachedNode1);

        // Stop Ignite.
        Ignition.stop(true);
    }
}
```{% endraw %}

이 예제 코드에서는 IgniteConfiguration을 사용하여 Ignite 클러스터를 설정합니다. TcpDiscoveryVmIpFinder를 사용하여 클러스터 내의 노드들의 IP 주소를 지정하고, TcpDiscoverySpi를 사용하여 IP 주소를 검색합니다.

그리고 CacheConfiguration을 사용하여 NodeFactory에서 사용하는 node-cache를 설정합니다. CacheMode는 PARTITIONED, AtomicityMode는 ATOMIC으로 설정합니다.

클러스터 내의 노드들은 이와 같은 설정을 사용하여 Ignition.start() 메서드를 호출하여 Ignite 클러스터를 시작합니다.

클라이언트에서는 Ignite를 사용하여 클러스터 내의 노드들과 연결합니다. Ignite.getOrCreateCache() 메서드를 사용하여 클러스터 내의 node-cache를 가져오고, get() 메서드를 사용하여 노드 객체를 검색합니다.

참고로, 이 예제 코드는 로컬 모드로 실행됩니다. 만약 클러스터 내의 다른 노드들이 사용되어야 한다면, IgniteConfiguration과 CacheConfiguration의 속성을 적절

## 사용자 입력:
> apache ignite는 서버아냐? 왜 원격클라이언트가  // Start Ignite.
        Ignite ignite = Ignition.start(cfg); 를 꼭해야 하나?

## ChatGPT 응답:
> Apache Ignite는 분산 인 메모리 데이터 그리드(Distributed In-Memory Data Grid) 플랫폼으로, 클러스터를 구성하는 여러 노드가 서로 협력하여 데이터를 저장하고 처리합니다. 이러한 특징 때문에 Apache Ignite는 일종의 서버 역할을 합니다.

따라서 Apache Ignite를 사용하여 분산된 캐시나 데이터 처리 로직을 구현하려면 먼저 클러스터를 구성해야 합니다. 이를 위해 IgniteConfiguration을 설정하고, Ignition.start() 메서드를 호출하여 Ignite 인스턴스를 시작합니다. 그리고 이 인스턴스를 사용하여 분산된 캐시나 데이터 처리 로직을 구현할 수 있습니다.

그리고 클라이언트에서는 Apache Ignite의 클라이언트 모드를 사용하여, 클러스터 내의 Ignite 인스턴스에 연결할 수 있습니다. 클라이언트 모드는 Ignition.startClient() 메서드를 사용하여 시작할 수 있으며, 클러스터 내의 노드들의 데이터나 캐시 등에 접근할 수 있습니다.

따라서, 서버 역할을 하는 Apache Ignite를 사용하기 위해서는 클러스터를 먼저 구성해야 하고, 클러스터 내의 노드들이 데이터를 공유할 수 있도록 설정해야 합니다. 클라이언트는 이렇게 구성된 클러스터에 연결하여 데이터나 캐시 등에 접근할 수 있습니다.

## 사용자 입력:
> 클라이언트가 nodefactory를 호출하기 하는데, springboot 프로그램 만들어줘

## ChatGPT 응답:
> 클라이언트가 NodeFactory를 호출하는 Spring Boot 프로그램을 만드는 방법은 다음과 같습니다.

1. Spring Boot 프로젝트 생성

먼저, Spring Boot 프로젝트를 생성합니다. Spring Initializr를 사용하여 프로젝트를 생성할 수 있습니다. 이 때, 필요한 라이브러리로는 Apache Ignite와 Spring Web이 있습니다. 

2. NodeFactory 구현

NodeFactory를 구현합니다. 이 때, IgniteConfiguration과 CacheConfiguration을 설정하여 Ignite 클러스터를 시작하고, 클러스터 내의 캐시에 접근할 수 있도록 합니다. 

{% raw %}```
import org.apache.ignite.Ignite;
import org.apache.ignite.IgniteCache;
import org.apache.ignite.Ignition;
import org.apache.ignite.cache.CacheAtomicityMode;
import org.apache.ignite.cache.CacheMode;
import org.apache.ignite.configuration.CacheConfiguration;
import org.apache.ignite.configuration.IgniteConfiguration;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;

@Component
public class NodeFactory {

    private static final String CACHE_NAME = "node-cache";
    private IgniteCache<Node, Node> nodeCache;

    @PostConstruct
    public void init() {
        // Set IgniteConfiguration.
        IgniteConfiguration igniteCfg = new IgniteConfiguration();
        igniteCfg.setIgniteInstanceName("node-cache-cluster");
        // Set CacheConfiguration.
        CacheConfiguration<Node, Node> cacheCfg = new CacheConfiguration<>(CACHE_NAME);
        cacheCfg.setCacheMode(CacheMode.PARTITIONED);
        cacheCfg.setAtomicityMode(CacheAtomicityMode.ATOMIC);

        // Start Ignite and get cache.
        Ignite ignite = Ignition.start(igniteCfg);
        nodeCache = ignite.getOrCreateCache(cacheCfg);
    }

    public Node get(Node node) {
        Node cached = nodeCache.get(node);
        if (cached == null) {
            cached = node;
            nodeCache.put(cached, cached);
        }
        return cached;
    }

    public Node contains(Node node) {
        return nodeCache.get(node);
    }

    public void clear() {
        nodeCache.clear();
    }

    public int size() {
        return nodeCache.size();
    }
}
```{% endraw %}

3. NodeController 구현

NodeFactory를 사용하는 NodeController를 구현합니다. 클라이언트는 NodeController의 REST API를 호출하여 NodeFactory를 사용할 수 있습니다.

{% raw %}```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/node")
public class NodeController {

    private final NodeFactory nodeFactory;

    @Autowired
    public NodeController(NodeFactory nodeFactory) {
        this.nodeFactory = nodeFactory;
    }

    @GetMapping("/{id}")
    public Node getNode(@PathVariable int id) {
        return nodeFactory.get(new Node(id));
    }

    @GetMapping("/contains/{id}")
    public Node containsNode(@PathVariable int id) {
        return nodeFactory.contains(new Node(id));
    }

    @PostMapping("/")
    public Node addNode(@RequestBody Node node) {
        return nodeFactory.get(node);
    }

    @DeleteMapping("/")
    public void clearNodes() {
        nodeFactory.clear();
    }

    @GetMapping("/size")
    public int getSize() {
        return nodeFactory.size();
    }
}
```{% endraw %}

4. 애플리케이션 실행

Spring Boot 애플리케이션을 실행하고, 클라이

