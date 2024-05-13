---
layout: post
title: "Tomcat 9 Manager 사용법"
tags: [용법, Tomcat, Manager]
---

## 사용자 입력:
> tomcat 9 manager 사용법 알려줘

## ChatGPT 응답:
> Apache Tomcat의 Manager는 웹 애플리케이션의 배포, 시작 및 중지, 상태 모니터링 및 기타 작업을 원격으로 수행할 수 있는 웹 기반 도구입니다. 

Tomcat Manager 사용을 시작하려면 다음 단계를 따르세요:

1. **설치 및 설정**:
   - Tomcat을 설치한 후, {% raw %}`conf/tomcat-users.xml`{% endraw %} 파일을 편집하여 Manager 권한을 가진 사용자를 추가해야 합니다.
   {% raw %}```xml
   <tomcat-users>
      ...
      <role rolename="manager-gui"/>
      <user username="admin" password="password123" roles="manager-gui"/>
      ...
   </tomcat-users>
   ```{% endraw %}
   이 예제에서는 {% raw %}`admin`{% endraw %} 사용자에게 Manager 웹 애플리케이션에 접근할 권한을 부여합니다. 

2. **Manager 애플리케이션 시작**:
   - Tomcat 서버를 시작합니다.
   - 웹 브라우저에서 {% raw %}`http://localhost:8080/manager/html`{% endraw %}로 이동합니다.
   - 앞서 설정한 사용자 이름과 비밀번호 ({% raw %}`admin`{% endraw %} 및 {% raw %}`password123`{% endraw %})로 로그인합니다.

3. **기본 작업**:
   - **배포**: 웹 애플리케이션의 WAR 파일을 업로드하거나 서버에 이미 있는 WAR 파일의 경로를 지정하여 애플리케이션을 배포할 수 있습니다.
   - **시작 및 중지**: 배포된 애플리케이션의 실행을 시작하거나 중지할 수 있습니다.
   - **재배포**: 변경된 WAR 파일을 새로 배포할 수 있습니다.
   - **상태 및 정보**: 각 애플리케이션의 상태, 세션 정보 및 기타 통계를 확인할 수 있습니다.
   - **애플리케이션 제거**: 배포된 애플리케이션을 제거할 수 있습니다.

4. **보안 고려 사항**:
   - Manager 애플리케이션은 매우 강력한 도구이므로 공개 서버에서는 사용하지 않는 것이 좋습니다. 
   - 필요한 경우 VPN, 방화벽 또는 다른 보안 메커니즘을 통해 접근을 제한하세요.
   - 사용자 이름과 비밀번호는 강력하게 설정해야 합니다. 

기억해야 할 주요 포인트는 Tomcat Manager는 웹 애플리케이션의 관리를 쉽게 할 수 있도록 도와주지만, 잘못 사용하면 심각한 보안 위협이 될 수 있으므로 주의가 필요합니다.

## 사용자 입력:
> 서버 xml에는 어떻게 설정해야돼?

## ChatGPT 응답:
> Tomcat의 {% raw %}`server.xml`{% endraw %} 파일은 Tomcat 서버의 주요 설정을 담당합니다. {% raw %}`server.xml`{% endraw %} 파일에는 여러 서비스와 커넥터, 엔진 및 호스트 등의 설정이 포함되어 있습니다.

Manager 애플리케이션을 사용하려면 {% raw %}`server.xml`{% endraw %}에 특별한 설정을 추가할 필요는 없습니다. 그러나 Manager 애플리케이션의 성능, 보안 및 접근성을 조정하려면 다음과 같은 일반적인 설정 사항을 고려할 수 있습니다:

1. **커넥터 설정**:
   {% raw %}`server.xml`{% endraw %}에서 {% raw %}`<Connector>`{% endraw %} 태그는 HTTP, HTTPS 및 AJP 커넥터를 정의합니다. 기본 HTTP 커넥터 설정은 다음과 같습니다:
   {% raw %}```xml
   <Connector port="8080" protocol="HTTP/1.1"
              connectionTimeout="20000"
              redirectPort="8443" />
   ```{% endraw %}
   여기서 {% raw %}`port`{% endraw %} 속성은 Tomcat이 리스닝하는 포트를 지정하며, {% raw %}`redirectPort`{% endraw %} 속성은 HTTPS 리다이렉션에 사용되는 포트를 지정합니다.

2. **호스트 설정**:
   {% raw %}`<Host>`{% endraw %} 태그는 가상 호스트 설정을 정의합니다. 기본 호스트 설정은 다음과 같습니다:
   {% raw %}```xml
   <Host name="localhost"  appBase="webapps"
         unpackWARs="true" autoDeploy="true">
   ```{% endraw %}
   여기서 {% raw %}`appBase`{% endraw %} 속성은 웹 애플리케이션을 찾을 디렉토리를 지정하며, {% raw %}`unpackWARs`{% endraw %}와 {% raw %}`autoDeploy`{% endraw %} 속성은 WAR 파일의 자동 배포 및 압축 해제를 제어합니다.

3. **보안 설정**:
   HTTPS를 사용하여 Tomcat에 연결하려면 {% raw %}`<Connector>`{% endraw %} 태그에 SSL/TLS 설정을 추가해야 합니다. 예:
   {% raw %}```xml
   <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
              maxThreads="150" SSLEnabled="true">
       <SSLHostConfig>
           <Certificate certificateKeystoreFile="conf/localhost-rsa.jks"
                        type="RSA" />
       </SSLHostConfig>
   </Connector>
   ```{% endraw %}
   여기서 {% raw %}`certificateKeystoreFile`{% endraw %} 속성은 서버 인증서와 개인 키를 포함하는 keystore 파일의 경로를 지정합니다.

4. **접근 제한**:
   Manager 애플리케이션에 대한 접근을 제한하려면 {% raw %}`webapps/manager/META-INF/context.xml`{% endraw %} 파일을 편집하여 {% raw %}`<Valve>`{% endraw %} 요소를 추가하거나 수정할 수 있습니다. 예를 들어, 특정 IP 주소에서만 액세스를 허용하려면 다음과 같이 설정할 수 있습니다:
   {% raw %}```xml
   <Valve className="org.apache.catalina.valves.RemoteAddrValve"
          allow="127\.0\.0\.1|::1|192\.168\.0\..*" />
   ```{% endraw %}

이러한 설정은 Tomcat의 기본 동작과 관련이 있으며, Manager 애플리케이션의 동작에 직접적인 영향을 주지는 않습니다. 그러나 이러한 설정은 Manager 애플리케이션의 접근성과 보안에 영향을 줄 수 있습니다.

