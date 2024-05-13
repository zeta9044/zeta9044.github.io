---
layout: post
title: "Logback 로그 파일 설정"
date: 2023-11-24
tags: [Logback, 로그, 설정, 파일]
categories: blog
---

## 사용자 입력:
> logback 에서 .error 중  특정클래스의 함수별로 별도로 log파일을 만들 수 있나?

## ChatGPT 응답:
> 네, Logback을 사용하여 특정 클래스의 함수별로 별도의 로그 파일을 만들 수 있습니다. 이것을 수행하는 가장 일반적인 방법은 Logback의 Logger 이름을 활용하는 것입니다. Logger 이름은 Java 클래스의 패키지 및 클래스 이름과 관련이 있으므로 특정 클래스의 함수별로 별도의 로그 파일을 만들려면 해당 함수에 대한 Logger를 만들어야 합니다.

아래는 Logback에서 이를 수행하는 예제입니다.

```xml
<!-- logback.xml 파일 설정 -->
<configuration>

    <!-- 기본 로그 설정 -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>application.log</file>
        <encoder>
            <pattern>%date [%thread] %-5level %logger{35} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 특정 클래스의 함수별로 로그 설정 -->
    <logger name="com.example.MyClass" level="DEBUG">
        <appender-ref ref="MY_CLASS_FILE" />
    </logger>

    <!-- 위에서 정의한 appender와 매핑 -->
    <appender name="MY_CLASS_FILE" class="ch.qos.logback.core.FileAppender">
        <file>myclass.log</file>
        <encoder>
            <pattern>%date [%thread] %-5level %logger{35} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

위의 예제에서는 기본 로그 파일인 "application.log"와 특정 클래스 "com.example.MyClass"에 대한 로그 파일인 "myclass.log"을 구성했습니다. 이로써 "com.example.MyClass" 클래스의 함수별로 별도의 로그 파일을 생성할 수 있게 됩니다. 클래스 이름과 함수 이름을 사용하여 로그를 구분하고 필요한 레벨로 로깅할 수 있습니다.

## 사용자 입력:
> 자바로된 데몬프로세스를 /app/bin/javaD 를 실행시킬 건데, logback.xml의 위치는 어디에 놔야 하나?

## ChatGPT 응답:
> Logback 설정 파일인 `logback.xml`을 데몬 프로세스인 `/app/bin/javaD`를 실행할 때 어디에 놓아야 하는지에 대한 위치는 다음과 같이 여러 가지 방법으로 결정할 수 있습니다. 주로 사용하는 방법 중 일부를 아래에 설명하겠습니다.

1. **classpath 내**: `logback.xml` 파일을 데몬 프로세스의 클래스패스 내에 위치시키는 방법입니다. 이렇게 하면 데몬 프로세스를 실행할 때 Logback가 해당 파일을 찾을 수 있습니다. 보통 JAR 파일 내에 `logback.xml` 파일을 위치시키는 방법으로 사용됩니다.

2. **상대 경로**: `/app/bin/javaD` 실행 파일이 있는 디렉토리와 같은 디렉토리에 `logback.xml` 파일을 위치시키는 방법입니다. 이 경우, 상대 경로를 사용하여 Logback 설정을 로드할 수 있습니다. 예를 들어, `../config/logback.xml`과 같이 상대 경로를 지정할 수 있습니다.

3. **절대 경로**: `logback.xml` 파일을 시스템의 절대 경로로 위치시키는 방법입니다. 이 경우, `logback.configurationFile` 시스템 속성을 사용하여 `logback.xml` 파일의 절대 경로를 지정할 수 있습니다. 예를 들어, 다음과 같이 Java 명령줄에서 시스템 속성을 설정할 수 있습니다.

```shell
java -Dlogback.configurationFile=/path/to/logback.xml -cp /app/bin/javaD
```

4. **환경 변수 사용**: 데몬 프로세스를 시작하기 전에 `LOG_CONFIG`와 같은 환경 변수를 설정하여 `logback.xml` 파일의 경로를 지정하는 방법도 있습니다.

어떤 방법을 선택하느냐는 프로세스를 어떻게 시작하고 구성하는지에 따라 다를 수 있습니다. 보안 및 관리 목적에 따라 적절한 방법을 선택하고 관리하십시오.

