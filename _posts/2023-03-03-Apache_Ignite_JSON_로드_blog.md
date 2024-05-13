---
layout: post
title: "Apache Ignite JSON 로드"
date: 2023-03-03
tags: [로드, Apache, Ignite, JSON]
categories: blog
---

## 사용자 입력:
> 깊이가 3단계인 json array를 apache inginte 기동시에 로드하는 방법 알려주는데 클라이언트에서 호출하는 것은 제외야

## ChatGPT 응답:
> Apache Ignite는 분산 메모리 데이터 그리드 및 캐시 플랫폼으로, 대용량 데이터 처리를 위한 고성능 인 메모리 데이터 처리를 지원합니다. Apache Ignite를 사용하여 JSON 배열을 로드하려면 다음과 같은 단계를 수행할 수 있습니다.

1. JSON 배열을 Java 객체로 변환합니다.
JSON 배열을 Java 객체로 변환하는 방법은 다양합니다. 자바에서 제공하는 JSON 라이브러리 중에서는 Jackson, Gson, Json-simple 등이 있습니다. 이 중에서 자신이 사용하는 라이브러리를 선택하여 JSON 배열을 Java 객체로 변환합니다.

2. Java 객체를 Apache Ignite 캐시에 저장합니다.
Apache Ignite는 분산 메모리 데이터 그리드이므로, 데이터를 저장할 때 캐시를 사용합니다. JSON 배열을 Java 객체로 변환한 후에는 Apache Ignite 캐시에 저장합니다. 이를 위해서는 Apache Ignite 라이브러리를 사용하여 캐시를 생성하고 데이터를 저장합니다.

3. Apache Ignite에서 데이터를 처리합니다.
Apache Ignite는 분산 메모리 데이터 그리드 및 캐시 플랫폼으로, 대용량 데이터 처리를 지원합니다. 데이터를 Apache Ignite 캐시에 저장한 후에는 Apache Ignite에서 제공하는 기능을 사용하여 데이터를 처리할 수 있습니다. 이를 위해서는 Apache Ignite 라이브러리를 사용하여 데이터를 처리합니다.

위의 단계를 참고하여, JSON 배열을 Apache Ignite에 로드하는 방법은 다음과 같습니다.

1. JSON 배열을 Java 객체로 변환합니다.
자신이 사용하는 JSON 라이브러리를 선택하여 JSON 배열을 Java 객체로 변환합니다.

2. Apache Ignite 캐시를 생성합니다.
Apache Ignite 캐시를 생성하는 방법은 다음과 같습니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(true);

CacheConfiguration<Integer, YourJavaObject> cacheConfig = new CacheConfiguration<>();
cacheConfig.setName("yourCacheName");
cacheConfig.setIndexedTypes(Integer.class, YourJavaObject.class);

Ignite ignite = Ignition.start(igniteConfig);
IgniteCache<Integer, YourJavaObject> cache = ignite.getOrCreateCache(cacheConfig);
```

위의 코드에서 YourJavaObject는 JSON 배열을 변환한 Java 객체의 클래스입니다. 

3. Java 객체를 Apache Ignite 캐시에 저장합니다.
Java 객체를 Apache Ignite 캐시에 저장하는 방법은 다음과 같습니다.

```java
YourJavaObject[] jsonArray = ... // JSON 배열을 Java 객체로 변환한 배열

for (int i = 0; i < jsonArray.length; i++) {
    cache.put(i, jsonArray[i]);
}
```

위의 코드에서 i는 데이터의 키값을 의미합니다. 키값은 중복되지 않아야 하며, 데이터를 검색하거나 수정할 때 사용됩니다.

4. Apache Ignite에서 데이터를 처리합니다.
Apache Ignite에서 데이터를 처리하는 방법은 다양합니다. 예를

## 사용자 입력:
> 계속해

## ChatGPT 응답:
> 들어가, Apache Ignite에서 제공하는 SQL 쿼리, 인덱스, 분산 컴퓨팅 등을 사용하여 데이터를 처리할 수 있습니다.

```java
SqlFieldsQuery query = new SqlFieldsQuery("SELECT * FROM yourCacheName WHERE field = ?");
query.setArgs("value");

try (FieldsQueryCursor<List<?>> cursor = cache.query(query)) {
    for (List<?> row : cursor) {
        // 데이터 처리
    }
}
```

위의 코드에서는 SQL 쿼리를 사용하여 데이터를 검색하고, 검색된 데이터를 처리합니다. 

또한, 분산 컴퓨팅을 사용하여 병렬 처리를 수행할 수도 있습니다.

```java
IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    // 데이터 처리
});
```

위의 코드에서는 broadcast 메서드를 사용하여 모든 노드에서 데이터 처리를 수행합니다.

이처럼, Apache Ignite를 사용하여 JSON 배열을 로드하고 처리하는 방법은 간단합니다. 자신이 사용하는 JSON 라이브러리와 데이터 처리 방법에 따라 코드를 작성하면 됩니다.

## 사용자 입력:
> 위에 코드는 쿨라이언트 코드잖어

## ChatGPT 응답:
> 네, 죄송합니다. 제가 이전 답변에서 클라이언트 코드를 제외하도록 하라는 말씀을 드리셨는데도 클라이언트 코드를 포함한 예제를 드렸습니다. 다시 한번 제가 올바르게 답변드리지 못해 죄송합니다.

Apache Ignite를 기동시키는 방법은 다음과 같습니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);

Ignite ignite = Ignition.start(igniteConfig);
```

Apache Ignite를 클라이언트 모드로 기동시키는 경우에는 setClientMode 메서드의 파라미터를 true로 설정합니다.

그리고 JSON 배열을 Apache Ignite 캐시에 저장하는 코드는 다음과 같습니다.

```java
IgniteCache<Integer, YourJavaObject> cache = ignite.getOrCreateCache("yourCacheName");

YourJavaObject[] jsonArray = ... // JSON 배열을 Java 객체로 변환한 배열

for (int i = 0; i < jsonArray.length; i++) {
    cache.put(i, jsonArray[i]);
}
```

위의 코드에서 YourJavaObject는 JSON 배열을 변환한 Java 객체의 클래스입니다. i는 데이터의 키값을 의미합니다. 키값은 중복되지 않아야 하며, 데이터를 검색하거나 수정할 때 사용됩니다.

Apache Ignite에서 제공하는 SQL 쿼리, 인덱스, 분산 컴퓨팅 등을 사용하여 데이터를 처리하는 방법은 이전에 예제로 드린 것과 동일합니다. 다시한번 죄송합니다.

## 사용자 입력:
> 클라이언트 코딩법은 알고 있다고… 그러니 다른 걸 알려줘

## ChatGPT 응답:
> 네, 알겠습니다. Apache Ignite에서는 로컬 모드와 클러스터 모드로 실행할 수 있습니다. 로컬 모드에서는 Apache Ignite를 단일 머신에서 실행할 수 있으며, 클러스터 모드에서는 여러 대의 머신에서 Apache Ignite를 실행할 수 있습니다. 클러스터 모드에서는 머신 간 통신이 필요하므로 네트워크 구성과 노드 구성 등의 설정이 필요합니다.

먼저, 로컬 모드에서 Apache Ignite를 실행하는 방법은 다음과 같습니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");

Ignite ignite = Ignition.start(igniteConfig);
```

위의 코드에서는 IgniteConfiguration 객체를 생성하고, 클라이언트 모드를 false로 설정하여 로컬 모드로 실행합니다. setIgniteInstanceName 메서드를 사용하여 인스턴스 이름을 설정할 수 있습니다.

클러스터 모드에서는 먼저 네트워크 구성을 설정해야 합니다. 다음은 IP 노드 발견 방법 중 하나인 MulticastDiscovery 방법을 사용하는 예제입니다.

```java
TcpDiscoverySpi discoverySpi = new TcpDiscoverySpi();

TcpDiscoveryVmIpFinder ipFinder = new TcpDiscoveryMulticastIpFinder();
ipFinder.setMulticastGroup("228.10.10.157");
ipFinder.setMulticastPort(11222);

discoverySpi.setIpFinder(ipFinder);

IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");
igniteConfig.setDiscoverySpi(discoverySpi);

Ignite ignite = Ignition.start(igniteConfig);
```

위의 코드에서는 TcpDiscoverySpi 객체를 생성하고, TcpDiscoveryMulticastIpFinder 객체를 사용하여 IP 주소를 검색합니다. setMulticastGroup 메서드와 setMulticastPort 메서드를 사용하여 멀티캐스트 그룹과 포트를 설정합니다. setIpFinder 메서드를 사용하여 IP 노드 발견 방법을 설정합니다.

클러스터 모드에서 노드 구성은 다음과 같이 설정할 수 있습니다.

```java
TcpDiscoverySpi discoverySpi = new TcpDiscoverySpi();

TcpDiscoveryVmIpFinder ipFinder = new TcpDiscoveryMulticastIpFinder();
ipFinder.setMulticastGroup("228.10.10.157");
ipFinder.setMulticastPort(11222);

discoverySpi.setIpFinder(ipFinder);

IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");
igniteConfig.setDiscoverySpi(discoverySpi);
igniteConfig.setPeerClassLoadingEnabled(true);

CacheConfiguration<Integer, YourJavaObject> cacheConfig = new CacheConfiguration<>();
cacheConfig.setName("yourCacheName");
cacheConfig.setIndexedTypes(Integer.class, YourJavaObject.class);

Ignite ignite = Ignition.start(igniteConfig);
IgniteCache<Integer, YourJavaObject> cache = ignite.getOrCreateCache(cacheConfig);
```

위의 코드에서는 setPeerClassLoadingEnabled 메서드를 사용하여 노

## 사용자 입력:
> 내가 궁금한건 네가 만든 클래스를 ignite.sh헐 때 어떻게 실행키냐고 바보야

## ChatGPT 응답:
> 제가 이해를 잘못한 것 같습니다. 네가 만든 클래스를 Apache Ignite에서 실행하기 위해서는 먼저 클래스가 적절하게 컴파일되어야 합니다. 일반적으로 Java 프로그램을 컴파일하기 위해서는 JDK(Java Development Kit)가 설치되어 있어야 합니다.

Java 소스 코드를 작성한 후에는 다음과 같은 명령어를 사용하여 컴파일할 수 있습니다.

```
javac YourClass.java
```

위의 명령어에서 YourClass는 컴파일하려는 클래스의 이름입니다. 이 명령어를 실행하면 YourClass.class 파일이 생성됩니다. 이제 이 클래스를 Apache Ignite에서 실행할 수 있습니다.

Apache Ignite에서 클래스를 실행하기 위해서는 먼저 클래스를 JAR 파일로 묶어야 합니다. JAR 파일을 만들기 위해서는 다음과 같은 명령어를 사용합니다.

```
jar cvf YourJar.jar YourClass.class
```

위의 명령어에서 YourJar는 생성하려는 JAR 파일의 이름이며, YourClass.class는 JAR 파일에 포함시킬 클래스의 이름입니다. 이 명령어를 실행하면 YourJar.jar 파일이 생성됩니다.

이제 생성된 JAR 파일을 Apache Ignite에서 실행할 때 사용하는 클래스 경로에 추가하면 됩니다. 클래스 경로를 추가하는 방법은 다음과 같습니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");
igniteConfig.setClassLoader(Thread.currentThread().getContextClassLoader());

igniteConfig.setPeerClassLoadingEnabled(true);
igniteConfig.setUserAttributes(F.asMap("java.net.preferIPv4Stack", "true"));

igniteConfig.setCacheConfiguration(cacheConfig);

Ignition.start(igniteConfig);
```

위의 코드에서 setClassLoader 메서드를 사용하여 클래스 경로를 추가합니다. getContextClassLoader 메서드를 사용하여 현재 스레드의 클래스 로더를 가져옵니다. setUserAttributes 메서드를 사용하여 네트워크 설정을 지정할 수 있습니다.

클래스를 실행하려면 Apache Ignite에서 제공하는 compute() 메서드를 사용합니다. compute() 메서드는 클러스터에서 코드를 실행할 수 있는 환경을 제공합니다. 예를 들어 다음과 같은 코드를 작성하여 클래스를 실행할 수 있습니다.

```java
IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    YourClass.run();
});
```

위의 코드에서 YourClass는 실행할 클래스의 이름이며, run() 메서드는 실행할 메서드입니다. broadcast() 메서드를 사용하여 모든 노드에서 코드를 실행합니다.

이와 같이 Apache Ignite에서 클래스를 실행하기 위해서는 클래스를 JAR 파일로 묶고, 클래스 경로를 설정한 후에 compute() 메서드를 사용하여 코드를 실행하면 됩니다.

## 사용자 입력:
>  Apache Ignite에서 클래스를 실행하기 위해서는 클래스를 JAR 파일로 묶고, 클래스 경로를 설정한 후에 compute() 메서드를 사용하여 코드를 실행하면 됩니다. 이부분을 어떻게 하는 거냐고

## ChatGPT 응답:
> 클래스를 JAR 파일로 묶는 방법은 이전 답변에서 설명해드린 것과 같습니다. JAR 파일을 만들기 위해서는 다음과 같은 명령어를 사용합니다.

```
jar cvf YourJar.jar YourClass.class
```

위의 명령어에서 YourJar는 생성하려는 JAR 파일의 이름이며, YourClass.class는 JAR 파일에 포함시킬 클래스의 이름입니다. 이 명령어를 실행하면 YourJar.jar 파일이 생성됩니다.

클래스 경로를 설정하기 위해서는 IgniteConfiguration 객체의 setClassLoader() 메서드를 사용합니다. setClassLoader() 메서드는 현재 스레드의 클래스 로더를 가져와서 클래스 경로에 추가합니다. 다음은 클래스 경로를 설정하는 예제입니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");

URL[] urls = new URL[] { new URL("file:/path/to/YourJar.jar") };
ClassLoader classLoader = new URLClassLoader(urls, Thread.currentThread().getContextClassLoader());

igniteConfig.setClassLoader(classLoader);
igniteConfig.setPeerClassLoadingEnabled(true);
igniteConfig.setUserAttributes(F.asMap("java.net.preferIPv4Stack", "true"));

igniteConfig.setCacheConfiguration(cacheConfig);

Ignition.start(igniteConfig);
```

위의 코드에서는 setClassLoader() 메서드를 사용하여 클래스 경로를 설정합니다. URLClassLoader 클래스를 사용하여 JAR 파일의 경로를 추가합니다. Thread.currentThread().getContextClassLoader() 메서드를 사용하여 현재 스레드의 클래스 로더를 가져옵니다. setPeerClassLoadingEnabled() 메서드를 사용하여 피어 클래스 로딩을 활성화하고, setUserAttributes() 메서드를 사용하여 네트워크 설정을 지정합니다. setCacheConfiguration() 메서드를 사용하여 캐시 설정을 지정합니다.

마지막으로 compute() 메서드를 사용하여 코드를 실행합니다. compute() 메서드는 클러스터에서 코드를 실행할 수 있는 환경을 제공합니다. 다음은 compute() 메서드를 사용하여 클래스를 실행하는 예제입니다.

```java
IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    YourClass.run();
});
```

위의 코드에서 YourClass는 실행할 클래스의 이름이며, run() 메서드는 실행할 메서드입니다. broadcast() 메서드를 사용하여 모든 노드에서 코드를 실행합니다.

## ChatGPT 응답:
> 클래스를 JAR 파일로 묶고, 클래스 경로를 설정하는 방법은 다음과 같습니다.

1. Java 소스 코드를 작성합니다.

```java
public class MyJavaClass {
    public static void run() {
        // 실행할 코드
    }
}
```

2. Java 소스 코드를 컴파일합니다.

```
javac MyJavaClass.java
```

위의 명령어를 실행하면 MyJavaClass.class 파일이 생성됩니다.

3. MyJavaClass.class 파일을 JAR 파일로 묶습니다.

```
jar cvf myjar.jar MyJavaClass.class
```

위의 명령어를 실행하면 myjar.jar 파일이 생성됩니다.

4. Apache Ignite에서 클래스를 실행할 때 사용하는 클래스 경로에 JAR 파일을 추가합니다.

```java
IgniteConfiguration igniteConfig = new IgniteConfiguration();
igniteConfig.setClientMode(false);
igniteConfig.setIgniteInstanceName("myIgniteInstance");
igniteConfig.setClassLoader(new URLClassLoader(new URL[]{new File("myjar.jar").toURI().toURL()}));

Ignition.start(igniteConfig);
```

위의 코드에서 URLClassLoader 클래스를 사용하여 JAR 파일을 추가합니다. URLClassLoader 클래스의 생성자에 JAR 파일 경로를 지정합니다.

5. compute() 메서드를 사용하여 클래스를 실행합니다.

```java
IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    MyJavaClass.run();
});
```

위의 코드에서 MyJavaClass는 실행할 클래스의 이름이며, run() 메서드는 실행할 메서드입니다. broadcast() 메서드를 사용하여 모든 노드에서 코드를 실행합니다.

이와 같이 클래스를 JAR 파일로 묶고, 클래스 경로를 설정한 후에 compute() 메서드를 사용하여 코드를 실행할 수 있습니다.

## 사용자 입력:
> 글쌔 이 클래스를 어떻개 실행시키냐고 ignite.sh 기동헐땨 어떻게 실향하냐고 바보야

## ChatGPT 응답:
> 제가 이해를 잘못한 것 같습니다. Apache Ignite에서 클래스를 실행하기 위해서는 클래스를 JAR 파일로 묶은 후, JAR 파일을 클래스 경로에 추가해야 합니다. 

만약 ignite.sh 스크립트를 사용하여 Apache Ignite를 실행하고 있다면, 스크립트 파일과 같은 디렉토리에 lib 폴더를 생성하고, JAR 파일을 lib 폴더에 추가하면 됩니다. 

예를 들어, MyJavaClass.class 파일을 JAR 파일로 묶은 myjar.jar 파일을 사용한다면, 다음과 같은 디렉토리 구조를 갖게 됩니다.

```
ignite/
├── bin/
│   ├── ignite.sh
├── conf/
│   ├── ignite.xml
├── lib/
│   ├── myjar.jar
```

이제 ignite.sh 스크립트를 실행하면 JAR 파일이 클래스 경로에 추가되어 클래스를 실행할 수 있습니다. 클래스를 실행하려면 다음과 같은 코드를 사용합니다.

```java
Ignite ignite = Ignition.start("ignite.xml");

IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    MyJavaClass.run();
});
```

위의 코드에서 ignite.xml은 Apache Ignite의 구성 파일이며, MyJavaClass는 실행할 클래스의 이름이고, run() 메서드는 실행할 메서드입니다. broadcast() 메서드를 사용하여 모든 노드에서 코드를 실행합니다.

이와 같이 JAR 파일을 클래스 경로에 추가한 후, compute() 메서드를 사용하여 클래스를 실행할 수 있습니다.

## 사용자 입력:
> 저렇게 하면 기동할 때 딱 한 번 로드하는 건가?

## ChatGPT 응답:
> 네, 맞습니다. JAR 파일을 클래스 경로에 추가하면 Apache Ignite가 기동될 때 한 번 로드되어 클래스를 사용할 수 있습니다. JAR 파일이 클래스 경로에 추가되면, Ignite의 모든 노드에서 클래스를 사용할 수 있습니다. 따라서 compute() 메서드를 사용하여 클래스를 실행할 때 모든 노드에서 실행할 수 있습니다.

Apache Ignite에서는 Peer Class Loading(PCL) 기능을 제공합니다. PCL 기능을 사용하면 노드 간에 클래스를 동적으로 공유하여 사용할 수 있습니다. PCL 기능을 사용하면 모든 노드에서 클래스를 로드할 필요가 없으며, 필요할 때마다 동적으로 클래스를 로드하여 사용할 수 있습니다. PCL 기능은 클래스가 많은 경우에 유용하며, 네트워크 대역폭을 절약할 수 있습니다.

하지만 PCL 기능은 보안상의 이유로 기본적으로 비활성화되어 있습니다. PCL 기능을 사용하려면 IgniteConfiguration 객체에서 setPeerClassLoadingEnabled(true) 메서드를 호출하여 활성화해야 합니다. PCL 기능을 활성화하면 동적 클래스 로딩을 위한 보안 설정 등 추가 작업이 필요합니다. 따라서 PCL 기능을 사용하기 전에 충분한 검토와 테스트가 필요합니다.

## 사용자 입력:
> 한 번 로드된 후 언제든 로드를 시키려면 어떻게 해야 할까?

## ChatGPT 응답:
> Apache Ignite에서는 Peer Class Loading(PCL) 기능을 사용하여 노드 간에 클래스를 동적으로 공유할 수 있습니다. PCL 기능을 사용하면 클래스가 변경되거나 추가될 때마다 클래스를 다시 로드하여 사용할 수 있습니다.

PCL 기능을 사용하려면 IgniteConfiguration 객체에서 setPeerClassLoadingEnabled(true) 메서드를 호출하여 활성화해야 합니다. PCL 기능을 활성화하면 클래스가 변경되거나 추가될 때마다 다른 노드에서 동적으로 클래스를 로드하여 사용할 수 있습니다.

클래스가 변경되었거나 추가되었을 때, Ignite에서는 클래스를 다시 로드하기 위해 다음과 같은 메서드를 제공합니다.

1. Ignite#resetLostPartitions() 메서드

resetLostPartitions() 메서드는 노드가 중단되었을 때, 노드의 데이터를 다른 노드에 복제할 수 있는 메서드입니다. 이 메서드를 호출하면 데이터가 복제되면서 변경된 클래스가 다시 로드됩니다.

```java
Ignite ignite = Ignition.start("ignite.xml");

ignite.resetLostPartitions();
```

2. Ignite#compute().broadcast() 메서드

compute().broadcast() 메서드를 사용하여 모든 노드에서 클래스를 실행할 때, 클래스가 변경되었거나 추가되었을 때, 변경된 클래스를 다시 로드할 수 있습니다.

```java
Ignite ignite = Ignition.start("ignite.xml");

IgniteCompute compute = ignite.compute();

compute.broadcast(() -> {
    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
    Class<?> clazz = classLoader.loadClass("MyClass");
    // 클래스 실행
});
```

위의 코드에서 Thread.currentThread().getContextClassLoader() 메서드를 사용하여 클래스 로더를 가져오고, loadClass() 메서드를 사용하여 변경된 클래스를 로드합니다. broadcast() 메서드를 사용하여 모든 노드에서 코드를 실행합니다.

이와 같이 PCL 기능을 사용하여 클래스를 동적으로 로드할 수 있으며, resetLostPartitions() 메서드와 compute().broadcast() 메서드를 사용하여 변경된 클래스를 다시 로드할 수 있습니다.

## 사용자 입력:
> 클래스 변경 추가는 아니야 rest로 명령하눈거야

## ChatGPT 응답:
> 클래스가 변경되거나 추가될 때, REST API를 사용하여 변경된 클래스를 다시 로드할 수 있습니다.

Apache Ignite에서는 REST API를 사용하여 Ignite의 기능을 원격으로 사용할 수 있습니다. REST API를 사용하면 Java 언어 이외의 다른 언어로 Ignite를 사용할 수 있으며, 원격으로 Ignite의 기능을 사용할 수 있습니다.

클래스를 변경하거나 추가할 때, 다음과 같은 방법으로 REST API를 사용할 수 있습니다.

1. JAR 파일을 업로드합니다.

```bash
curl -X POST -H "Content-Type: multipart/form-data" -F "file=@myjar.jar" http://localhost:8080/ignite?cmd=put-class
```

위의 명령어에서 myjar.jar는 업로드할 JAR 파일의 이름입니다. curl 명령어를 사용하여 REST API를 호출합니다. put-class 명령어를 사용하여 JAR 파일을 업로드합니다.

2. 클래스를 실행합니다.

```bash
curl -X POST http://localhost:8080/ignite?cmd=run-class&name=MyJavaClass&method=run
```

위의 명령어에서 MyJavaClass는 실행할 클래스의 이름이며, run은 실행할 메서드입니다. run-class 명령어를 사용하여 클래스를 실행합니다.

이와 같이 REST API를 사용하여 JAR 파일을 업로드하고, 클래스를 실행할 수 있습니다. REST API를 사용하면 원격으로 Ignite의 기능을 사용할 수 있으며, 변경된 클래스를 즉시 로드할 수 있습니다.

