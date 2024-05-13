---
layout: post
title: "제우스 WAS Spring Boot 배포"
tags: [Spring, 배포, 제우스, WAS, Boot]
---

## 사용자 입력:
> 한국의 제우스 WAS는 spring boot로 된 war를 deploy 못하나?

## ChatGPT 응답:
> 한국의 제우스 WAS는 Spring Boot로 된 WAR 파일을 배포할 수 있습니다. 

하지만 제우스 WAS의 경우에는 Spring Boot 어플리케이션을 배포할 때 몇 가지 주의할 점이 있습니다.

먼저, 제우스 WAS는 Servlet 3.0 스펙을 지원하므로 Spring Boot 어플리케이션의 web.xml 파일이 제거되어 있어야 합니다. 이를 위해서는 Spring Boot Maven 또는 Gradle 플러그인에서 "web.xml"을 제외시켜야 합니다.

또한, Spring Boot 어플리케이션을 제우스 WAS에 배포할 때는 embedded tomcat이 사용하는 포트와 충돌하지 않도록 설정해야 합니다.

마지막으로, Spring Boot 어플리케이션을 제우스 WAS에 배포하기 위해서는 제우스 WAS가 지원하는 Java 버전과 Spring Boot의 Java 버전이 일치해야 합니다.

이러한 주의사항을 지켜서 Spring Boot 어플리케이션을 제우스 WAS에 배포하면 정상적으로 동작할 수 있습니다.

## 사용자 입력:
> 한국의 제우스 WAS에서 war는 webmodule로 배포되나?

## ChatGPT 응답:
> 네, 한국의 제우스 WAS에서 WAR 파일은 web module로 배포됩니다. 

제우스 WAS는 Java Servlet과 JavaServer Pages (JSP)를 지원하는 웹 어플리케이션 서버이며, WAR 파일은 웹 어플리케이션을 패키징하는 표준 방법 중 하나입니다. 

WAR 파일은 웹 어플리케이션을 구성하는 여러 파일들과 디렉토리들을 하나의 파일로 묶어서 배포하는 방식입니다. 이때, WAR 파일 안에는 웹 어플리케이션의 설정 정보와 Java 클래스, JSP 파일, HTML 파일, JavaScript 파일 등의 리소스가 포함됩니다. 

제우스 WAS에서 WAR 파일을 배포하면, WAR 파일이 web module로 변환되어 웹 어플리케이션 컨테이너에 로드됩니다. 이후, 웹 어플리케이션 컨테이너는 WAR 파일 안에 포함된 리소스를 이용하여 웹 어플리케이션을 실행하게 됩니다.

## 사용자 입력:
> java.lang.ClassCastException: class org.apache.tomcat.websocket.server.WsServerContainer cannot be cast to class jeus.websocket.server.WebSocketServerContainer (org.apache.tomcat.websocket.server.WsServerContainer and jeus.websocket.server.WebSocketServerContainer are in unnamed module of loader jeus.server.classloader.RootClassLoader @2f2c9b19)

## ChatGPT 응답:
> 이러한 {% raw %}`java.lang.ClassCastException`{% endraw %} 에러는 제우스 WAS에서 WebSocket을 사용할 때 발생할 수 있는 문제 중 하나입니다.

이 에러는 {% raw %}`org.apache.tomcat.websocket.server.WsServerContainer`{% endraw %} 클래스와 {% raw %}`jeus.websocket.server.WebSocketServerContainer`{% endraw %} 클래스가 서로 다른 클래스로 인식되어 형변환 오류가 발생한다는 것을 나타냅니다. 

이 문제를 해결하려면, 제우스 WAS에서 제공하는 WebSocket API인 {% raw %}`jeus.websocket.server.WebSocketServerContainer`{% endraw %}을 사용해야 합니다. 따라서, 프로젝트의 라이브러리에 {% raw %}`jeus-websocket.jar`{% endraw %} 파일이 추가되어 있는지 확인해보세요. 

만약 라이브러리에 {% raw %}`jeus-websocket.jar`{% endraw %} 파일이 없다면, 해당 JAR 파일을 다운로드하여 프로젝트의 라이브러리에 추가하면 됩니다. 라이브러리에 JAR 파일을 추가하는 방법은 프로젝트의 빌드 도구에 따라 다를 수 있습니다.

또한, 이 에러가 발생하는 경우에는 제우스 WAS와 함께 제공되는 {% raw %}`tomcat-websocket.jar`{% endraw %} 파일이 충돌하는 경우가 많습니다. 이 경우, 해당 JAR 파일을 제거하거나, {% raw %}`jeus-websocket.jar`{% endraw %} 파일을 우선적으로 로딩하도록 설정하면 해결됩니다. 

이를 위해서는 {% raw %}`jeus-websocket.jar`{% endraw %} 파일을 우선적으로 로딩할 수 있는 설정 파일({% raw %}`jeus.properties`{% endraw %})을 만들어야 합니다. {% raw %}`jeus.properties`{% endraw %} 파일을 만들어서 해당 파일에 다음과 같이 설정을 추가하세요.

{% raw %}```
jeus.classpath.precedence=true
jeus.classpath.server.precedence=true
jeus.classpath.server.override=true
jeus.classpath.ext.precedence=false
jeus.classpath.lib.precedence=false
jeus.classpath.append=<jeus-websocket.jar 경로>
```{% endraw %}

위 설정을 추가하면 {% raw %}`jeus-websocket.jar`{% endraw %} 파일이 로딩될 때 다른 라이브러리들보다 먼저 로딩되어서 {% raw %}`java.lang.ClassCastException`{% endraw %} 에러가 발생하지 않게 됩니다.

## 사용자 입력:
> jeus-websocket.jar을 클래스로드에서 빼려면?

## ChatGPT 응답:
> {% raw %}`jeus-websocket.jar`{% endraw %}를 클래스로더에서 제외하려면 {% raw %}`jeus.properties`{% endraw %} 파일을 이용하여 제외할 수 있습니다. 

제우스 WAS는 클래스로더의 우선순위를 제어하는 설정 파일인 {% raw %}`jeus.properties`{% endraw %}를 제공합니다. {% raw %}`jeus.properties`{% endraw %} 파일을 사용하면 서버의 클래스로더에 로딩될 라이브러리의 우선순위를 조정할 수 있습니다. 이를 이용하여 {% raw %}`jeus-websocket.jar`{% endraw %}를 클래스로더에서 제외할 수 있습니다.

아래는 {% raw %}`jeus-websocket.jar`{% endraw %}를 클래스로더에서 제외하는 방법입니다.

1. {% raw %}`jeus.properties`{% endraw %} 파일을 엽니다.
2. 다음 설정을 추가합니다.

{% raw %}```
jeus.classpath.lib.precedence=false
jeus.classpath.append=<jeus-websocket.jar 경로>
```{% endraw %}

3. {% raw %}`jeus-websocket.jar`{% endraw %}를 클래스로더에서 제외할 모든 서버의 {% raw %}`jeus.properties`{% endraw %} 파일에 위 설정을 추가합니다.

위 설정을 추가하면, {% raw %}`jeus.classpath.lib.precedence=false`{% endraw %}는 제외할 라이브러리들을 지정할 수 있도록 설정하는 것이고, {% raw %}`jeus.classpath.append=<jeus-websocket.jar 경로>`{% endraw %}는 제외할 라이브러리를 지정하는 것입니다.

또는 다음과 같이 {% raw %}`jeus-websocket.jar`{% endraw %} 파일을 서버에서 삭제하여 클래스로더에서 제외할 수도 있습니다. 

1. {% raw %}`jeus-websocket.jar`{% endraw %} 파일이 위치한 경로로 이동합니다.
2. {% raw %}`jeus-websocket.jar`{% endraw %} 파일을 삭제합니다.
3. WAS를 재시작합니다.

하지만 이 방법은 {% raw %}`jeus-websocket.jar`{% endraw %} 파일을 완전히 삭제하기 때문에, 해당 라이브러리를 사용하는 웹 어플리케이션이 있는 경우 문제가 발생할 수 있습니다. 따라서 {% raw %}`jeus.properties`{% endraw %} 파일을 이용하여 클래스로더에서 제외하는 방법을 권장합니다.

## 사용자 입력:
> jeus.properties 위치는?

## ChatGPT 응답:
> 제우스 WAS의 {% raw %}`jeus.properties`{% endraw %} 파일은 일반적으로 {% raw %}`$JEUS_HOME/conf`{% endraw %} 디렉토리에 위치합니다. {% raw %}`$JEUS_HOME`{% endraw %}은 제우스 WAS가 설치된 디렉토리를 나타냅니다.

제우스 WAS의 설치 디렉토리는 보통 다음과 같은 경로에 있습니다.

- Windows: {% raw %}`C:\JEUS8`{% endraw %}
- Linux/Unix: {% raw %}`/usr/local/jeus8`{% endraw %}

{% raw %}`jeus.properties`{% endraw %} 파일은 제우스 WAS의 클래스로더에 로드되는 라이브러리의 우선순위를 제어하는 설정 파일입니다. 따라서, {% raw %}`jeus.properties`{% endraw %} 파일을 수정하여 서버에서 사용하는 라이브러리들의 우선순위를 조절할 수 있습니다.

만약 {% raw %}`$JEUS_HOME/conf`{% endraw %} 디렉토리에 {% raw %}`jeus.properties`{% endraw %} 파일이 없다면, 새로운 파일을 만들어야 합니다. 이때, 파일 이름은 반드시 {% raw %}`jeus.properties`{% endraw %}로 지정해야 합니다. 파일을 생성한 후, 원하는 설정을 추가하면 됩니다.

