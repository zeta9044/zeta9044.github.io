---
layout: post
title: "Java 8 Regex 추출"
date: 2023-10-24
tags: [Regex, Java, 추출]
categories: blog
---

## 사용자 입력:
> #include "Derived.h"  이 문자열에서 Derived.h만 추출하려면, 즉 "" 안의 값을 추출하는 것. 그리고, #include <iostream> 에서 iostream만 추출하려면, 즉 <>안의 값을 추출하는 것. 이것을 java8의  regex를 이용해서 추출하는 함수 만들어봐.

## ChatGPT 응답:
> Java 8을 사용하여 #include 지시문에서 파일 이름을 추출하는 함수를 만드는 것은 다음과 같습니다. 여기서는 두 가지 경우를 처리하는 함수를 하나로 만들겠습니다:

1. `#include "filename.h"`
2. `#include <filename>`

아래는 해당 함수입니다:

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class IncludeExtractor {

    public static void main(String[] args) {
        String test1 = "#include \"Derived.h\"";
        String test2 = "#include <iostream>";

        System.out.println(extractFilename(test1)); // 출력: Derived.h
        System.out.println(extractFilename(test2)); // 출력: iostream
    }

    /**
     * #include 지시문에서 파일 이름을 추출하는 함수
     * 
     * @param line #include 지시문
     * @return 파일 이름 또는 null
     */
    public static String extractFilename(String line) {
        // "" 안의 값을 추출하는 패턴 또는 <> 안의 값을 추출하는 패턴
        Pattern pattern = Pattern.compile("#include\\s*(?:\"([^\"]+)\"|<([^>]+)>)");
        Matcher matcher = pattern.matcher(line);
        
        if (matcher.find()) {
            if (matcher.group(1) != null) {
                return matcher.group(1);
            } else {
                return matcher.group(2);
            }
        }
        
        return null;
    }
}
```

위의 코드는 두 종류의 `#include` 지시문 모두를 처리합니다. 첫 번째 테스트 문자열에서는 `"Derived.h"`를, 두 번째 테스트 문자열에서는 `iostream`을 출력합니다.

## 사용자 입력:
> 위에 추출한 파일명에서  ext만 추출하는 함수 추가해봐.

## ChatGPT 응답:
> 파일 이름에서 확장자를 추출하는 함수를 만들려면, 파일 이름의 마지막 `.` 뒤에 오는 문자열을 가져오면 됩니다. 

다음은 해당 함수를 추가한 코드입니다:

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class IncludeExtractor {

    public static void main(String[] args) {
        String test1 = "#include \"Derived.h\"";
        String test2 = "#include <iostream>";

        String filename1 = extractFilename(test1);
        String filename2 = extractFilename(test2);

        System.out.println(filename1); // 출력: Derived.h
        System.out.println(extractExtension(filename1)); // 출력: h

        System.out.println(filename2); // 출력: iostream
        System.out.println(extractExtension(filename2)); // 출력: (확장자가 없으므로 null이 출력됩니다.)
    }

    /**
     * #include 지시문에서 파일 이름을 추출하는 함수
     * 
     * @param line #include 지시문
     * @return 파일 이름 또는 null
     */
    public static String extractFilename(String line) {
        Pattern pattern = Pattern.compile("#include\\s*(?:\"([^\"]+)\"|<([^>]+)>)");
        Matcher matcher = pattern.matcher(line);
        
        if (matcher.find()) {
            if (matcher.group(1) != null) {
                return matcher.group(1);
            } else {
                return matcher.group(2);
            }
        }
        
        return null;
    }

    /**
     * 파일 이름에서 확장자를 추출하는 함수
     * 
     * @param filename 파일 이름
     * @return 확장자 또는 null
     */
    public static String extractExtension(String filename) {
        if (filename == null) return null;

        int dotIndex = filename.lastIndexOf(".");
        if (dotIndex == -1) return null; // 확장자가 없는 경우

        return filename.substring(dotIndex + 1);
    }
}
```

위의 코드에서는 `extractExtension` 메서드가 파일 이름의 확장자를 추출합니다. 확장자가 없는 경우 null을 반환합니다.

