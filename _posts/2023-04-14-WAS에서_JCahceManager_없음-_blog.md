---
layout: post
title: "WAS에서 JCahceManager 없음."
tags: [JCahceManager]
---

## 사용자 입력:
> WAS서버에서 JCahceManger 없다고 나오면,  WAS lib중 무엇이 빠진 걸까?

## ChatGPT 응답:
> JCahceManager는 Ehcache를 사용하여 캐싱을 구현하는 Java 어플리케이션에서 매우 흔하게 사용되는 클래스입니다.

만약 WAS (WebSphere Application Server)에서 JCahceManager가 없다는 오류가 발생한다면, 아마도 WAS의 classpath에 Ehcache 관련 라이브러리가 빠져있을 가능성이 높습니다.

이를 해결하기 위해서는 WAS의 classpath에 Ehcache 관련 라이브러리를 추가하거나, 애플리케이션 내에서 해당 라이브러리를 포함하도록 설정해야 합니다. 이를 위해서는 WAS에서 사용하는 빌드 도구와 라이브러리 관리 도구를 이용하여 필요한 라이브러리를 추가하는 방법을 검토해 볼 수 있습니다.

## 사용자 입력:
> 한국의 제우스 WAS서버 아니?

## ChatGPT 응답:
> 제우스 (Zeus)는 웹 애플리케이션 서버(WAS)로서, 한국에서 개발 및 사용되고 있는 WAS입니다.

만약 제우스 WAS에서 JCahceManager가 없다는 오류가 발생한다면, 이는 제우스 WAS에서 Ehcache 관련 라이브러리가 빠져있을 가능성이 있습니다. 따라서, 제우스 WAS에서도 WAS의 classpath에 Ehcache 관련 라이브러리를 추가하거나, 애플리케이션 내에서 해당 라이브러리를 포함하도록 설정해야 합니다. 이를 위해서는 제우스 WAS에서 사용하는 빌드 도구와 라이브러리 관리 도구를 이용하여 필요한 라이브러리를 추가하는 방법을 검토해 볼 수 있습니다.

## 사용자 입력:
> 제우스 WAS의 classpath 지정하는 방법 아니?

## ChatGPT 응답:
> 제우스 WAS에서 classpath를 지정하는 방법은 다음과 같습니다.

1. 제우스 WAS의 설치 디렉토리로 이동합니다.
2. "conf" 폴더 내의 "setenv.sh" 파일을 엽니다. (Windows에서는 "setenv.bat" 파일입니다.)
3. "setenv.sh" 파일 내의 "CLASSPATH" 환경 변수를 설정합니다. 이때, 필요한 라이브러리 파일들이 포함된 디렉토리를 지정합니다. 예를 들어, 다음과 같이 지정할 수 있습니다.

   {% raw %}```
   export CLASSPATH=/path/to/lib1:/path/to/lib2:/path/to/lib3
   ```{% endraw %}

4. "setenv.sh" 파일을 저장하고 종료합니다.

이후 제우스 WAS를 재시작하면, 지정한 디렉토리에 포함된 라이브러리 파일들이 classpath에 추가되어 사용할 수 있게 됩니다.

## 사용자 입력:
> 위에 내용 톰캣 아니야?

## ChatGPT 응답:
> 저의 이전 답변에서는 톰캣을 기준으로 설명을 드렸었습니다. 죄송합니다. 

제우스 WAS에서 classpath를 지정하는 방법은 다음과 같습니다.

1. 제우스 WAS의 설치 디렉토리로 이동합니다.
2. "bin" 폴더 내의 "start_zeus.sh" 파일을 엽니다. (Windows에서는 "start_zeus.bat" 파일입니다.)
3. "start_zeus.sh" 파일 내의 "CLASSPATH" 환경 변수를 설정합니다. 이때, 필요한 라이브러리 파일들이 포함된 디렉토리를 지정합니다. 예를 들어, 다음과 같이 지정할 수 있습니다.

   {% raw %}```
   CLASSPATH=/path/to/lib1:/path/to/lib2:/path/to/lib3
   export CLASSPATH
   ```{% endraw %}

4. "start_zeus.sh" 파일을 저장하고 종료합니다.

이후 제우스 WAS를 재시작하면, 지정한 디렉토리에 포함된 라이브러리 파일들이 classpath에 추가되어 사용할 수 있게 됩니다.

## 사용자 입력:
> Unable to find the property [LoginTimeout] in the driver's class [oracle.jdbc.driver.OracleDriver] for the data source [liadb]. Verify that it is a valid property for this data source. 해결방법?

## ChatGPT 응답:
> "Unable to find the property [LoginTimeout] in the driver's class [oracle.jdbc.driver.OracleDriver] for the data source [liadb]" 오류는 JDBC 드라이버에서 LoginTimeout 속성을 찾을 수 없을 때 발생합니다. 이 오류를 해결하기 위해서는 다음과 같은 단계를 수행할 수 있습니다.

1. WAS 관리 콘솔에 로그인합니다.
2. "자원" > "JDBC" > "JDBC 프로바이더"로 이동합니다.
3. JDBC 프로바이더 이름을 클릭하여 "JDBC 드라이버" 탭으로 이동합니다.
4. "기본 JDBC 드라이버 구성" 섹션에서 "사용자 정의 속성"을 클릭합니다.
5. "LoginTimeout" 속성을 추가하고 값을 지정합니다. 예를 들어, 30으로 설정하면 30초 동안 로그인을 시도합니다.
6. "적용" 버튼을 클릭하여 변경 사항을 저장합니다.

이제 WAS에서 데이터 소스를 다시 시작하고, 문제가 해결되었는지 확인해 보시기 바랍니다. 만약 이 방법으로도 문제를 해결할 수 없다면, Oracle JDBC 드라이버의 버전을 확인하고, 해당 버전의 매뉴얼에서 LoginTimeout 속성을 지원하는지 확인해 보시기 바랍니다.

## 사용자 입력:
> ojdbc8.jar 에 LoginTimeout 속성이 없나?

## ChatGPT 응답:
> Oracle JDBC 드라이버인 ojdbc8.jar에서는 LoginTimeout 속성을 지원합니다. 따라서, "oracle.jdbc.driver.OracleDriver" 클래스를 사용하는 WAS에서 해당 속성을 사용할 수 있습니다.

만약 "Unable to find the property [LoginTimeout] in the driver's class [oracle.jdbc.driver.OracleDriver]" 오류가 발생한다면, 다음과 같은 확인 사항들을 검토해 보십시오.

1. JDBC 드라이버가 제대로 로드되었는지 확인합니다. 
2. ojdbc8.jar 파일이 WAS의 classpath에 포함되어 있는지 확인합니다.
3. JDBC 드라이버의 버전이 호환되는 WAS 버전과 일치하는지 확인합니다.
4. JDBC 드라이버의 구성 설정에서 LoginTimeout 속성을 지원하는지 확인합니다.

위의 확인 사항들을 검토하고도 문제가 해결되지 않는다면, WAS와 JDBC 드라이버의 로그를 살펴보며 더 자세한 원인을 파악해 볼 필요가 있습니다.

