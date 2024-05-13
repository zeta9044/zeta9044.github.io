---
layout: post
title: "Gradle 저장소 추가하기"
date: 2023-09-20
tags: [추가, 장소, Gradle]
categories: blog
---

## 사용자 입력:
>   <repositories>
    <repository>
      <id>jgit-repository</id>
      <url>https://repo.eclipse.org/content/groups/releases/</url>
    </repository>
  </repositories>이것을 gradle선언으로 변경해줘.

## ChatGPT 응답:
> Gradle에서 Maven 저장소를 추가하려면 `repositories` 블록 내에 `maven` 블록을 사용하면 됩니다. 주어진 XML 선언을 기반으로 Gradle 선언은 다음과 같습니다:

```groovy
repositories {
    maven {
        url 'https://repo.eclipse.org/content/groups/releases/'
    }
}
```

위의 선언을 사용하여 Gradle 프로젝트에 저장소를 추가할 수 있습니다.

