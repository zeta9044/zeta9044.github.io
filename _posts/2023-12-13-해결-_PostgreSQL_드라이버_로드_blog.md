---
layout: post
title: "해결: PostgreSQL 드라이버 로드"
date: 2023-12-13
tags: [드라이버, PostgreSQL, 해결, 로드]
categories: blog
---

## 사용자 입력:
> ERROR com.zaxxer.hikari.HikariConfig - Failed to load driver class org.postgresql.Driver  # PostgreSQL driver from HikariConfig class classloader jdk.internal.loader.ClassLoaders$AppClassLoader@69663380 어떻게 조치해야 될까?

## ChatGPT 응답:
> The error message you're seeing, "Failed to load driver class org.postgresql.Driver from HikariConfig class classloader," typically occurs when your Java application is unable to find the PostgreSQL JDBC driver. This problem often arises due to issues with the classpath or missing dependencies. Here are some steps to resolve it:

1. **Add the PostgreSQL JDBC Driver to Your Project:**
   - If you are using Maven, add the following dependency in your `pom.xml` file:

     ```xml
     <dependency>
         <groupId>org.postgresql</groupId>
         <artifactId>postgresql</artifactId>
         <version>LATEST_VERSION</version>
     </dependency>
     ```
   - If you're using Gradle, add this to your `build.gradle` file:

     ```gradle
     dependencies {
         implementation 'org.postgresql:postgresql:LATEST_VERSION'
     }
     ```
   - Replace `LATEST_VERSION` with the latest version of the PostgreSQL JDBC driver. You can find this on the [Maven Central Repository](https://search.maven.org/).

2. **Ensure Proper Classpath Configuration:**
   - If you're not using a build tool like Maven or Gradle, ensure that the PostgreSQL JDBC driver JAR is included in your application's classpath.

3. **Check Your Code Configuration:**
   - Verify that your code correctly specifies the PostgreSQL driver. For HikariCP, the configuration typically looks something like this:

     ```java
     HikariConfig config = new HikariConfig();
     config.setJdbcUrl("jdbc:postgresql://localhost:5432/yourdatabase");
     config.setUsername("username");
     config.setPassword("password");
     config.setDriverClassName("org.postgresql.Driver");
     ```

4. **Check for Conflicts:**
   - If you have multiple versions of the JDBC driver, or if there are other conflicting libraries in your classpath, it might cause issues. Make sure there's only one version of the PostgreSQL driver.

5. **Rebuild and Restart:**
   - After making changes, rebuild your project and restart your application to ensure the new configuration is loaded.

By following these steps, you should be able to resolve the error and successfully connect to your PostgreSQL database using HikariCP.

