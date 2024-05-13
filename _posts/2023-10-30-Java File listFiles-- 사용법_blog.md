---
layout: post
title: "Java File listFiles() 사용법"
date: 2023-10-30
tags: [listFiles, File, Java, 용법]
categories: blog
---

## 사용자 입력:
> java8, java.io.File의 listFiles()에 대해서 알려줘.

## ChatGPT 응답:
> 물론이죠! `java.io.File` 클래스의 `listFiles()` 메서드는 주어진 디렉토리 내의 파일과 하위 디렉토리 목록을 반환합니다. 이 메서드는 주로 파일 탐색이나 디렉토리의 콘텐츠를 나열할 때 사용됩니다.

### 사용법:

1. **기본적인 사용법**: 디렉토리 내의 모든 파일과 디렉토리를 `File` 객체 배열로 반환합니다.

   ```java
   File directory = new File("path_to_directory");
   File[] files = directory.listFiles();
   for (File file : files) {
       System.out.println(file.getName());
   }
   ```

2. **필터 사용**: `FilenameFilter`나 `FileFilter` 인터페이스를 구현하여 특정 조건에 맞는 파일만 나열할 수 있습니다.

   - `FilenameFilter` 사용 예:

     ```java
     File directory = new File("path_to_directory");
     File[] txtFiles = directory.listFiles((dir, name) -> name.endsWith(".txt"));
     for (File file : txtFiles) {
         System.out.println(file.getName());
     }
     ```

   - `FileFilter` 사용 예:

     ```java
     File directory = new File("path_to_directory");
     File[] directoriesOnly = directory.listFiles(file -> file.isDirectory());
     for (File dir : directoriesOnly) {
         System.out.println(dir.getName());
     }
     ```

### 주의사항:

- `listFiles()`는 디렉토리에 대해서만 호출될 수 있습니다. 파일에 대해 호출하면 `null`을 반환합니다.
- 디렉토리를 읽는 데 실패하거나 I/O 오류가 발생하면 `null`을 반환합니다. 그러므로 항상 반환 값이 `null`인지 아닌지 검사하는 것이 좋습니다.

### Java 8+에서의 추가적인 팁:

Java 8 이후의 버전에서는 Stream API와 람다 표현식을 활용하여 `listFiles()`와 조합하여 더욱 강력하고 유연한 파일 처리를 수행할 수 있습니다. 예를 들어, 특정 확장자를 가진 파일만 필터링하거나 정렬하는 등의 작업을 스트림 연산을 통해 쉽게 수행할 수 있습니다.

