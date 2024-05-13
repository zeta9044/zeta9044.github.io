---
layout: post
title: "Log4j2 XML Console 설정"
date: 2023-09-11
tags: [설정, XML, Console]
categories: blog
---

## 사용자 입력:
> log4j2 xml 기본설정. console만 나오게.

## ChatGPT 응답:
> log4j2를 사용하여 XML을 통해 콘솔(Console)에만 로그를 출력하려면 다음과 같이 간단한 설정을 할 수 있습니다. 먼저, log4j2.xml 파일을 생성하거나 기존 파일을 수정하세요.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="INFO">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>
```

이 XML 구성에서는 다음과 같은 요소들이 사용되었습니다:

1. `<Appenders>`: 로그 이벤트를 어디에 출력할지 정의합니다.
2. `<Console>`: 콘솔에 로그를 출력하는 Appender입니다.
   - `name` 속성: Appender의 이름을 정의합니다.
   - `target` 속성: 로그를 출력할 대상을 지정합니다. `SYSTEM_OUT`은 표준 출력(console)을 나타냅니다.
   - `<PatternLayout>`: 로그 메시지의 출력 형식을 정의합니다. 위의 예제에서는 시간, 스레드 이름, 로그 레벨, 로거 이름 및 메시지를 포함하는 형식을 사용합니다.

3. `<Loggers>`: 어떤 로거에서 로그 이벤트를 수집할지 정의합니다.
4. `<Root>`: 모든 로그 이벤트에 대한 최상위 로거 설정입니다.
   - `level` 속성: 최소 로그 레벨을 정의합니다. 이 예제에서는 INFO로 설정되어 있으므로 INFO 레벨 이상의 로그가 콘솔에 출력됩니다.
   - `<AppenderRef>`: 어떤 Appender를 사용할지 지정합니다. 이 경우에는 "Console" Appender를 사용합니다.

이렇게 설정하면 log4j2가 콘솔에만 로그를 출력하도록 구성됩니다. 필요한 경우 더 많은 Appender를 추가하거나 로그 레벨 및 출력 형식을 조정할 수 있습니다.

