---
layout: post
title: "Gradle 업데이트 지침."
date: 2024-02-08
tags: [지침, 업데이트, Gradle]
categories: blog
---

## 사용자 입력:
> buildscript {
	repositories {
		// mavenLocal()
		mavenCentral()
		jcenter()
		// maven { url 'https://oss.sonatype.org/content/repositories/snapshots/' }
	}

	dependencies {
		classpath 'org.anarres.gradle:gradle-stdproject-plugin:1.0.9'
	}
}

apply plugin: 'org.anarres.stdproject'
stdproject {
	javadocLinkSource = true;
}

	group = "org.anarres"

	apply plugin: 'org.anarres.stdmodule'
	stdmodule {
		description "An embeddable C Preprocessor for the JVM."
		author id: 'shevek', name: 'Shevek', email: 'github@anarres.org'
		license 'Apache-2.0'
	}

	sourceCompatibility = 1.5

dependencies {
	compile 'com.google.code.findbugs:annotations:3.0.1'
	compile 'org.slf4j:slf4j-api:1.7.12'

	compile 'net.sf.jopt-simple:jopt-simple:4.7'
	compile 'org.apache.ant:ant:1.7.0'
	compile 'com.github.zafarkhaja:java-semver:0.8.0'

	testCompile 'com.google.guava:guava:18.0'
}

// This ensures that the info-plugin's properties file is in the
// same location for the test suite as in the JAR.
task('processTestVersionResources', type: Copy, dependsOn: processTestResources) {
	into project.sourceSets.test.output.resourcesDir
	from(writeManifestProperties) {
		into "META-INF"
	}
}
testClasses.dependsOn(processTestVersionResources)

apply plugin: 'application'

mainClassName = "org.anarres.cpp.Main"
이 빌드gradle 내용을 gradle 6.9.1 문법에 맞게 바꿔줘.

## ChatGPT 응답:
> Gradle의 문법은 시간이 지남에 따라 변화하여, 특정 버전에서는 사용되던 구문이 다른 버전에서는 더이상 사용되지 않거나, 변경된 방식으로 사용되게 됩니다. Gradle 6.9.1에 맞춰 기존의 `build.gradle` 스크립트를 수정하기 위해서는 다음과 같은 주요 변경 사항을 적용해야 합니다:

1. **repositories 및 dependencies 정의**:
   - Gradle 6.x에서는 여전히 `repositories` 및 `dependencies` 블록이 사용되므로, 이 부분은 크게 변경할 필요가 없습니다. 다만, `jcenter()`는 사용 중단되었으니, 필요하지 않으면 제거하는 것이 좋습니다.
   
2. **Plugin 적용 방식**:
   - Plugin 적용 방식이 간소화되었습니다. `apply plugin: 'plugin-name'` 형식 대신 plugins DSL을 사용하는 것이 권장됩니다.

3. **Java 버전 호환성**:
   - `sourceCompatibility`를 설정하는 방식은 그대로 유지되므로 변경할 필요가 없습니다.

4. **Dependency 구성**:
   - `compile`과 `testCompile` 구성이 `implementation`과 `testImplementation`으로 대체되었습니다.

5. **Task 정의**:
   - Task 정의 방식은 크게 변하지 않았으나, 새로운 API를 사용하여 더 명확하게 정의할 수 있습니다.

아래는 위의 지침을 반영하여 수정한 `build.gradle` 스크립트의 예시입니다:

```groovy
plugins {
    id 'org.anarres.stdproject' version '1.0.9' // 버전 정보는 확인 필요
    id 'org.anarres.stdmodule' version '1.0.9' // 버전 정보는 확인 필요
    id 'application'
}

group = "org.anarres"
mainClassName = "org.anarres.cpp.Main"

repositories {
    mavenCentral()
    // jcenter() // 사용 중단됨
}

dependencies {
    implementation 'com.google.code.findbugs:annotations:3.0.1'
    implementation 'org.slf4j:slf4j-api:1.7.12'
    implementation 'net.sf.jopt-simple:jopt-simple:4.7'
    implementation 'org.apache.ant:ant:1.7.0'
    implementation 'com.github.zafarkhaja:java-semver:0.8.0'

    testImplementation 'com.google.guava:guava:18.0'
}

stdproject {
    javadocLinkSource = true;
}

stdmodule {
    description "An embeddable C Preprocessor for the JVM."
    author id: 'shevek', name: 'Shevek', email: 'github@anarres.org'
    license 'Apache-2.0'
}

sourceCompatibility = 1.5

task processTestVersionResources(type: Copy, dependsOn: processTestResources) {
    into project.sourceSets.test.output.resourcesDir
    from(project.tasks.named("writeManifestProperties")) {
        into "META-INF"
    }
}
tasks.named("testClasses").configure { dependsOn(processTestVersionResources) }
```

이 스크립트는 Gradle 6.9.1 문법을 준수합니다. 그러나, 실제 적용하기 전에 `org.anarres.stdproject`와 `org.anarres.stdmodule` 플러그인의 버전 정보와 호환성을 확인하고, 필요한 경우 적절한 버전으로 업데이트하십시오. 또한, 사용하는 플러그인과 라이브러리의 최신 버전이 Gradle 6.9.1과 호환되는지 확인해야 합니다.

