---
layout: post
title: "Illegal Reflective Access Warning."
date: 2023-03-08
tags: [Warning, Illegal, Reflective, Access]
categories: blog
---

## 사용자 입력:
> Illegal reflective access by org.codehaus.groovy.reflection.CachedClass (file:/C:/Users/datastreams/.gradle/caches/modules-2/files-2.1/org.codehaus.groovy/groovy-all/2.4.13/7e410735a50e495323dd4855ddd13ea7dad23ac1/groovy-all-2.4.13.jar) to method java.lang.Object.finalize()

Please consider reporting this to the maintainers of org.codehaus.groovy.reflection.CachedClass

	at org.springframework.boot.devtools.restart.RestartApplicationListener.onApplicationEvent(RestartApplicationListener.java:45)
Use --illegal-access=warn to enable warnings of further illegal reflective access operations

All illegal access operations will be denied in a future release

## ChatGPT 응답:
> This is a warning message related to illegal reflective access in Java. It indicates that the code is trying to access a method, `java.lang.Object.finalize()`, via reflection in an illegal way. The message also suggests that the `org.codehaus.groovy.reflection.CachedClass` class is the source of the illegal access.

The warning message advises reporting the issue to the maintainers of `org.codehaus.groovy.reflection.CachedClass`. If you are not the developer of this code, you can forward this message to the appropriate maintainers.

The warning message also suggests using the `--illegal-access=warn` command-line option to enable warnings of further illegal reflective access operations. This option can help you identify and fix potential issues in your code before they become problematic.

Finally, the message warns that all illegal access operations will be denied in a future Java release. Therefore, it is essential to address this warning and fix any related issues in your code to ensure its continued compatibility and stability.

## 사용자 입력:
> 한글로 설명해줘

## ChatGPT 응답:
> 이것은 자바에서 일어나는 비합법적인 반사적 접근(reflection)과 관련된 경고 메시지입니다. 이 메시지는 코드가 reflection을 통해 `java.lang.Object.finalize()` 메소드에 비합법적으로 접근하려고 시도했다는 것을 나타냅니다. 또한 이 메시지는 `org.codehaus.groovy.reflection.CachedClass` 클래스가 비합법적인 접근의 원인이 되었다고 제안합니다.

이 경고 메시지는 `org.codehaus.groovy.reflection.CachedClass`의 유지보수자에게 이 문제를 보고하는 것을 권장합니다. 이 코드의 개발자가 아니라면, 이 메시지를 적절한 유지보수자에게 전달할 수 있습니다.

이 경고 메시지는 `--illegal-access=warn` 명령줄 옵션을 사용하여 추가적인 비합법적인 반사적 접근 작업에 대한 경고를 활성화할 것을 제안합니다. 이 옵션을 사용하면 코드에서 잠재적인 문제를 식별하고 수정할 수 있습니다.

마지막으로, 이 메시지는 모든 비합법적인 접근 작업이 앞으로의 자바 릴리스에서 거부될 것임을 경고합니다. 따라서 이 경고를 해결하고 코드에서 관련된 문제를 수정하여 계속된 호환성과 안정성을 보장하는 것이 중요합니다.

## 사용자 입력:
> 	at com.datastreams.qtrack.boot.QtrackApplication.main(QtrackApplication.groovy:40)
Illegal reflective access by org.codehaus.groovy.reflection.CachedClass (file:/C:/Users/datastreams/.gradle/caches/modules-2/files-2.1/org.codehaus.groovy/groovy-all/2.4.13/7e410735a50e495323dd4855ddd13ea7dad23ac1/groovy-all-2.4.13.jar) to method java.lang.Object.finalize()


## ChatGPT 응답:
> 이것은 QtrackApplication.groovy 파일의 40번째 줄에서 발생한 illegal reflective access와 관련된 경고 메시지입니다. 이 메시지는 코드가 reflection을 통해 `java.lang.Object.finalize()` 메소드에 비합법적으로 접근하려고 시도했으며, `org.codehaus.groovy.reflection.CachedClass` 클래스가 이 비합법적인 접근의 원인이라는 것을 나타냅니다. 

이 경우, QtrackApplication.groovy 파일의 40번째 줄에서 코드를 수정하거나 `org.codehaus.groovy.reflection.CachedClass` 클래스를 업데이트하는 것이 문제를 해결할 수 있습니다. 그러나 이것이 불가능한 경우, 이 메시지는 무시해도 되지만, 모든 비합법적인 접근 작업이 앞으로의 자바 릴리스에서 거부될 예정이므로, 가능하면 수정하는 것이 좋습니다.

## 사용자 입력:
> springboot상의 코드로 @ComponentScan(
	basePackages='com.datastreams.qtrack, com.datastreams.impactstream',
	excludeFilters =  @ComponentScan.Filter(type=FilterType.ASSIGNABLE_TYPE, value=MdosaApplication )
) 가 있는데, groovy로 되어 있다. 여기서 잘못된 점 있는가?

## ChatGPT 응답:
> 위의 코드에서 볼 수 있듯이 `@ComponentScan` 어노테이션에는 `basePackages`와 `excludeFilters` 속성이 있습니다. `basePackages` 속성은 Spring이 컴포넌트를 검색하는 데 사용할 패키지의 이름을 지정하는 데 사용되며, `excludeFilters` 속성은 컴포넌트 검색에서 제외해야 하는 구성 요소를 지정하는 데 사용됩니다.

그러나 이 코드가 Groovy로 작성되어 있다고 해서 문제가 될 것은 없습니다. Groovy는 Java와 호환되는 JVM 기반 언어로, Java에서 사용되는 대부분의 라이브러리와 프레임워크와 함께 사용될 수 있습니다. 따라서 Spring Boot 애플리케이션을 Groovy로 작성하는 것은 가능합니다.

즉, Groovy로 작성된 위 코드는 올바른 구문이며 Spring Boot 애플리케이션을 정상적으로 동작시킬 수 있습니다. 그러나 코드 자체에서 문제가 발생하는지 확인해야 하며, 이 코드를 포함한 전체 애플리케이션에서 잘못된 사용이나 오류가 발생하지 않도록 주의해야 합니다.

## 사용자 입력:
> > Task :qtrack-web:bootRun FAILED
[0.035s][info][gc] Using G1
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.codehaus.groovy.reflection.CachedClass (file:/C:/Users/datastreams/.gradle/caches/modules-2/files-2.1/org.codehaus.groovy/groovy-all/2.4.13/7e410735a50e495323dd4855ddd13ea7dad23ac1/groovy-all-2.4.13.jar) to method java.lang.Object.finalize()
WARNING: Please consider reporting this to the maintainers of org.codehaus.groovy.reflection.CachedClass
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
[0.850s][info][gc] GC(0) Pause Young (Normal) (G1 Evacuation Pause) 26M->12M(254M) 4.870ms
[1.086s][info][gc] GC(1) Pause Young (Normal) (G1 Evacuation Pause) 28M->14M(254M) 3.268ms
Exception in thread "main" java.lang.ClassCastException: class jdk.internal.loader.ClassLoaders$AppClassLoader cannot be cast to class java.net.URLClassLoader (jdk.internal.loader.ClassLoaders$AppClassLoader and java.net.URLClassLoader are in module java.base of loader 'bootstrap')
	at org.springframework.boot.devtools.restart.DefaultRestartInitializer.getUrls(DefaultRestartInitializer.java:93)
	at org.springframework.boot.devtools.restart.DefaultRestartInitializer.getInitialUrls(DefaultRestartInitializer.java:56)
	at org.springframework.boot.devtools.restart.Restarter.<init>(Restarter.java:137)
	at org.springframework.boot.devtools.restart.Restarter.initialize(Restarter.java:528)
	at org.springframework.boot.devtools.restart.RestartApplicationListener.onApplicationStartedEvent(RestartApplicationListener.java:68)
	at org.springframework.boot.devtools.restart.RestartApplicationListener.onApplicationEvent(RestartApplicationListener.java:45)
	at org.springframework.context.event.SimpleApplicationEventMulticaster.doInvokeListener(SimpleApplicationEventMulticaster.java:172)
	at org.springframework.context.event.SimpleApplicationEventMulticaster.invokeListener(SimpleApplicationEventMulticaster.java:165)
	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:139)
	at org.springframework.context.event.SimpleApplicationEventMulticaster.multicastEvent(SimpleApplicationEventMulticaster.java:122)
	at org.springframework.boot.context.event.EventPublishingRunListener.starting(EventPublishingRunListener.java:69)
	at org.springframework.boot.SpringApplicationRunListeners.starting(SpringApplicationRunListeners.java:47)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:292)
	at org.springframework.boot.builder.SpringApplicationBuilder.run(SpringApplicationBuilder.java:135)
	at org.springframework.boot.builder.SpringApplicationBuilder.configureAsChildIfNecessary(SpringApplicationBuilder.java:147)
An illegal reflective access operation has occurred

## ChatGPT 응답:
> 이 메시지는 Spring Boot 애플리케이션이 시작될 때 발생하는 예외와 함께 나타나는 경고 메시지입니다.

`WARNING: An illegal reflective access operation has occurred` 메시지는 비합법적인 반사적 접근(reflection) 작업이 발생했다는 것을 나타내며, 이는 자바 애플리케이션의 보안을 위협할 수 있는 문제입니다. 메시지는 `org.codehaus.groovy.reflection.CachedClass` 클래스가 비합법적인 접근을 수행했다고 보고하고 있습니다.

메시지는 `WARNING: Please consider reporting this to the maintainers of org.codehaus.groovy.reflection.CachedClass`와 같이 해당 문제를 `org.codehaus.groovy.reflection.CachedClass` 클래스의 유지보수자에게 보고하도록 권장합니다.

`WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations` 메시지는 `--illegal-access=warn` 명령줄 옵션을 사용하여 추가적인 비합법적인 반사적 접근 작업에 대한 경고를 활성화할 것을 제안합니다.

마지막으로, `Exception in thread "main" java.lang.ClassCastException: class jdk.internal.loader.ClassLoaders$AppClassLoader cannot be cast to class java.net.URLClassLoader (jdk.internal.loader.ClassLoaders$AppClassLoader and java.net.URLClassLoader are in module java.base of loader 'bootstrap')`와 같이 예외가 발생했다는 것을 나타내며, 이는 Spring Boot 애플리케이션의 클래스 로딩과 관련된 문제일 수 있습니다. 이 문제를 해결하려면 애플리케이션 클래스 로더를 올바르게 구성하거나 해당 클래스의 캐스팅을 수정해야 할 수 있습니다.

