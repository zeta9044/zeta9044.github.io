---
layout: post
title: "Gradle .gitignore 파일"
date: 2023-07-05
tags: [gitignore, 파일, Gradle]
categories: blog
---

## 사용자 입력:
> gradle에서 쓰는 java용 .gitignore만들어줘.

## ChatGPT 응답:
> 아래는 Gradle 프로젝트를 위한 Java용 .gitignore 파일입니다.

```
# Gradle specific
.gradle/
build/

# IDE specific
.idea/
*.iml
*.ipr
*.iws
out/

# Compiled class file
*.class

# Log files
*.log

# Maven specific
target/

# Eclipse specific
bin/
.settings/
.metadata/

# NetBeans specific
nbproject/private/
build/
nbbuild/
dist/
nbdist/
.nb-gradle/

# IntelliJ specific
out/
.idea_modules/
*.iml
*.ipr
*.iws
.idea/

# Mac OS specific
.DS_Store

# Windows specific
Thumbs.db

# Eclipse
.classpath
.project
.cache-main
.project
.settings/
.factorypath
.gradle/
build.gradle
gradlew
gradlew.bat
```

이 .gitignore 파일은 Gradle 프로젝트를 사용할 때 생성되는 빌드, IDE 설정 및 컴파일된 클래스 파일 등을 Git 저장소에서 제외합니다. 

