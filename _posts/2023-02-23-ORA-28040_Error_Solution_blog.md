---
layout: post
title: "ORA-28040 Error Solution"
tags: [Solution, ORA, Error]
---

## 사용자 입력:
> java.sql.SQLException: ORA-28040: No matching authentication protocol

## ChatGPT 응답:
> This error message typically indicates that the version of the Oracle client software that you are using is not compatible with the authentication protocol that is being used by the Oracle database.

To resolve this issue, you should upgrade your Oracle client software to a version that supports the authentication protocol that is being used by the Oracle database. You can also try changing the authentication protocol used by the Oracle database to a protocol that is supported by your current Oracle client software.

You may also want to check if the user credentials that you are using to connect to the database are correct and have the appropriate permissions to access the database. 

If you are still having issues, you should consult the Oracle documentation or contact Oracle support for further assistance.

## 사용자 입력:
> oracle 19c와 호환되는 client jdbc jar는?

## ChatGPT 응답:
> Oracle 19c의 JDBC 드라이버는 다음 링크에서 다운로드할 수 있습니다.

https://www.oracle.com/database/technologies/jdbc-drivers-19c-downloads.html

위 링크에서 다운로드 가능한 파일에는 다음과 같은 것들이 있습니다.

- ojdbc8.jar : JDBC 4.2, Java 8 및 JDBC 4.1, 4.0을 지원합니다.
- ojdbc10.jar : JDBC 4.2, Java 10을 지원합니다.
- ojdbc11.jar : JDBC 4.2, Java 11 및 JDBC 4.1, 4.0을 지원합니다.
- ojdbc.jar : 이전 버전의 JDBC를 지원합니다.

이 중에서 사용하고자 하는 Java 버전과 JDBC 버전에 맞게 선택하여 다운로드하시면 됩니다. 예를 들어, Java 11을 사용하는 경우 ojdbc11.jar를 사용할 수 있습니다.

## 사용자 입력:
> java.sql.SQLException: ORA-28040: No matching authentication protocol, jdbc버전 문제인가?

## ChatGPT 응답:
> 예, 이 오류는 JDBC 드라이버 버전이 호환되지 않아 발생할 수 있습니다. ORA-28040 오류는 데이터베이스에서 사용하는 인증 프로토콜과 JDBC 드라이버에서 사용하는 인증 프로토콜이 호환되지 않을 때 발생합니다.

Oracle 19c는 JDBC 드라이버 버전 11.x 이상을 사용해야 합니다. 만약 Oracle 19c와 호환되는 JDBC 드라이버 버전을 사용하고 있지 않은 경우 이 오류가 발생할 수 있습니다. 

따라서 Oracle 19c와 호환되는 JDBC 드라이버 버전을 사용하는지 확인하십시오. Oracle 공식 웹사이트에서 JDBC 드라이버 다운로드 페이지에서 Oracle 19c와 호환되는 JDBC 드라이버를 다운로드하여 사용하시면 됩니다.

