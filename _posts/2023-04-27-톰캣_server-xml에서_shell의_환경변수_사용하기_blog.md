---
layout: post
title: "톰캣 server.xml에서 shell의 환경변수 사용하기"
date: 2023-04-27
tags: [톰캣, server, 의, 사용, 환경변수]
categories: blog
---

## 사용자 입력:
> 톰캣 server.xml에서 shell의 환경변수를 쓰는 방법 알려줘

## ChatGPT 응답:
> 톰캣의 `server.xml` 파일에서 shell 환경변수를 사용하려면 다음과 같이 할 수 있습니다:

1. `server.xml` 파일에서 설정하려는 속성의 값을 `${}`로 감싸 주세요. 예를 들어, 다음과 같은 속성을 가진 Connector가 있다고 가정해 봅시다.

```xml
<Connector port="${HTTP_PORT}" protocol="HTTP/1.1" />
```

2. 해당 환경변수를 shell에서 설정해 주세요. 예를 들어, HTTP_PORT 환경변수를 설정하려면 다음과 같이 하세요.

```bash
export HTTP_PORT=8080
```

3. 톰캣을 시작하면 `${HTTP_PORT}`가 설정된 값으로 대체됩니다. 위의 예에서는 8080으로 대체됩니다.

참고로, `server.xml` 파일에서 shell 환경변수를 사용하는 것은 보안 문제를 일으킬 수 있으므로 주의해야 합니다. 환경변수 값을 악용할 수 있는 사용자가 해당 서버에 액세스할 수 있는 경우, 해당 사용자가 환경변수 값을 변경하여 악의적인 목적으로 사용할 수 있습니다. 따라서, 가능한 경우에는 `server.xml` 파일에서 하드 코딩된 값을 사용하는 것이 좋습니다.

## 사용자 입력:
> connectionInitSql="SET search_path TO ${liaDB}" 이렇게 쓰면, Caused by: com.zaxxer.hikari.pool.HikariPool$PoolInitializationException: Failed to initialize pool: ERROR: syntax error at or near "$" 이게 나오네

## ChatGPT 응답:
> 에러 메시지를 보면 PostgreSQL에서 `SET search_path` 쿼리에 `${liaDB}`가 포함되어 있어서 문법 오류가 발생한 것으로 보입니다. `${liaDB}`는 쉘 환경변수를 나타내는 것으로 보이므로, 이 값을 PostgreSQL 쿼리 내에서 사용하려면 먼저 해당 쉘 환경변수의 값을 가져와야 합니다.

PostgreSQL에서는 `psql` 쉘에서 사용하는 방법과 같이 `$$`를 사용하여 쿼리 내에서 변수를 참조할 수 있습니다. 따라서 `SET search_path` 쿼리에서 `${liaDB}`를 대체하는 방법은 다음과 같습니다.

```xml
connectionInitSql="SET search_path TO $$${liaDB}$$"
```

`${liaDB}` 대신 `$$${liaDB}$$`를 사용하여 PostgreSQL에서 변수를 참조하도록 지정합니다. `$` 문자는 이스케이프 처리되어 `$$`로 변경됩니다.

위의 설정으로 시도해 보시고 여전히 문제가 발생하는 경우에는 PostgreSQL 쿼리 자체에서 오류가 있는지 확인하시기 바랍니다.

## 사용자 입력:
> 위 내용에서 오라클은 어떻게 connectionInitSql 써야 해

## ChatGPT 응답:
> 오라클에서는 `connectionInitSql` 속성을 사용하여 `SET` 명령문을 실행할 수 있습니다. 그러나 오라클에서는 `SET` 명령문을 사용하여 스키마를 변경하는 것이 아니라 `ALTER SESSION` 명령문을 사용해야 합니다.

따라서, `connectionInitSql` 속성에서는 다음과 같이 `ALTER SESSION` 명령문을 사용하여 스키마를 변경할 수 있습니다.

```xml
connectionInitSql="ALTER SESSION SET CURRENT_SCHEMA = ${liaDB}"
```

위의 설정은 `${liaDB}` 값을 현재 세션의 스키마로 설정합니다. `${liaDB}`는 쉘 환경변수를 나타내는 것으로, 해당 값을 실제 값으로 대체해야 합니다.

오라클에서도 PostgreSQL과 마찬가지로 보안 상의 이유로 `connectionInitSql` 속성에서 하드 코딩된 값을 사용하는 것이 좋습니다.

## 사용자 입력:
>  connectionInitSql="SET search_path TO "LIAUSR"" 이거 맞나?

## ChatGPT 응답:
> PostgreSQL에서 `SET search_path` 명령문을 사용하여 스키마를 변경할 때는 스키마 이름을 따옴표로 묶어 주어야 합니다. 

따라서, `connectionInitSql` 속성에서 스키마 이름을 따옴표로 묶어 주어야 합니다. 하지만 현재 설정된 `connectionInitSql` 속성에서는 따옴표가 잘못 사용되고 있습니다.

올바른 구문은 다음과 같습니다.

```xml
connectionInitSql="SET search_path TO 'LIAUSR'"
```

위의 설정은 `search_path`를 `LIAUSR` 스키마로 설정합니다. 스키마 이름은 따옴표로 묶어 주어야 하며, 이스케이프 문자를 사용하지 않아도 됩니다.

반면, `${liaDB}`와 같이 쉘 환경변수를 참조하는 경우에는 PostgreSQL에서는 `$$${liaDB}$$`와 같이 `$` 문자를 이스케이프 처리해야 하므로, 다음과 같은 구문을 사용해야 합니다.

```xml
connectionInitSql="SET search_path TO $$${liaDB}$$"
```

위의 설정은 `search_path`를 `${liaDB}` 값으로 설정합니다. 환경변수 값을 참조하기 위해 `$` 문자를 이스케이프 처리하고, PostgreSQL에서는 `$` 대신 `$$`를 사용하여 이스케이프 처리합니다.

## 사용자 입력:
> 톰캣 9.x 서버에서 server.xml 에서 db 연결 리소스를 정의하는데 user,password를 암호화해서 처리하고 싶어.

## ChatGPT 응답:
> 톰캣 9.x에서는 JNDI 리소스를 사용하여 데이터베이스 연결을 정의할 수 있습니다. JNDI 리소스를 사용하면 `server.xml` 파일에 직접 암호화된 사용자 이름과 비밀번호를 저장할 필요가 없으므로 보안성이 향상됩니다.

JNDI 리소스를 사용하여 데이터베이스 연결을 설정하려면 다음 단계를 따르세요.

1. `server.xml` 파일에서 `GlobalNamingResources` 섹션을 찾습니다.

2. `GlobalNamingResources` 섹션 내에서 `Resource` 요소를 추가합니다. `Resource` 요소는 데이터베이스 연결 정보를 정의합니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="myuser"
            password="mypassword"
            maxActive="20" maxIdle="10" maxWait="-1" />
</GlobalNamingResources>
```

위의 설정에서 `name` 속성은 JNDI 이름을 나타냅니다. `auth` 속성은 인증 방법을 나타내며, `type` 속성은 사용할 JDBC 드라이버를 나타냅니다. `username`과 `password` 속성은 데이터베이스에 연결할 때 사용됩니다. `maxActive`, `maxIdle`, `maxWait` 속성은 커넥션 풀링을 위한 설정입니다.

3. 암호화된 사용자 이름과 비밀번호를 사용하려면, `username`과 `password` 속성 값을 암호화된 문자열로 지정하면 됩니다. 암호화된 문자열은 암호화 키를 사용하여 생성한 뒤, `server.xml` 파일에 저장할 수 있습니다. 암호화된 문자열을 디코딩하려면, 암호화 키가 필요합니다.

예를 들어, Jasypt 라이브러리를 사용하여 암호화된 사용자 이름과 비밀번호를 생성하려면, 다음과 같은 설정을 추가할 수 있습니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="ENC(nDgJhKjLmNoQrStUvWxZy16==)"
            password="ENC(pRqStUvWxZ1/3+6A9DcFfIhKjM=)"
            maxActive="20" maxIdle="10" maxWait="-1" />
  <ResourceParams name="jdbc/mydb">
    <parameter>
      <name>username</name>
      <value>ENC(nDgJhKjLmNoQrStUvWxZy16==)</value>
    </parameter>
    <parameter>
      <name>password</name>
      <value>ENC(pRqStUvWxZ1/

## 사용자 입력:
> 위 내용이  끊겼는데, 이어서 계속 

## ChatGPT 응답:
> 3. 암호화된 사용자 이름과 비밀번호를 사용하려면, `username`과 `password` 속성 값을 암호화된 문자열로 지정하면 됩니다. 암호화된 문자열은 암호화 키를 사용하여 생성한 뒤, `server.xml` 파일에 저장할 수 있습니다. 암호화된 문자열을 디코딩하려면, 암호화 키가 필요합니다.

예를 들어, Jasypt 라이브러리를 사용하여 암호화된 사용자 이름과 비밀번호를 생성하려면, 다음과 같은 설정을 추가할 수 있습니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="ENC(nDgJhKjLmNoQrStUvWxZy16==)"
            password="ENC(pRqStUvWxZ1/3+6A9DcFfIhKjM=)"
            maxActive="20" maxIdle="10" maxWait="-1" />
  <ResourceParams name="jdbc/mydb">
    <parameter>
      <name>username</name>
      <value>ENC(nDgJhKjLmNoQrStUvWxZy16==)</value>
    </parameter>
    <parameter>
      <name>password</name>
      <value>ENC(pRqStUvWxZ1/3+6A9DcFfIhKjM=)</value>
    </parameter>
  </ResourceParams>
</GlobalNamingResources>
```

위의 설정에서 `username`과 `password` 속성 값은 `ENC(...)`로 시작하는 암호화된 문자열로 지정되어 있습니다. 암호화된 문자열은 Jasypt 라이브러리를 사용하여 생성한 뒤, 암호화 키와 함께 `server.xml` 파일에 저장되어 있습니다.

암호화된 문자열을 디코딩하려면, 암호화 키와 함께 Jasypt 라이브러리를 사용하여 디코딩해야 합니다. 디코딩된 사용자 이름과 비밀번호는 데이터베이스에 연결할 때 사용됩니다. 디코딩된 사용자 이름과 비밀번호를 얻으려면, `ResourceParams` 요소를 사용하여 디코딩할 속성을 지정해야 합니다.

위의 설정에서는 `ResourceParams` 요소를 사용하여 `username`과 `password` 속성 값을 디코딩합니다. `name` 요소는 디코딩할 속성 이름을 나타내며, `value` 요소는 디코딩할 암호화된 문자열을 나타냅니다.

## 사용자 입력:
> 암호화된 문자열은 Jasypt 라이브러리를 사용하여 생성하는 방법 알려줘

## ChatGPT 응답:
> Jasypt는 Java 애플리케이션에서 문자열을 암호화하고 복호화하는 라이브러리입니다. Jasypt를 사용하여 사용자 이름과 비밀번호를 암호화하려면 다음 단계를 따르세요.

1. Jasypt 라이브러리를 다운로드하고 클래스 경로에 추가하세요. Jasypt는 Maven 등을 사용하여 의존성 관리를 할 수 있습니다.

2. Jasypt를 사용하여 암호화할 속성 값을 가져옵니다.

```java
String username = "myuser";
String password = "mypassword";

// 암호화 키를 지정합니다.
String passwordEncryptorPassword = "passwordEncryptorPassword";

// Jasypt StandardPBEStringEncryptor를 생성합니다.
StandardPBEStringEncryptor encryptor = new StandardPBEStringEncryptor();
encryptor.setPassword(passwordEncryptorPassword);

// 속성 값을 암호화합니다.
String encryptedUsername = encryptor.encrypt(username);
String encryptedPassword = encryptor.encrypt(password);

System.out.println("Encrypted username: " + encryptedUsername);
System.out.println("Encrypted password: " + encryptedPassword);
```

위의 코드에서는 `myuser`와 `mypassword`를 암호화합니다. 암호화 키는 `passwordEncryptorPassword`로 지정합니다. `StandardPBEStringEncryptor`를 사용하여 속성 값을 암호화하고, `encrypt` 메소드를 호출하여 암호화된 문자열을 얻습니다.

3. 암호화된 문자열을 `server.xml` 파일에 저장합니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="ENC(암호화된 사용자 이름)"
            password="ENC(암호화된 비밀번호)"
            maxActive="20" maxIdle="10" maxWait="-1" />
</GlobalNamingResources>
```

위의 설정에서는 `username`과 `password` 속성 값이 `ENC(...)`로 시작하는 암호화된 문자열로 지정되어 있습니다. 이전 단계에서 생성한 암호화된 문자열을 `ENC(...)`로 묶은 뒤, `server.xml` 파일에 저장합니다.

암호화된 문자열을 디코딩하려면, 암호화 키와 함께 Jasypt 라이브러리를 사용하여 디코딩해야 합니다.

## 사용자 입력:
> 위 설정에서 openssl enc -aes-256-cbc -a -salt -pass pass 이걸로 만든 username password를 사용할 수 있도록 해줘

## ChatGPT 응답:
> `openssl enc` 명령어를 사용하여 암호화된 문자열을 생성하려면 다음 단계를 따르세요.

1. 사용자 이름과 비밀번호를 암호화합니다.

```
$ echo -n 'myuser' | openssl enc -aes-256-cbc -a -salt -pass pass:passwordEncryptorPassword
# 출력 예: U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=

$ echo -n 'mypassword' | openssl enc -aes-256-cbc -a -salt -pass pass:passwordEncryptorPassword
# 출력 예: U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=
```

위의 예제에서는 `myuser`와 `mypassword`를 각각 암호화합니다. `-n` 옵션을 사용하여 출력에 개행 문자를 포함시키지 않습니다. `-aes-256-cbc`는 AES-256-CBC 암호화 알고리즘을 사용하여 암호화하도록 지정합니다. `-a`는 Base64 인코딩을 사용하여 암호화된 문자열을 출력합니다. `-salt`는 암호화할 때 솔트를 사용하도록 지정합니다. `-pass pass:passwordEncryptorPassword`는 암호화 키를 `passwordEncryptorPassword`로 지정합니다.

2. `server.xml` 파일에서 암호화된 사용자 이름과 비밀번호를 지정합니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="ENC(U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=)"
            password="ENC(U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=)"
            maxActive="20" maxIdle="10" maxWait="-1" />
</GlobalNamingResources>
```

위의 설정에서는 `username`과 `password` 속성 값이 `ENC(...)`로 시작하는 암호화된 문자열로 지정되어 있습니다. 앞서 생성한 암호화된 문자열을 `ENC(...)`로 묶은 뒤, `server.xml` 파일에 저장합니다.

암호화된 문자열을 디코딩하려면, `openssl enc` 명령어를 사용하여 암호화된 문자열을 복호화하고, `ResourceParams` 요소를 사용하여 디코딩할 속성을 지정해야 합니다.

## 사용자 입력:
> 암호화된 문자열을 디코딩하려면, openssl enc 명령어를 사용하여 암호화된 문자열을 복호화하고, ResourceParams 요소를 사용하여 디코딩할 속성을 지정하는 방법 알려줘

## ChatGPT 응답:
> 암호화된 문자열을 디코딩하려면, 다음 단계를 따르세요.

1. `openssl enc` 명령어를 사용하여 암호화된 문자열을 복호화합니다.

```
$ echo 'U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=' | openssl enc -aes-256-cbc -a -d -salt -pass pass:passwordEncryptorPassword
# 출력 예: myuser

$ echo 'U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=' | openssl enc -aes-256-cbc -a -d -salt -pass pass:passwordEncryptorPassword
# 출력 예: mypassword
```

위의 예제에서는 `U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=`와 `U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=`를 각각 복호화합니다. `-d` 옵션을 사용하여 입력을 복호화하도록 지정합니다. 나머지 옵션은 암호화할 때와 동일합니다.

2. `ResourceParams` 요소를 사용하여 디코딩할 속성을 지정합니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="ENC(U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=)"
            password="ENC(U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=)"
            maxActive="20" maxIdle="10" maxWait="-1" />
  <ResourceParams name="jdbc/mydb">
    <parameter>
      <name>username</name>
      <value>myuser</value>
    </parameter>
    <parameter>
      <name>password</name>
      <value>mypassword</value>
    </parameter>
  </ResourceParams>
</GlobalNamingResources>
```

위의 설정에서는 `ResourceParams` 요소를 사용하여 `username`과 `password` 속성 값을 디코딩합니다. `name` 요소는 디코딩할 속성 이름을 나타내며, `value` 요소는 디코딩한 값을 나타냅니다. 디코딩한 사용자 이름과 비밀번호는 `ResourceParams` 요소를 사용하여 `Resource` 요소에 지정된 JNDI 이름과 연결됩니다.

위의 설정에서는 `myuser`와 `mypassword`를 각각 디코딩하여 `ResourceParams` 요소에 지정하였습니다. 이제 JNDI 이름 `jdbc/mydb`로 데이터베이스에 연결할 때, 디코딩한 사용자 이름과 비

## 사용자 입력:
> 톰캣 9.x db연결 username password 암복호화 설정방법 .server.xml

## ChatGPT 응답:
> Tomcat 9.x에서 DB 연결을 위한 `server.xml` 파일에 암호화된 사용자 이름과 비밀번호를 추가하는 방법에 대해 이전에 설명드렸습니다. 이제는 `server.xml` 파일에서 암호화된 사용자 이름과 비밀번호를 복호화하여 데이터베이스에 연결하는 방법을 알려드리겠습니다.

1. 먼저, Jasypt 라이브러리를 다운로드하고 클래스 경로에 추가합니다.

2. Jasypt를 사용하여 복호화할 암호화된 사용자 이름과 비밀번호를 가져옵니다.

```java
// 암호화된 사용자 이름과 비밀번호
String encryptedUsername = "U2FsdGVkX18yyzWk0vhnsaw71zOpAxwI2DdOnd6zoOU=";
String encryptedPassword = "U2FsdGVkX18MSWz3vC1eB97IHwAYNkADx5n5o5A5S5E=";

// 암호화 키를 지정합니다.
String passwordEncryptorPassword = "passwordEncryptorPassword";

// Jasypt StandardPBEStringEncryptor를 생성합니다.
StandardPBEStringEncryptor encryptor = new StandardPBEStringEncryptor();
encryptor.setPassword(passwordEncryptorPassword);

// 사용자 이름과 비밀번호를 복호화합니다.
String username = encryptor.decrypt(encryptedUsername);
String password = encryptor.decrypt(encryptedPassword);

System.out.println("Decrypted username: " + username);
System.out.println("Decrypted password: " + password);
```

위의 코드에서는 암호화된 사용자 이름과 비밀번호를 `encryptedUsername`와 `encryptedPassword` 변수에 저장합니다. 암호화 키는 `passwordEncryptorPassword`로 지정합니다. `StandardPBEStringEncryptor`를 사용하여 복호화하고, `decrypt` 메소드를 호출하여 복호화된 값을 얻습니다.

3. `server.xml` 파일에서 암호화된 사용자 이름과 비밀번호를 복호화한 값으로 지정합니다.

```xml
<GlobalNamingResources>
  <Resource name="jdbc/mydb" auth="Container"
            type="javax.sql.DataSource"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost/mydb"
            username="myuser"
            password="mypassword"
            maxActive="20" maxIdle="10" maxWait="-1" />
  <ResourceParams name="jdbc/mydb">
    <parameter>
      <name>username</name>
      <value>myuser</value>
    </parameter>
    <parameter>
      <name>password</name>
      <value>mypassword</value>
    </parameter>
  </ResourceParams>
</GlobalNamingResources>
```

위의 설정에서는 `ResourceParams` 요소를 사용하여 `username`과 `password` 속성 값을 복호화한 값으로 지정합니다. `name` 요소는 디코딩할 속성 이름을 나타내며, `value` 요소는 디코딩한 값을 나타냅니다. 디코딩한 사용자 이름과 비밀번호는 `ResourceParams` 요소를 사용하여 `Resource` 요

## 사용자 입력:
> 위 내용으로 하면, 보안에 문제가 있잖아.

## ChatGPT 응답:
> 암호화된 사용자 이름과 비밀번호를 `server.xml` 파일에 저장하면 보안에 취약합니다. 따라서, 실제로는 이러한 값들을 `server.xml` 파일에 직접 저장하는 것은 권장되지 않습니다. 

보안을 위해서는 암호화된 값들을 외부에서 로드하는 방법을 사용하는 것이 좋습니다. 예를 들어, 외부 설정 파일에 암호화된 값들을 저장하고, `server.xml` 파일에서는 이러한 값들을 참조하도록 설정할 수 있습니다.

또는, JNDI를 사용하여 암호화된 값들을 저장하고, `server.xml` 파일에서는 JNDI를 사용하여 이러한 값들을 참조하도록 설정할 수도 있습니다. JNDI를 사용하면, 암호화된 값들을 안전하게 저장하고, 필요한 경우 애플리케이션 코드에서 JNDI를 사용하여 이러한 값들을 얻을 수 있습니다.

보안을 위해서는 암호화된 값들을 안전하게 저장하고, 애플리케이션에서 필요한 경우에만 이러한 값을 얻도록 해야 합니다.

