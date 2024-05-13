---
layout: post
title: "Tomcat 디렉토리 이동."
tags: [이동, Tomcat, 디렉토리]
---

## 사용자 입력:
> tomcat의 webapp 디렉토리를 특정 디렉토리로 바꾸면 jndi설정에 문제가 있나?

## ChatGPT 응답:
> 일반적으로 Tomcat의 webapp 디렉토리를 다른 디렉토리로 이동하는 것은 JNDI 설정에 영향을 미치지 않습니다. 그러나 이동한 디렉토리가 Tomcat의 기본 디렉토리 구조에서 벗어나는 경우에는 JNDI 경로를 수정해야 할 수 있습니다.

만약 Tomcat에서 JNDI 데이터 소스를 사용하고 있다면, JNDI 컨텍스트를 설정하는 XML 파일의 경로를 수정해야 할 수도 있습니다. 이 경우에는 변경된 경로를 XML 파일에 지정해주어야 합니다.

그러나 JNDI 설정이 변경되지 않았다면, 웹 어플리케이션은 변경된 디렉토리에서도 여전히 동작할 것입니다. 다만, Tomcat의 기본 디렉토리 구조를 변경하면 다른 문제가 발생할 가능성이 있으므로, 변경 전에 충분한 테스트를 수행하는 것이 좋습니다.

## 사용자 입력:
> "만약 Tomcat에서 JNDI 데이터 소스를 사용하고 있다면, JNDI 컨텍스트를 설정하는 XML 파일의 경로를 수정해야 할 수도 있습니다. 이 경우에는 변경된 경로를 XML 파일에 지정해주어야 합니다."에 대해서 자세히...예제까지 보태서 설명해봐.

## ChatGPT 응답:
> Tomcat에서 JNDI 데이터 소스를 사용하는 경우, context.xml 파일을 사용하여 JNDI 컨텍스트를 설정합니다. context.xml 파일은 $CATALINA_HOME/conf 디렉토리에 있습니다.

JNDI 데이터 소스를 설정하려면, context.xml 파일에 다음과 같은 구문을 추가해야 합니다.

{% raw %}```
<Resource name="jdbc/mydb" auth="Container" type="javax.sql.DataSource"
           maxTotal="100" maxIdle="30" maxWaitMillis="10000"
           username="dbuser" password="dbpass" driverClassName="com.mysql.jdbc.Driver"
           url="jdbc:mysql://localhost/mydb"/>
```{% endraw %}

위 예제에서는 "jdbc/mydb"라는 이름의 JNDI 데이터 소스를 설정하고 있습니다. 이 설정을 사용하는 웹 어플리케이션에서는 "java:comp/env/jdbc/mydb" JNDI 이름으로 데이터 소스를 참조할 수 있습니다.

만약 Tomcat의 webapp 디렉토리를 다른 디렉토리로 이동하게 된다면, JNDI 설정에서는 다음과 같이 변경해야 합니다.

1. 변경된 webapp 디렉토리 경로를 참조하는 Context 엘리먼트를 context.xml 파일에 추가합니다.

{% raw %}```
<Context docBase="/path/to/new/webapp/dir">
...
</Context>
```{% endraw %}

2. JNDI 데이터 소스를 설정하는 Resource 엘리먼트에서 url 속성 값을 변경합니다.

{% raw %}```
<Resource name="jdbc/mydb" auth="Container" type="javax.sql.DataSource"
           maxTotal="100" maxIdle="30" maxWaitMillis="10000"
           username="dbuser" password="dbpass" driverClassName="com.mysql.jdbc.Driver"
           url="jdbc:mysql://localhost/new_webapp_dir/mydb"/>
```{% endraw %}

위 예제에서는 url 속성 값에 "/new_webapp_dir"을 추가하여 데이터베이스 연결 URL을 수정하였습니다. 이제 JNDI 이름 "java:comp/env/jdbc/mydb"를 사용하여 데이터 소스에 접근할 수 있습니다.

하지만 이렇게 변경하는 것은 JNDI 경로가 다시 변경될 때마다 매번 수정해줘야 하므로, 일반적으로는 context.xml 파일에서 상대 경로를 사용하거나, JNDI 데이터 소스를 설정하는 Resource 엘리먼트에서 docBase 속성을 사용하여 웹 어플리케이션 경로를 참조하는 방법을 사용합니다. 이렇게 하면 웹 어플리케이션의 경로가 변경되더라도 JNDI 설정을 변경할 필요가 없어집니다.

## 사용자 입력:
> 네가 말한 docBAse가 Dcatalia.base 발하는 건가?

## ChatGPT 응답:
> {% raw %}`docBase`{% endraw %}와 {% raw %}`catalina.base`{% endraw %}는 다른 것입니다. 

{% raw %}`docBase`{% endraw %}는 Tomcat에서 사용되는 웹 어플리케이션의 디렉토리 경로를 지정하는 속성입니다. {% raw %}`docBase`{% endraw %}는 {% raw %}`Context`{% endraw %} 요소에 정의되며, 웹 어플리케이션의 WAR 파일이나 디렉토리 경로를 가리킵니다. Tomcat이 웹 어플리케이션을 로드할 때, {% raw %}`docBase`{% endraw %} 경로에 있는 파일들을 읽어들여 웹 어플리케이션을 로드합니다.

{% raw %}`catalina.base`{% endraw %}는 Tomcat의 인스턴스 디렉토리 경로를 지정하는 환경 변수입니다. {% raw %}`catalina.base`{% endraw %}는 Tomcat의 기본 디렉토리 구조에서 {% raw %}`$CATALINA_HOME`{% endraw %} 디렉토리를 가리킵니다. {% raw %}`$CATALINA_HOME`{% endraw %}은 Tomcat 바이너리 파일들이 위치한 디렉토리입니다.

따라서, {% raw %}`docBase`{% endraw %}와 {% raw %}`catalina.base`{% endraw %}는 서로 다른 개념이며, {% raw %}`docBase`{% endraw %}는 웹 어플리케이션의 디렉토리 경로를 지정하고, {% raw %}`catalina.base`{% endraw %}는 Tomcat 인스턴스 디렉토리 경로를 지정하는 환경 변수입니다.

## 사용자 입력:
> 그러면... 이전에 tomcat/webapp/qtrack , qtrack이 웹어플리케이션이야..이것이  /app/qtack으로 바뀌었어... 이러면 dockbase는 어떻게 써야해?

## ChatGPT 응답:
> 만약 {% raw %}`qtrack`{% endraw %} 웹 어플리케이션 디렉토리가 {% raw %}`/app/qtrack`{% endraw %}으로 이동하였다면, {% raw %}`docBase`{% endraw %} 속성은 다음과 같이 설정할 수 있습니다.

{% raw %}```xml
<Context docBase="/app/qtrack">
  ...
</Context>
```{% endraw %}

위와 같이 {% raw %}`Context`{% endraw %} 요소에서 {% raw %}`docBase`{% endraw %} 속성에 변경된 디렉토리 경로를 설정해주면, Tomcat은 해당 디렉토리에서 웹 어플리케이션을 로드합니다. 이제 {% raw %}`catalina.base`{% endraw %} 디렉토리 내의 {% raw %}`conf/server.xml`{% endraw %} 파일에 위와 같은 {% raw %}`Context`{% endraw %} 요소를 추가하면 됩니다.

{% raw %}```xml
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="true">
  <Context docBase="/app/qtrack">
    ...
  </Context>
  ...
</Host>
```{% endraw %}

위와 같이 {% raw %}`Context`{% endraw %} 요소를 추가한 뒤, Tomcat을 다시 시작하면 변경된 디렉토리에서 {% raw %}`qtrack`{% endraw %} 웹 어플리케이션이 로드됩니다.

