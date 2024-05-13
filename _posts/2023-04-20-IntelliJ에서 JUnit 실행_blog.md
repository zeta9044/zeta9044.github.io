---
layout: post
title: "IntelliJ에서 JUnit 실행"
date: 2023-04-20
tags: [JUnit, 실행]
categories: blog
---

## 사용자 입력:
> intellij에서 junit를 실행하는데, gradle로 실행하는 거 말고 intellij로 실행하는 설정하는 거 알려줘

## ChatGPT 응답:
> IntelliJ에서 Gradle 대신 IntelliJ로 JUnit 테스트를 실행하려면 다음 단계를 따르세요:

1. IntelliJ IDEA를 엽니다.
2. 오른쪽 상단 모서리에 있는 `Gradle`을 찾아서 클릭합니다.
3. 열린 `Gradle` 창에서 프로젝트 이름을 찾아 우클릭하고, `Open Module Settings`를 선택합니다.
4. 새로운 창에서, 왼쪽 패널에서 `Build, Execution, Deployment`를 선택합니다.
5. 하위 메뉴에서 `Build Tools`를 클릭한 다음, `Gradle`를 선택합니다.
6. 오른쪽 패널에서 `Build and run using` 및 `Run tests using` 드롭다운 메뉴에서 `IntelliJ IDEA`를 선택합니다.
7. 변경 사항을 저장하려면 `Apply`를 클릭하고, `OK`를 클릭하여 창을 닫습니다.

이제 IntelliJ에서 JUnit 테스트를 직접 실행할 수 있습니다. 이렇게 하면 프로젝트를 실행하거나 테스트할 때 IntelliJ가 기본 빌드 도구로 사용되며, Gradle 대신에 IntelliJ로 실행하게 됩니다.

## 사용자 입력:
> ojdbc.jar에서 oracle.jdbc.OraclePreparedStatement 이런 패키지가 있나?

## ChatGPT 응답:
> ojdbc.jar 파일에는 `oracle.jdbc.OraclePreparedStatement` 클래스가 없습니다. 대신에 `oracle.jdbc.driver.OraclePreparedStatement`라는 클래스가 있습니다. 이 클래스는 `oracle.jdbc.driver` 패키지 내에 위치하며, `PreparedStatement` 인터페이스를 구현합니다.

`OraclePreparedStatement` 대신 `PreparedStatement`를 사용하는 것이 일반적입니다. 이렇게 하면 코드가 데이터베이스 벤더에 종속되지 않고, 표준 JDBC 인터페이스를 사용하여 다른 데이터베이스로 쉽게 전환할 수 있습니다. 예를 들어, 다음과 같이 코드를 작성할 수 있습니다:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class JdbcExample {
    public static void main(String[] args) {
        try {
            // Load the Oracle JDBC driver
            Class.forName("oracle.jdbc.driver.OracleDriver");

            // Connect to the database
            Connection connection = DriverManager.getConnection("jdbc:oracle:thin:@//<HOST>:<PORT>/<SID>", "<USERNAME>", "<PASSWORD>");

            // Prepare a SQL query
            String sql = "SELECT * FROM my_table WHERE id = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(sql);
            
            // Set the parameter value
            preparedStatement.setInt(1, 1);
            
            // Execute the query
            ResultSet resultSet = preparedStatement.executeQuery();
            
            // Process the result set
            while (resultSet.next()) {
                System.out.println("Column1: " + resultSet.getString("column1"));
                System.out.println("Column2: " + resultSet.getString("column2"));
            }
            
            // Close the resources
            resultSet.close();
            preparedStatement.close();
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

이 코드는 표준 JDBC 인터페이스를 사용하여 Oracle 데이터베이스에 연결하고, `PreparedStatement`를 사용하여 SQL 쿼리를 실행합니다.

## 사용자 입력:
> import oracle.jdbc.OraclePreparedStatement;
import oracle.jdbc.OracleResultSet;
import oracle.sql.CLOB; 를 ojdbc8.jar에 대응하는 걸로 바꿔줘.

## ChatGPT 응답:
> ojdbc8.jar 파일은 Oracle JDBC 드라이버의 버전 12.2.0.1부터 사용할 수 있는 파일입니다. 이 버전에서는 패키지 이름이 약간 변경되었습니다. 아래 코드는 ojdbc8.jar를 사용하는 방법에 맞게 수정된 것입니다.

```java
import oracle.jdbc.OracleConnection;
import oracle.jdbc.OraclePreparedStatement;
import oracle.jdbc.OracleResultSet;
import oracle.sql.CLOB;
```

하지만 표준 JDBC 인터페이스를 사용하는 것이 좋습니다. 이렇게 하면 코드가 데이터베이스 벤더에 종속되지 않고, 다른 데이터베이스로 쉽게 전환할 수 있습니다. 표준 인터페이스를 사용하는 코드는 다음과 같습니다.

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Clob;
```

표준 인터페이스를 사용하면 코드가 더 유연해지고 데이터베이스간 호환성이 향상됩니다.

## 사용자 입력:
> 최강유
import oracle.jdbc.OraclePreparedStatement;
import oracle.jdbc.OracleResultSet;
import oracle.sql.CLOB; 를 ojdbc10.jar의 버전 54.0 에 대응하는 걸로 바꿔줘.

## ChatGPT 응답:
> ojdbc10.jar는 Oracle JDBC 드라이버의 버전 21c를 위한 파일입니다. 이 버전에서도 패키지 이름은 이전 버전과 동일합니다. 따라서 ojdbc10.jar 버전 54.0을 사용하는 경우에도 아래와 같이 import 구문을 사용하면 됩니다.

```java
import oracle.jdbc.OraclePreparedStatement;
import oracle.jdbc.OracleResultSet;
import oracle.sql.CLOB;
```

그러나 코드의 벤더 독립성과 데이터베이스 간 호환성을 유지하려면 표준 JDBC 인터페이스를 사용하는 것이 좋습니다. 표준 인터페이스를 사용한 코드는 다음과 같습니다.

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Clob;
```

표준 인터페이스를 사용하면 코드가 더 유연해지고 데이터베이스간 호환성이 향상됩니다.

