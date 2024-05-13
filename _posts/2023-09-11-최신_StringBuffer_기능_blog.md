---
layout: post
title: "최신 StringBuffer 기능"
date: 2023-09-11
tags: [최신, StringBuffer, 기능]
categories: blog
---

## 사용자 입력:
> java StringBuffer에서 마지막  문자열을 삭제하는 방법.

## ChatGPT 응답:
> Java의 `StringBuffer` 클래스를 사용하여 마지막 문자를 삭제하려면 `deleteCharAt()` 메서드를 사용할 수 있습니다. 이 메서드는 지정된 인덱스의 문자를 삭제합니다. 마지막 문자를 삭제하려면, `StringBuffer`의 길이에서 1을 뺀 값을 인덱스로 사용하면 됩니다.

예제:
```java
public class Main {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer("Hello");
        sb.deleteCharAt(sb.length() - 1);
        System.out.println(sb);  // 출력: Hell
    }
}
```

위의 예제에서는 "Hello" 문자열의 마지막 문자 'o'가 삭제되었기 때문에 "Hell"이 출력됩니다.

