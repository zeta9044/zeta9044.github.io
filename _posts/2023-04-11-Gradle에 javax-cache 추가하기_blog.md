---
layout: post
title: "Gradle에 javax.cache 추가하기"
date: 2023-04-11
tags: [javax, cache, 추가]
categories: blog
---

## 사용자 입력:
> implementation 'javax.cache:cache-api:1.0.0'  이렇게 build.gradle에 선언하면, war 만들어질때, WEB-INF/lib에 라이브러리가 안들어가나?

## ChatGPT 응답:
> `javax.cache:cache-api:1.0.0` 라이브러리를 build.gradle 파일에 선언한다고 하여도, 해당 라이브러리가 WAR 파일의 WEB-INF/lib 폴더에 포함될지 여부는 다른 요소들에 달려 있습니다. 

일반적으로 Gradle은 `compile` 또는 `implementation` 의존성으로 선언된 라이브러리를 자동으로 WAR 파일에 추가합니다. 그러나 다른 요인들이 영향을 미칠 수 있습니다. 예를 들어, Gradle 설정 파일에 다음과 같은 코드가 포함되어 있다면:

```gradle
war {
   classpath fileTree('lib') // WAR 파일에 추가할 외부 라이브러리 디렉토리 경로
   webInf {
      from 'src/main/resources/web-inf' // 추가로 포함할 파일 및 디렉터리 경로
   }
}
```

이 설정이 있다면 `javax.cache:cache-api:1.0.0` 라이브러리는 WAR 파일에 추가되지 않을 수 있습니다. 따라서 해당 설정 파일을 분석해야 할 것입니다.

그러나 보통 Gradle에서는 라이브러리를 제대로 추가하기 위해 `compile` 또는 `implementation` 의존성으로 선언되면 라이브러리가 WAR 파일에 추가됩니다.

## 사용자 입력:
> javax.cache:cache-api:1.0.0 라이브러리가 WEB-INF/lib에 있는데도 불구하고, WAS서버에서 못찾는다는 메세지가 나올때는 어떻게 해야 하나?

## ChatGPT 응답:
> WAS 서버에서 라이브러리를 찾지 못하는 경우, 다음과 같은 몇 가지 문제점이 있을 수 있습니다.

1. 라이브러리 버전 충돌: WAS 서버에 설치된 라이브러리 버전과 프로젝트에서 사용한 라이브러리 버전이 다른 경우, 충돌이 발생할 수 있습니다. 이 경우에는 서버에 설치된 라이브러리 버전과 프로젝트에서 사용한 라이브러리 버전을 일치시켜야 합니다.

2. 라이브러리 파일 위치: 라이브러리 파일이 WAR 파일의 WEB-INF/lib 폴더에 있는데도 WAS 서버가 해당 라이브러리를 찾지 못하는 경우, WAR 파일의 배포 경로가 잘못 설정된 것일 수 있습니다. 이 경우에는 WAR 파일을 다시 빌드하거나, WAS 서버의 배포 경로를 확인해야 합니다.

3. 라이브러리 사용 설정: WAS 서버에서 라이브러리를 사용하도록 설정되어 있지 않은 경우, 해당 라이브러리를 사용할 수 없습니다. 이 경우에는 WAS 서버에서 라이브러리를 사용할 수 있는 설정을 확인하고, 필요한 경우 설정을 수정해야 합니다.

4. 라이브러리 로딩 실패: 라이브러리 로딩 중에 오류가 발생한 경우, 해당 라이브러리를 사용할 수 없습니다. 이 경우에는 오류 메시지를 확인하고, 필요한 경우 로그 파일을 분석하여 문제를 해결해야 합니다.

위와 같은 문제점을 해결하기 위해서는 WAS 서버의 설정을 확인하고, 문제가 있는 경우 설정을 수정하거나 프로젝트를 다시 빌드해야 합니다. 또한, 오류 메시지를 자세히 분석하여 문제점을 파악하고, 해결 방법을 찾아야 합니다.

## 사용자 입력:
> 한국의 제우스 서버에서 JNDI 데이터 소스를 가져올 때 "encoding" 속성을 UTF-8로 설정하는 방법은?

## ChatGPT 응답:
> 한국의 제우스 서버에서 JNDI 데이터 소스를 가져올 때 "encoding" 속성을 UTF-8로 설정하려면 다음과 같이 하면 됩니다.

1. JNDI 데이터 소스를 정의하는 XML 파일을 엽니다. 보통은 server.xml 또는 context.xml 파일입니다.
2. JNDI 데이터 소스의 `<Resource>` 요소에서 `URLEncoding` 속성을 UTF-8로 설정합니다.

예를 들어, 다음과 같이 `<Resource>` 요소를 정의했다고 가정합니다.

```xml
<Resource name="jdbc/myDataSource" auth="Container" type="javax.sql.DataSource" driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/mydb" username="myuser" password="mypassword" />
```

이 경우, `URLEncoding` 속성을 추가하여 다음과 같이 설정합니다.

```xml
<Resource name="jdbc/myDataSource" auth="Container" type="javax.sql.DataSource" driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/mydb" username="myuser" password="mypassword" URLEncoding="UTF-8" />
```

위와 같이 설정하면 JNDI 데이터 소스에서 UTF-8 인코딩을 사용할 수 있습니다.

## 사용자 입력:
> 톰캣 서버에서 JNDI 데이터 소스를 가져올 때 "encoding" 속성을 UTF-8로 설정하는 방법은?

## ChatGPT 응답:
> 톰캣 서버에서 JNDI 데이터 소스를 가져올 때 "encoding" 속성을 UTF-8로 설정하려면 다음과 같이 하면 됩니다.

1. JNDI 데이터 소스를 정의하는 XML 파일을 엽니다. 보통은 server.xml 또는 context.xml 파일입니다.
2. JNDI 데이터 소스의 `<Resource>` 요소에서 `connectionProperties` 속성을 추가합니다. 
3. `connectionProperties` 속성에 `useUnicode=true&characterEncoding=UTF-8` 값을 설정합니다.

예를 들어, 다음과 같이 `<Resource>` 요소를 정의했다고 가정합니다.

```xml
<Resource name="jdbc/myDataSource" auth="Container" type="javax.sql.DataSource" driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/mydb" username="myuser" password="mypassword" />
```

이 경우, `connectionProperties` 속성을 추가하여 다음과 같이 설정합니다.

```xml
<Resource name="jdbc/myDataSource" auth="Container" type="javax.sql.DataSource" driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/mydb" username="myuser" password="mypassword" connectionProperties="useUnicode=true&amp;characterEncoding=UTF-8" />
```

위와 같이 설정하면 JNDI 데이터 소스에서 UTF-8 인코딩을 사용할 수 있습니다.

