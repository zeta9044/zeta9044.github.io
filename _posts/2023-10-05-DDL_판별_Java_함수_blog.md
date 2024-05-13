---
layout: post
title: "DDL 판별 Java 함수"
date: 2023-10-05
tags: [함수, Java, DDL, 판별]
categories: blog
---

## 사용자 입력:
> java로 DDL문을 판별하는 함수 만들어줘.

## ChatGPT 응답:
> DDL(Data Definition Language) 문을 판별하는 Java 함수를 제공하겠습니다. DDL 문에는 주로 `CREATE`, `ALTER`, `DROP`, `TRUNCATE` 등의 키워드가 포함되어 있습니다. 아래는 이러한 키워드를 기반으로 DDL 문을 판별하는 간단한 Java 함수입니다:

```java
public class DDLChecker {

    // DDL 키워드 리스트
    private static final String[] DDL_KEYWORDS = {"CREATE", "ALTER", "DROP", "TRUNCATE"};

    /**
     * 주어진 SQL 쿼리가 DDL 문인지 판별
     * @param sqlQuery SQL 쿼리 문자열
     * @return DDL 문이면 true, 그렇지 않으면 false
     */
    public static boolean isDDL(String sqlQuery) {
        if (sqlQuery == null) {
            return false;
        }

        String trimmedQuery = sqlQuery.trim().toUpperCase();
        for (String keyword : DDL_KEYWORDS) {
            if (trimmedQuery.startsWith(keyword)) {
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        String testQuery1 = "CREATE TABLE users (id INT, name VARCHAR(100));";
        String testQuery2 = "SELECT * FROM users;";
        
        System.out.println(isDDL(testQuery1)); // true
        System.out.println(isDDL(testQuery2)); // false
    }
}
```

위 코드에서 `isDDL` 함수는 주어진 SQL 쿼리 문자열이 DDL인지 판별합니다. 주의해야 할 점은 이 함수는 기본적인 DDL 판별만을 수행하므로, 복잡한 SQL 쿼리나 다양한 형태의 DDL 문에 대한 완벽한 판별을 원한다면 더 고도화된 로직이 필요합니다.

