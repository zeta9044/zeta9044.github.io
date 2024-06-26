---
layout: post
title: "Java Compiler Version Check"
tags: [Compiler, Version, Check, Java]
---

## 사용자 입력:
> java compile버전 확인

## ChatGPT 응답:
> Java 컴파일러 버전을 확인하려면 다음 명령어를 사용할 수 있습니다. 명령 프롬프트 또는 터미널에서 다음과 같이 입력하십시오:

{% raw %}```
javac -version
```{% endraw %}

이 명령을 실행하면 현재 설치된 Java 컴파일러의 버전 정보가 표시됩니다. 예를 들어, "javac 1.8.0_181"과 같은 출력이 표시될 것입니다. 이 출력은 설치된 Java 개발 키트 (JDK) 버전을 나타냅니다.

참고로, Java 컴파일러의 버전은 JDK의 일부로 제공되며, Java 개발 환경을 설치할 때 자동으로 함께 설치됩니다. 따라서 JDK를 업그레이드하면 컴파일러 버전도 업그레이드됩니다.

## 사용자 입력:
> 클래스가 무슨 버전 인지 알아보는 법

## ChatGPT 응답:
> Java 클래스 파일의 버전 정보를 확인하려면 {% raw %}`javap`{% endraw %} 도구를 사용할 수 있습니다. {% raw %}`javap`{% endraw %} 도구는 Java 클래스 파일의 바이트 코드를 분석하여 클래스의 세부 정보를 표시하는 데 사용됩니다. 클래스 파일의 버전 정보도 이 출력에 포함되어 있습니다.

다음은 클래스 파일 버전을 확인하는 방법입니다:

1. 명령 프롬프트 또는 터미널을 엽니다.

2. 다음 명령을 사용하여 클래스 파일의 내용을 확인합니다:

   {% raw %}```
   javap -verbose YourClassName.class
   ```{% endraw %}

   여기서 {% raw %}`YourClassName`{% endraw %}은 확인하려는 클래스 파일의 이름입니다. 클래스 파일이 현재 작업 디렉토리에 있어야 합니다.

3. {% raw %}`javap`{% endraw %}는 클래스 파일의 세부 정보를 표시합니다. 클래스 파일의 버전 정보는 출력 중에 "major version" 및 "minor version"과 같은 라인에서 나타납니다. 예를 들어:

   {% raw %}```
   ...
   minor version: 0
   major version: 55
   ...
   ```{% endraw %}

   위의 예에서 "major version"은 주 버전을 나타내고, "minor version"은 부 버전을 나타냅니다. 이를 통해 클래스 파일의 Java 버전을 파악할 수 있습니다. 이 경우 "major version: 55"는 Java 11 버전을 나타냅니다.

클래스 파일 버전은 Java 버전과 관련이 있으며, 다양한 Java 버전에 따라 다른 숫자로 표현됩니다. 아래는 주요 Java 버전과 해당 클래스 파일 버전의 일부 예시입니다:

- Java 1.1: 클래스 파일 버전 45.3
- Java 1.2: 클래스 파일 버전 46.0
- Java 1.3: 클래스 파일 버전 47.0
- Java 1.4: 클래스 파일 버전 48.0
- Java 5 (Java 1.5): 클래스 파일 버전 49.0
- Java 6 (Java 1.6): 클래스 파일 버전 50.0
- Java 7 (Java 1.7): 클래스 파일 버전 51.0
- Java 8 (Java 1.8): 클래스 파일 버전 52.0
- Java 9 (Java 9): 클래스 파일 버전 53.0
- Java 10 (Java 10): 클래스 파일 버전 54.0
- Java 11 (Java 11): 클래스 파일 버전 55.0
- Java 12 (Java 12): 클래스 파일 버전 56.0
- Java 13 (Java 13): 클래스 파일 버전 57.0
- Java 14 (Java 14): 클래스 파일 버전 58.0
- Java 15 (Java 15): 클래스 파일 버전 59.0
- Java 16 (Java 16): 클래스 파일 버전 60.0
- Java 17 (Java 17): 클래스 파일 버전 61.0

클래스 파일 버전을 확인하여 Java 소스 코드가 어떤 버전으로 컴파일되었는지 파악할 수 있습니다.

