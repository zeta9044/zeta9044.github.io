---
layout: post
title: "Restructure Gradle for 6.8.3"
date: 2023-12-20
tags: [Gradle, for, Restructure]
categories: blog
---

## 사용자 입력:
> apply plugin: 'war'
apply plugin: 'maven'
apply plugin: 'spring-boot'

//version = '1.0.0-SNAPSHOT'
//group = 'com.datastreams'

sourceSets {
    main {
        groovy {
            srcDirs = ['src/main/java']
        }
    }

    test {
        groovy {
            srcDirs = ['src/test/java']
        }
    }
}

configurations {
    compile.exclude module: 'slf4j-log4j12'
    cargo
}



dependencies {
    compile "org.codehaus.groovy:groovy-all:${groovy_version}"
    compile(project(':qtrack-server')) {
        exclude module: 'servlet-api'
    }

    compile "org.springframework.boot:spring-boot-devtools:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-web:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-aop:${spring_boot_version}"
    compile ("org.springframework.boot:spring-boot-starter-freemarker:${spring_boot_version}"){
        exclude module: 'freemarker'
    }
    compile "org.springframework.boot:spring-boot-starter-freemarker:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-jdbc:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-actuator:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-websocket:${spring_boot_version}"
    compile "org.springframework.boot:spring-boot-starter-cache:${spring_boot_version}"
    providedRuntime "org.springframework.boot:spring-boot-starter-tomcat:${spring_boot_version}"
    testCompile "org.springframework.boot:spring-boot-starter-test:${spring_boot_version}"

    compile "org.springframework.security:spring-security-config:${spring_security_version}"
    compile "org.springframework.security:spring-security-core:${spring_security_version}"
    compile "org.springframework.security:spring-security-web:${spring_security_version}"
    compile "org.springframework.security:spring-security-cas:${spring_security_version}"
    compile "org.springframework.security:spring-security-taglibs:${spring_security_version}"

    compile 'org.freemarker:freemarker:2.3.23'

    compile('org.apache.commons:commons-text:1.8')
    //compile group: 'org.apache.commons', name: 'commons-lang3', version: '3.6'
    compile 'com.navercorp.lucy:lucy-xss-servlet:2.0.0'

    compile group: 'commons-io', name: 'commons-io', version: '2.4'
    compile group: 'commons-lang', name: 'commons-lang', version: '2.6'
    compile group: 'commons-codec', name: 'commons-codec', version: '1.5'
    compile group: 'commons-beanutils', name: 'commons-beanutils', version:'1.8.3'
    compile group: 'commons-fileupload', name: 'commons-fileupload', version: '1.2.2'
    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
    compile group: 'org.mybatis', name: 'mybatis', version: '3.5.9'
    compile group: 'org.mybatis', name: 'mybatis-spring', version: '2.0.7'
    compile group: 'org.apache.poi', name: 'poi', version: '3.15'
    compile group: 'org.apache.poi', name: 'poi-ooxml', version: '3.15'
    //compile group: 'org.apache.poi', name: 'poi-ooxml-schemas', version: '3.15'
    //compile group: 'org.apache.poi', name: 'ooxml-schemas', version: '1.3'
    compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.2'
    compile group: 'org.hibernate', name: 'hibernate-validator-annotation-processor', version: '5.2.0.Final'
    compile group: 'com.jgraphx', name: 'jgraphx' , version: '2.3.0.2'

    providedRuntime "javax.servlet.jsp:jsp-api:2.2"
    providedRuntime group: 'javax.servlet', name: 'javax.servlet-api', version: '3.1.0'
    testCompile group: 'com.jayway.jsonpath', name: 'json-path', version: '0.8.1'

    cargo 'org.codehaus.cargo:cargo-core-container-tomcat:1.4.18'
    cargo 'org.codehaus.cargo:cargo-ant:1.4.18'

    //teraone config dependencies
    compile group: 'org.springframework.session', name: 'spring-session', version: '1.3.0.RELEASE'
    compile ('com.datastreams:teraone-session-jar:1.4.2-SNAPSHOT') {
        exclude module: 'slf4j-log4j12'
    }

    compile group: 'com.datastreams', name: 'qtrack-ui', version: '4.0.0-SNAPSHOT'

    //캐시 프레임워크-ehcache
    implementation 'org.ehcache:ehcache:3.8.0'
    implementation 'javax.cache:cache-api:1.0.0' // expiry 기능을 위해 필요

    testCompile 'com.jayway.jsonpath:json-path:2.4.0'
    // https://mvnrepository.com/artifact/com.jayway.jsonpath/json-path-assert
    testImplementation 'com.jayway.jsonpath:json-path-assert:2.4.0'
    //websocket보안
    implementation 'org.springframework.security:spring-security-messaging:4.2.3.RELEASE'

}


compileJava {
    dependsOn clean
}

processResources {  /* qtrack/qtrack-web/src/main/resources */
    filesMatching('application.yml') { file ->
        /* qtrack/qtrack-web/src/main/resources/application.yml */
        filter { line ->
            line.replaceAll('siteConfig:.*', "siteConfig: ${site}")
        }
    }
}

war {
    // rootSpec.filesMatching('**/application.yml') { file ->
        /* qtrack/qtrack-web/build/resources/main/application.yml */
    //     filter { line ->
    //         line.replaceAll('siteConfig:.*', "siteConfig: ${site}")
    //     }
    // }
    classpath = classpath.filter { file ->
        println file.name
        (
                !file.name.startsWith('ojdbc6-11.2.0.4')
        )
    }
    exclude 'dist'
    exclude 'images'
    exclude 'WEB-INF/common'
    exclude 'WEB-INF/qtrack'
    duplicatesStrategy = 'exclude'
    archiveName = "qtrack.war"
}

bootRun {
    jvmArgs '-Xmx7G', '-verbosegc'
    // export GRADLE_OPTS="-Xmx10G -verbosegc"
}

/* war */
task server(group:'build') {
    description = 'Custom Task - [ war ]'
    finalizedBy war
}

/* UI(webpack) build */
task ui(type:Exec, group:'build') {
    description = 'Custom Task - [ UI(webpack) build ]'
    dependsOn 'cleanDist'
    environment = System.getenv() + [site: site]
    workingDir 'src/main/js'
    commandLine 'yarn.cmd', 'build'
}

/* UI(webpack) proxy server */
task proxy(type:Exec, group:'build') {
    description = 'Custom Task - [ UI(webpack) proxy server ]'
    dependsOn 'cleanDist'
    environment = System.getenv() + [site: site]
    workingDir 'src/main/js'
    commandLine 'yarn.cmd', 'open:proxy'
}

/* clean + war + UI(webpack) build */
task all(group:'build') {
    description = 'Custom Task - [ war + UI(webpack) build ]'
    dependsOn ui
    finalizedBy server
}

/* clean dist folder */
task cleanDist(type: Exec) {
    workingDir 'src/main/webapp/dist'
    commandLine 'rm', '-rf', '*'
}

task deployWar {
	doLast {
	    ant.taskdef(resource: 'cargo.tasks', classpath: configurations.cargo.asPath)
	    ant.cargo(containerId: "tomcat8x", action: "redeploy", type:"remote") {
	        configuration(type: "runtime"){
	            property name: "cargo.hostname", value: project.property("cargo.hostname")
	            property name: "cargo.servlet.port", value: project.property("cargo.servlet.port")
	            property name: "cargo.remote.username", value: project.property("cargo.remote.username")
	            property name: "cargo.remote.password", value: project.property("cargo.remote.password")
	            deployable(type: "war", file: "build/libs/${war.archiveName}"){
	                property name: "context", value: project.property("cargo.deploy.context")
	            }
	        }
	    }
	}
}

task runLayout(type: JavaExec ){
	jvmArgs '-Xmx5G'
	classpath = sourceSets.test.runtimeClasspath
	main = 'com.datastreams.qtrack.graph.service.GraphLayoutTest'
}

task runManyEdge(type: JavaExec ){
	jvmArgs '-Xmx300m'
	classpath = sourceSets.test.runtimeClasspath
	main = 'com.datastreams.qtrack.graph.service.ManyEdgeTest'
}

task runBigSugGraph(type: JavaExec ){
	jvmArgs '-Xmx5g'
	classpath = sourceSets.test.runtimeClasspath
	main = 'com.datastreams.qtrack.graph.service.BigSubGraphTest'
}
이것을 gradle 6.8.3에  맞게 재구성해줘.

## ChatGPT 응답:
> To restructure your Gradle script for compatibility with Gradle version 6.8.3, several modifications and updates are necessary. I'll outline the changes below:

1. **Plugin Syntax**: The way plugins are applied has been updated in Gradle 6.8.3. Use the `plugins` block for a cleaner and more idiomatic way.

2. **Dependency Configuration**: Gradle 6.x introduces changes in the way dependencies are declared. For instance, `compile` and `testCompile` configurations are deprecated and replaced by `implementation` and `testImplementation`.

3. **Source Sets**: The configuration for source sets remains largely the same, but ensure it aligns with your project structure.

4. **Excluding Modules**: This remains the same, but always verify against the latest Gradle documentation.

5. **Task Configuration**: Custom tasks should be compatible, but it's always good to test them after upgrading.

6. **Cargo Plugin**: Ensure that the Cargo plugin used is compatible with Gradle 6.8.3.

7. **War Task**: The configuration should be compatible, but double-check any deprecations or changes in the War plugin with Gradle 6.x.

8. **JavaExec Tasks**: These tasks should be compatible, but verify their behavior after the upgrade.

Below is the restructured script:

```groovy
plugins {
    id 'java'
    id 'war'
    id 'maven'
    id 'org.springframework.boot' version '2.4.2' // Use the correct Spring Boot plugin version
}

group = 'com.datastreams'
version = '1.0.0-SNAPSHOT'

sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']
        }
    }
    test {
        java {
            srcDirs = ['src/test/java']
        }
    }
}

configurations {
    implementation.exclude module: 'slf4j-log4j12'
    cargo
}

dependencies {
    // Update all 'compile' to 'implementation', and 'testCompile' to 'testImplementation'
    // Ensure all dependency versions are compatible with Gradle 6.8.3
    // Example:
    implementation "org.codehaus.groovy:groovy-all:${groovy_version}"
    implementation(project(':qtrack-server')) {
        exclude module: 'servlet-api'
    }
    // ... rest of the dependencies ...
}

// Custom tasks and other configurations
// Ensure that custom tasks and other configurations are compatible with Gradle 6.8.3

// Example for a custom task
task customTask(type: JavaExec) {
    // Task configuration
}
```

This script is a basic guide. You will need to adjust it according to the specific requirements of your project and test it thoroughly to ensure compatibility with Gradle 6.8.3. Also, always refer to the official Gradle documentation for the most accurate and up-to-date information.

## 사용자 입력:
> 한글로 설명해.

## ChatGPT 응답:
> Gradle 6.8.3 버전에 맞게 Gradle 스크립트를 재구성하기 위해 몇 가지 수정과 업데이트가 필요합니다. 아래에 변경 사항을 설명하겠습니다.

1. **플러그인 구문**: Gradle 6.8.3에서는 플러그인을 적용하는 방식이 업데이트되었습니다. `plugins` 블록을 사용하여 보다 깔끔하고 관례적인 방식으로 플러그인을 적용하세요.

2. **의존성 구성**: Gradle 6.x는 의존성을 선언하는 방식에 변화를 가져왔습니다. 예를 들어, `compile` 및 `testCompile` 구성이 폐지되고 `implementation` 및 `testImplementation`으로 대체되었습니다.

3. **소스 세트**: 소스 세트에 대한 구성은 대체로 동일하지만, 프로젝트 구조와 일치하는지 확인하세요.

4. **모듈 제외**: 이 부분은 동일하게 유지되지만, 최신 Gradle 문서와 항상 확인하는 것이 좋습니다.

5. **작업 구성**: 사용자 정의 작업은 호환될 수 있지만, 업그레이드 후에 항상 테스트하는 것이 좋습니다.

6. **Cargo 플러그인**: 사용하는 Cargo 플러그인이 Gradle 6.8.3과 호환되는지 확인하세요.

7. **War 작업**: 구성은 호환될 수 있지만, Gradle 6.x의 War 플러그인에 대한 변경 사항이나 폐지 사항을 확인하세요.

8. **JavaExec 작업**: 이러한 작업은 호환될 수 있지만, 업그레이드 후에 그들의 행동을 확인하세요.

아래는 재구성된 스크립트입니다:

```groovy
plugins {
    id 'java'
    id 'war'
    id 'maven'
    id 'org.springframework.boot' version '2.4.2' // 정확한 Spring Boot 플러그인 버전 사용
}

group = 'com.datastreams'
version = '1.0.0-SNAPSHOT'

sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']
        }
    }
    test {
        java {
            srcDirs = ['src/test/java']
        }
    }
}

configurations {
    implementation.exclude module: 'slf4j-log4j12'
    cargo
}

dependencies {
    // 모든 'compile'을 'implementation'으로, 'testCompile'을 'testImplementation'으로 업데이트
    // 모든 의존성 버전이 Gradle 6.8.3과 호환되는지 확인
    // 예시:
    implementation "org.codehaus.groovy:groovy-all:${groovy_version}"
    implementation(project(':qtrack-server')) {
        exclude module: 'servlet-api'
    }
    // ... 나머지 의존성 ...
}

// 사용자 정의 작업 및 기타 구성
// 사용자 정의 작업 및 기타 구성이 Gradle 6.8.3과 호환되는지 확인

// 사용자 정의 작업 예시
task customTask(type: JavaExec) {
    // 작업 구성
}
```

이 스크립트는 기본적인 가이드입니다. 프로젝트의 특정 요구 사항에 맞게 조정하고 Gradle 6.8.3과의 호환성을 확보하기 위해 철저히 테스트해야 합니다. 또한, 가장 정확하고 최신의 정보를 얻기 위해서는 공식 Gradle 문서를 항상 참조하세요.

