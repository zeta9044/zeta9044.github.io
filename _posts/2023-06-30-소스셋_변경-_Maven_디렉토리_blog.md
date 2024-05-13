---
layout: post
title: "소스셋 변경: Maven 디렉토리"
date: 2023-06-30
tags: [소스, Maven, 디렉토리, 변경, 셋]
categories: blog
---

## 사용자 입력:
> sourceSets {
    main {
         java {
            srcDirs = ['src']
         }
         resources {
             srcDirs = ["src"]
             exclude '**/*.java'
             exclude '**/*.jj'
			 exclude '**/*.jjt'
         }
    }

    test {
        java {
            srcDirs = ['test']
        }
    }
}  위 코드는 build.gradle의 일부인데, 소스셋을 maven 기본셋으로 바꿔줘.

## ChatGPT 응답:
> Maven의 기본 디렉터리 구조는 다음과 같습니다:

- `src/main/java` : 프로젝트의 주요 소스 코드를 포함합니다.
- `src/main/resources` : 프로젝트의 리소스 파일을 포함합니다.
- `src/test/java` : 프로젝트의 테스트 코드를 포함합니다.

따라서, Maven의 기본 디렉터리 구조를 적용하려면, build.gradle 파일을 다음과 같이 변경해야 합니다:

```groovy
sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']
        }
        resources {
            srcDirs = ['src/main/resources']
            exclude '**/*.java'
            exclude '**/*.jj'
            exclude '**/*.jjt'
        }
    }

    test {
        java {
            srcDirs = ['src/test/java']
        }
    }
}
```

위의 코드는 Maven의 기본 소스셋 구조를 Gradle에서 사용하는 방법을 보여줍니다. 

이렇게 하면, 주요 소스코드는 `src/main/java`, 리소스는 `src/main/resources`, 테스트 코드는 `src/test/java` 디렉토리에 위치하게 됩니다. 그리고 리소스 디렉토리에서는 `.java`, `.jj`, `.jjt` 파일이 제외되도록 설정되어 있습니다.

