---
layout: post
title: "UTF-8 XML Error"
tags: [UTF, Error, XML]
---

## 사용자 입력:
> spring framework에서 jndi로 DB에 접속해서 테이블안에 들어있는 항목으로 xml을 만드는 작업을 하다가, org.xml.sax.SAXParseException  Invalid byte 2 of 3-byte UTF-8 sequence 오류가 발생했다. 원인이 무엇일까?

## ChatGPT 응답:
> 

이 오류는 UTF-8 인코딩 방식으로 인코딩 된 XML 문서에서 잘못된 바이트 시퀀스가 발견되었을 때 발생합니다.

가능한 원인 중 하나는 JNDI 데이터 소스에서 반환 된 데이터가 UTF-8로 인코딩되지 않았을 수 있습니다. 이 경우 데이터 소스에서 데이터를 가져올 때 인코딩을 UTF-8로 설정하거나, 가져온 데이터를 UTF-8로 변환하는 작업이 필요합니다.

또 다른 가능한 원인은 XML 문서 자체가 잘못되었을 수 있습니다. 이 경우 문서를 열어 수동으로 검사하거나 XML 검사기를 사용하여 유효성을 검사할 수 있습니다.

또한, 문제가 발생한 바이트 시퀀스와 관련된 정보를 찾아 볼 수 있습니다. 이 정보를 사용하여 문제를 해결하는 것이 유용할 수 있습니다.

위의 가능한 원인 중 하나가 해당 문제를 해결하는 데 도움이 될 수 있으며, 문제를 더 자세히 파악하기 위해 문제가 발생한 코드 부분을 확인하는 것이 좋습니다.

## 사용자 입력:
> JNDI 데이터 소스에서 반환 된 데이터가 UTF-8로 인코딩되지 않은 것으로 보는데,  이 경우 데이터 소스에서 데이터를 가져올 때 인코딩을 UTF-8로 설정하는 방법은?

## ChatGPT 응답:
> JNDI 데이터 소스에서 데이터를 가져올 때 인코딩을 UTF-8로 설정하려면, 다음과 같이 데이터베이스 연결 URL에 "useUnicode=true&characterEncoding=UTF-8"을 추가하여 UTF-8 인코딩을 사용하도록 설정해야 합니다.

예를 들어, MySQL 데이터베이스의 경우, 다음과 같이 연결 URL을 설정할 수 있습니다.

{% raw %}```
jdbc:mysql://localhost:3306/mydatabase?useUnicode=true&characterEncoding=UTF-8
```{% endraw %}

Oracle 데이터베이스의 경우, 다음과 같이 연결 URL을 설정할 수 있습니다.

{% raw %}```
jdbc:oracle:thin:@localhost:1521:ORCL;charset=UTF-8
```{% endraw %}

위의 예제에서는 "characterEncoding" 매개 변수를 UTF-8로 설정하여 인코딩 방식을 지정하고 있습니다. 이렇게 설정하면 JNDI 데이터 소스에서 가져오는 데이터의 인코딩 방식이 UTF-8로 지정되어 문제를 해결할 수 있습니다.

## 사용자 입력:
> oracle 19c일때는 어떻게 해야해?

## ChatGPT 응답:
> Oracle 19c에서 JDBC 드라이버를 사용하여 연결할 때도, JDBC 연결 URL에 "useUnicode=true&characterEncoding=UTF-8" 매개 변수를 추가하여 UTF-8 인코딩을 사용하도록 설정할 수 있습니다.

예를 들어, 다음과 같이 연결 URL을 설정할 수 있습니다.

{% raw %}```
jdbc:oracle:thin:@localhost:1521/ORCLPDB1?useUnicode=true&characterEncoding=UTF-8
```{% endraw %}

위 예제에서는 "characterEncoding" 매개 변수를 UTF-8로 설정하여 인코딩 방식을 지정하고 있습니다. 이렇게 설정하면 Oracle 19c에서 JNDI 데이터 소스에서 가져오는 데이터의 인코딩 방식이 UTF-8로 지정되어 문제를 해결할 수 있습니다.

또는 다음과 같이 JNDI 데이터 소스 구성 파일에서 "connectionProperties" 속성을 추가하여 UTF-8 인코딩을 사용하도록 설정할 수도 있습니다.

{% raw %}```
<bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean">
    <property name="jndiName" value="java:comp/env/jdbc/myDataSource"/>
    <property name="connectionProperties">
        <props>
            <prop key="useUnicode">true</prop>
            <prop key="characterEncoding">UTF-8</prop>
        </props>
    </property>
</bean>
```{% endraw %}

위의 예제에서는 "connectionProperties" 속성을 사용하여 "useUnicode"와 "characterEncoding" 매개 변수를 설정하고 있습니다. 이렇게 설정하면 Oracle 19c에서 JNDI 데이터 소스에서 가져오는 데이터의 인코딩 방식이 UTF-8로 지정되어 문제를 해결할 수 있습니다.

## 사용자 입력:
> lineNumber: 69; columnNumber: 103; "characterEncoding" 엔티티에 대한 참조는 ';' 구분자로 끝나야 합니다.


## ChatGPT 응답:
> 이 오류는 XML 문서에서 "characterEncoding" 엔티티에 대한 참조가 잘못되었을 때 발생합니다. 일반적으로 "characterEncoding" 엔티티에 대한 참조는 XML 선언에서 사용됩니다.

XML 선언은 다음과 같은 형식으로 구성됩니다.

{% raw %}```
<?xml version="1.0" encoding="UTF-8"?>
```{% endraw %}

여기서 "encoding" 속성은 문서의 문자 인코딩 방식을 지정합니다. 이 속성에 대한 값은 ';' 문자로 끝나야 합니다.

즉, "encoding" 

## 사용자 입력:
> jdbc:oracle:thin:@localhost:1521/ORCLPDB1?useUnicode=true&characterEncoding=UTF-8
이게 잘못됐데. jdbc:oracle:thin:@localhost:1521/ORCLPDB1?useUnicode=true&characterEncoding=UTF-8


## ChatGPT 응답:
> 죄송합니다, 제가 이전에 작성한 답변에서 실수를 저질렀습니다. 정정된 내용을 다시 안내드리겠습니다.

정확한 JDBC 연결 URL은 다음과 같습니다.

{% raw %}```
jdbc:oracle:thin:@localhost:1521/ORCLPDB1?useUnicode=true&encoding=UTF-8
```{% endraw %}

위의 예제에서는 "encoding" 매개 변수를 UTF-8로 설정하여 인코딩 방식을 지정하고 있습니다. 이렇게 설정하면 Oracle 19c에서 JNDI 데이터 소스에서 가져오는 데이터의 인코딩 방식이 UTF-8로 지정되어 문제를 해결할 수 있습니다.

또한, Spring Framework에서 JNDI 데이터 소스 구성 파일에서 "connectionProperties" 속성을 추가하여 UTF-8 인코딩을 사용하도록 설정할 수도 있습니다.

{% raw %}```
<bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean">
    <property name="jndiName" value="java:comp/env/jdbc/myDataSource"/>
    <property name="connectionProperties">
        <props>
            <prop key="useUnicode">true</prop>
            <prop key="encoding">UTF-8</prop>
        </props>
    </property>
</bean>
```{% endraw %}

위의 예제에서는 "connectionProperties" 속성을 사용하여 "useUnicode"와 "encoding" 매개 변수를 설정하고 있습니다. 이렇게 설정하면 Oracle 19c에서 JNDI 데이터 소스에서 가져오는 데이터의 인코딩 방식이 UTF-8로 지정되어 문제를 해결할 수 있습니다.

## 사용자 입력:
> Oracle 19c 에서 JNDI 데이터 소스에서 반환 된 데이터가 UTF-8로 인코딩되지 않은 것으로 보는데,  이 경우 데이터 소스에서 데이터를 가져올 때 인코딩을 UTF-8로 설정하는 방법은?

## ChatGPT 응답:
> Oracle 19c에서 JNDI 데이터 소스에서 반환 된 데이터가 UTF-8로 인코딩되지 않은 경우, 다음과 같은 방법으로 데이터를 가져올 때 인코딩을 UTF-8로 설정할 수 있습니다.

1. JDBC 드라이버에 설정하기

JDBC 드라이버에 인코딩을 UTF-8로 설정하는 방법은 JDBC 드라이버의 URL에 "useUnicode=true&characterEncoding=UTF-8" 매개 변수를 추가하는 것입니다. 예를 들어, 다음과 같은 URL을 사용할 수 있습니다.

{% raw %}```
jdbc:oracle:thin:@localhost:1521:ORCL?useUnicode=true&characterEncoding=UTF-8
```{% endraw %}

2. Oracle 데이터베이스에 설정하기

Oracle 데이터베이스의 NLS_LANG 매개 변수를 UTF-8로 설정하는 방법도 있습니다. 이렇게하면 모든 클라이언트 연결에 대해 UTF-8 인코딩이 자동으로 적용됩니다.

NLS_LANG 매개 변수를 설정하려면 다음과 같은 명령을 사용합니다.

{% raw %}```
export NLS_LANG=.AL32UTF8
```{% endraw %}

3. 애플리케이션에서 설정하기

애플리케이션에서 데이터를 가져올 때 UTF-8 인코딩을 사용하도록 설정하는 방법도 있습니다. 이 방법은 JNDI 데이터 소스를 가져올 때 "encoding" 속성을 UTF-8로 설정하는 것입니다.

{% raw %}```
<Context>
  <Resource name="jdbc/myDataSource" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="oracle.jdbc.OracleDriver"
            url="jdbc:oracle:thin:@localhost:1521:ORCL"
            username="scott"
            password="tiger"
            maxActive="20"
            encoding="UTF-8" />
</Context>
```{% endraw %}

위의 방법 중 하나를 선택하여 데이터 소스에서 데이터를 UTF-8로 가져와 문제를 해결할 수 있습니다.

