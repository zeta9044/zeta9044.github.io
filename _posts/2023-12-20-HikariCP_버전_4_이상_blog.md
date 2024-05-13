---
layout: post
title: "HikariCP 버전 4 이상"
date: 2023-12-20
tags: [이상, HikariCP, 버전]
categories: blog
---

## 사용자 입력:
> springboot의 hikaricp 버전이 4 이상인 버전은?

## ChatGPT 응답:
> search("Spring Boot HikariCP version 4 or higher")

## ChatGPT 응답:
> mclick([2, 3, 8])

## ChatGPT 응답:
> Spring Boot versions 2 and 3 come with HikariCP as the default JDBC connection pool, and it is automatically included when you use either `spring-boot-starter-jdbc` or `spring-boot-starter-data-jpa` starter dependencies. You don't need to add an extra dependency for HikariCP in these versions of Spring Boot. However, if you want to use a different version of HikariCP than the one included by default, you can explicitly specify it in your project dependencies.

For instance, HikariCP version 5.0.1 can be used by adding the following dependency in your `pom.xml` file:

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>5.0.1</version>
</dependency>
```

This configuration is useful if you're looking to leverage the latest features or fixes in HikariCP that may not be present in the version bundled with Spring Boot.

For Spring Boot versions below 2.0, you need to explicitly include HikariCP in your dependencies. For example, HikariCP version 3.1.0, suitable for Java 8 and Java 9, can be included like this:

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>3.1.0</version>
</dependency>
```

These configurations ensure that you are using a specific version of HikariCP with Spring Boot, whether it's for compatibility reasons or to use specific features of HikariCP【6†source】【7†source】.

## 사용자 입력:
> 현재, spring-boot가 1.5인데, hikariCp는 4.xx를 쓰고 싶으면?

## ChatGPT 응답:
> If you are using Spring Boot 1.5 and wish to use HikariCP version 4.xx, you will need to manually include HikariCP in your project dependencies. This is because Spring Boot 1.5 does not include HikariCP by default. 

Here is how you can do it:

1. **Add the HikariCP Dependency**: You'll need to add HikariCP version 4.xx as a dependency in your `pom.xml` file. This can be done by specifying the desired version in the dependency block. For example:

   ```xml
   <dependency>
       <groupId>com.zaxxer</groupId>
       <artifactId>HikariCP</artifactId>
       <version>4.x.x</version> <!-- Replace with the specific 4.xx version you need -->
   </dependency>
   ```

2. **Configure the DataSource**: In Spring Boot 1.5, the default datasource is not HikariCP. Therefore, after adding the HikariCP dependency, you will need to explicitly configure HikariCP as your DataSource in your `application.properties` or `application.yml` file. For example:

   ```properties
   spring.datasource.type=com.zaxxer.hikari.HikariDataSource
   spring.datasource.url=jdbc:mysql://localhost:3306/yourDatabase
   spring.datasource.username=yourUsername
   spring.datasource.password=yourPassword
   # Other HikariCP settings
   ```

3. **Compatibility Check**: Ensure that the version of HikariCP you're planning to use is compatible with the other frameworks and libraries you're using in your project, especially given that Spring Boot 1.5 is an older version. 

Remember that using a newer version of a library with an older version of Spring Boot might lead to compatibility issues, so it's important to test thoroughly to ensure that everything works as expected. 

If you encounter any compatibility issues, you might have to consider updating your Spring Boot version or adjusting your choice of HikariCP version accordingly.

## 사용자 입력:
> springboot에서 자동적으로 2.5버전을 기동하거든.

## ChatGPT 응답:
> search("Spring Boot 2.5 HikariCP version included")

## ChatGPT 응답:
> mclick([0, 2, 3])

## ChatGPT 응답:
> If you are using Spring Boot 2.5 and want to use HikariCP version 4.xx, you can do so by explicitly adding the HikariCP dependency to your project. Spring Boot 2.5 uses HikariCP as the default JDBC connection pool, but it might not include version 4.xx by default. To use a specific version of HikariCP, you need to add the dependency to your `pom.xml` file.

Here's how you can do it:

1. **Add the HikariCP Dependency**: Include the specific version of HikariCP you want to use in your `pom.xml`. For example, if you want to use version 4.0.3, your dependency would look like this:

   ```xml
   <dependency>
       <groupId>com.zaxxer</groupId>
       <artifactId>HikariCP</artifactId>
       <version>4.0.3</version> <!-- Replace with the specific 4.xx version you need -->
   </dependency>
   ```

2. **Configure DataSource Properties**: In your `application.properties` or `application.yml`, you can configure various HikariCP properties. Some common properties include `spring.datasource.hikari.connection-timeout`, `spring.datasource.hikari.minimum-idle`, `spring.datasource.hikari.maximum-pool-size`, `spring.datasource.hikari.idle-timeout`, and `spring.datasource.hikari.max-lifetime`. These properties allow you to fine-tune the connection pool according to your application's needs.

3. **Test and Verify**: After adding the dependency and configuring the properties, you should test your application to ensure that the desired version of HikariCP is being used and that it is configured correctly.

By following these steps, you can use HikariCP version 4.xx with Spring Boot 2.5, taking advantage of specific features or improvements that might be available in that version of HikariCP.

## 사용자 입력:
> gradle을 쓰고 있는데,  springboot의 의존성에서 hikaricp 2.5를 배제 하고, 4.0.3을 외부에서 선언하는 방법은?

## ChatGPT 응답:
> To use HikariCP version 4.0.3 in a Spring Boot project with Gradle, while excluding the default HikariCP version provided by Spring Boot, you need to modify your `build.gradle` file. Here's how you can do it:

1. **Exclude HikariCP from Spring Boot's Starter Dependencies**: You need to exclude the HikariCP dependency from the Spring Boot starter modules. This is done by specifying the exclusion within the dependencies block. 

2. **Add HikariCP 4.0.3 as an External Dependency**: After excluding the default HikariCP version, you then need to add the specific HikariCP version (4.0.3) you want to use as an external dependency.

Here is an example of how your `build.gradle` might look:

```gradle
dependencies {
    // Exclude HikariCP from Spring Boot Starter JDBC
    implementation('org.springframework.boot:spring-boot-starter-jdbc') {
        exclude group: 'com.zaxxer', module: 'HikariCP'
    }

    // Other Spring Boot dependencies
    implementation 'org.springframework.boot:spring-boot-starter-web'
    // ...

    // Explicitly add HikariCP version 4.0.3
    implementation 'com.zaxxer:HikariCP:4.0.3'
}
```

In this example:

- The `exclude` clause under `spring-boot-starter-jdbc` ensures that the default HikariCP dependency is not included.
- The explicit `implementation 'com.zaxxer:HikariCP:4.0.3'` line adds the desired version of HikariCP to your project.

Make sure to refresh your Gradle project after making these changes to update the project's dependencies. This will ensure that Spring Boot uses HikariCP version 4.0.3 instead of the version it normally includes.

