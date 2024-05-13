---
layout: post
title: "Java 및 Oracle CLOB 데이터 삽입"
date: 2023-12-26
tags: [및, Java, 삽입, Oracle, 데이터, CLOB]
categories: blog
---

## 사용자 입력:
> java 그리고 오라클 jdbc를 이용해서 4000자이상의 문자열을 clob 컬럼에 insert update하는 예제 보여줘

## ChatGPT 응답:
> Java와 Oracle JDBC를 사용하여 4000자 이상의 문자열을 CLOB 컬럼에 `INSERT`하고 `UPDATE`하는 예제를 보여드리겠습니다.

### 예제: CLOB 컬럼에 문자열 삽입 및 업데이트

#### 1. 필요한 라이브러리

Oracle JDBC 드라이버를 포함해야 합니다. Maven 프로젝트의 경우 `pom.xml`에 다음과 같이 추가합니다.

```xml
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc8</artifactId>
    <version>19.3.0.0</version>
</dependency>
```

#### 2. 데이터베이스 연결 설정

```java
String url = "jdbc:oracle:thin:@호스트:포트:DB이름";
String username = "사용자명";
String password = "비밀번호";
```

#### 3. CLOB 컬럼에 문자열 삽입 (INSERT)

```java
import java.sql.*;

public class InsertClobExample {
    public static void main(String[] args) {
        String sql = "INSERT INTO 테이블명 (clob_column) VALUES (?)";

        try (Connection conn = DriverManager.getConnection(url, username, password);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            String longString = "4000자 이상의 문자열..."; // 삽입할 문자열
            pstmt.setString(1, longString);
            pstmt.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 4. CLOB 컬럼 업데이트 (UPDATE)

```java
public class UpdateClobExample {
    public static void main(String[] args) {
        String sql = "UPDATE 테이블명 SET clob_column = ? WHERE 조건";

        try (Connection conn = DriverManager.getConnection(url, username, password);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            String newLongString = "새로운 4000자 이상의 문자열..."; // 업데이트할 문자열
            pstmt.setString(1, newLongString);
            pstmt.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 주의사항
- `테이블명`과 `조건`은 실제 사용하는 테이블 이름과 조건으로 교체해야 합니다.
- 문자열이 매우 긴 경우, `setString` 메소드 대신 `setCharacterStream` 또는 `setClob`을 사용할 수도 있습니다.
- Oracle 데이터베이스 및 JDBC 드라이버 버전에 따라 코드가 다를 수 있습니다.
- 항상 데이터베이스 연결을 안전하게 관리하고, 예외 처리를 적절히 수행해야 합니다.

