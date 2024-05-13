---
layout: post
title: "JSqlParser 유형 식별"
date: 2024-02-27
tags: [식별, 유형, JSqlParser]
categories: blog
---

## 사용자 입력:
> Jsqlparser를 이용해서, 어떤 문자열이 DBMS의 query,dml,ddl인지 알려고 한다.

## ChatGPT 응답:
> JSqlParser는 Java로 작성된 SQL 문을 파싱하는 라이브러리입니다. 이 라이브러리를 사용하여 주어진 문자열이 SQL 쿼리, DML (데이터 조작 언어), 또는 DDL (데이터 정의 언어) 중 어떤 것인지 식별할 수 있습니다.

다음은 JSqlParser를 사용하여 문자열이 어떤 유형의 SQL인지 확인하는 간단한 Java 코드의 예시입니다. 이 코드는 SQL 문의 첫 번째 토큰을 가져와서 해당 토큰이 어떤 유형인지 확인합니다.

```java
import net.sf.jsqlparser.JSQLParserException;
import net.sf.jsqlparser.parser.CCJSqlParserUtil;
import net.sf.jsqlparser.statement.Statement;

public class SqlTypeIdentifier {

    public static void main(String[] args) {
        String sql = "SELECT * FROM my_table"; // 여기에 분석할 SQL문을 넣으세요

        try {
            Statement statement = CCJSqlParserUtil.parse(sql);

            if (statement != null) {
                if (statement.toString().toUpperCase().startsWith("SELECT")) {
                    System.out.println("This is a SELECT statement.");
                } else if (statement.toString().toUpperCase().startsWith("INSERT")
                        || statement.toString().toUpperCase().startsWith("UPDATE")
                        || statement.toString().toUpperCase().startsWith("DELETE")) {
                    System.out.println("This is a DML statement.");
                } else if (statement.toString().toUpperCase().startsWith("CREATE")
                        || statement.toString().toUpperCase().startsWith("ALTER")
                        || statement.toString().toUpperCase().startsWith("DROP")) {
                    System.out.println("This is a DDL statement.");
                } else {
                    System.out.println("Unknown SQL statement type.");
                }
            } else {
                System.out.println("Failed to parse SQL statement.");
            }
        } catch (JSQLParserException e) {
            e.printStackTrace();
        }
    }
}
```

이 코드는 SQL 문의 첫 번째 토큰을 검사하여 SELECT, INSERT/UPDATE/DELETE, CREATE/ALTER/DROP 중 어떤 유형인지 식별합니다. 하지만 이는 간단한 방법이며 완전한 검증을 위해서는 더 많은 로직과 검사가 필요할 수 있습니다.

## 사용자 입력:
> parse만 되면, dbms에서 사용하는 문자열이라고 판단되는 거지?

## ChatGPT 응답:
> 네, JSqlParser를 사용하여 SQL 문을 성공적으로 파싱하는 것은 해당 문자열이 SQL 문의 구문론적으로 유효한 형태로 작성되었다는 것을 의미합니다. 즉, 문법적으로 문제가 없고 SQL 쿼리나 명령어로 해석될 수 있다는 것을 나타냅니다.

그러나 이것은 해당 SQL 문이 특정 DBMS에서 실제로 실행될 수 있는지 여부에 대한 보장이 아닙니다. SQL 문의 유효성을 파악하는 것은 단순히 구문 분석에 초점을 맞춘 것이며, 각 DBMS는 자체적인 확장이나 문법 규칙을 가지고 있을 수 있습니다.

따라서 SQL 문이 특정 DBMS에서 사용 가능한 것인지 확인하려면 해당 DBMS의 문서를 참고하거나, 파싱된 SQL 문을 실행하여 DBMS에서 어떻게 처리되는지 확인해야 합니다. JSqlParser는 문법적인 측면에서 도움이 되지만, DBMS에 의해 지원되는 기능 및 특성을 고려해야 합니다.

