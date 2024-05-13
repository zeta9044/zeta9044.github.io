---
layout: post
title: "HikariCP 설정 오류 해결"
tags: [해결, HikariCP, 설정, 오류]
---

## 사용자 입력:
> org.quartz.dataSource.myDS.provider =hikaricp 이 설정이 jdk8에서 안돼나?

## ChatGPT 응답:
> {% raw %}`org.quartz.dataSource.myDS.provider=hikaricp`{% endraw %} 설정은 Quartz 스케줄러의 설정 중 하나로, 데이터 소스로 HikariCP 커넥션 풀을 사용하겠다는 의미입니다. HikariCP는 높은 성능의 JDBC 커넥션 풀 라이브러리로, JDK 8과 호환됩니다.

JDK 8에서 해당 설정이 작동하지 않는다면, 몇 가지 가능한 이유가 있습니다:

1. **의존성 문제:** {% raw %}`hikaricp`{% endraw %} 라이브러리가 프로젝트의 클래스패스에 포함되어 있지 않을 수 있습니다. Maven이나 Gradle 같은 빌드 도구를 사용하는 경우, {% raw %}`hikaricp`{% endraw %}에 대한 의존성을 {% raw %}`pom.xml`{% endraw %} 또는 {% raw %}`build.gradle`{% endraw %} 파일에 추가해야 합니다.

2. **설정 오류:** Quartz 설정 파일({% raw %}`quartz.properties`{% endraw %})에 문제가 있을 수 있습니다. {% raw %}`provider=hikaricp`{% endraw %} 설정 외에도, HikariCP를 사용하기 위한 다른 필요한 속성들(예: URL, 사용자 이름, 비밀번호 등)이 제대로 설정되어 있는지 확인해야 합니다.

3. **버전 충돌:** 사용 중인 Quartz 또는 HikariCP의 버전이 JDK 8과 호환되지 않을 수 있습니다. 일반적으로 HikariCP와 Quartz는 JDK 8과 호환되지만, 최신 버전의 라이브러리에서는 JDK 버전 요구사항이 변경될 수 있으므로 이를 확인해야 합니다.

4. **실행 환경 문제:** 실행 중인 환경의 문제로 인해 설정이 제대로 적용되지 않을 수 있습니다. 예를 들어, 애플리케이션 서버의 설정이나 시스템 환경 변수 등이 영향을 줄 수 있습니다.

이 문제를 해결하기 위해 다음 단계를 따를 수 있습니다:

- HikariCP 의존성이 프로젝트에 포함되어 있는지 확인합니다.
- {% raw %}`quartz.properties`{% endraw %} 파일이 올바르게 설정되었는지 확인합니다.
- 사용 중인 Quartz와 HikariCP 라이브러리의 버전이 JDK 8과 호환되는지 확인합니다.
- 로그를 확인하여 구체적인 오류 메시지를 찾아 문제를 진단합니다.
- 실행 환경을 점검하여 필요한 시스템 속성이나 환경 변수가 설정되어 있는지 확인합니다.

더 구체적인 도움을 원한다면, 설정 파일과 코드, 그리고 발생하는 오류 메시지의 자세한 내용을 제공해 주시면 좋겠습니다.

