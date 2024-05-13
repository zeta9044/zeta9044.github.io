---
layout: post
title: "연결된 Spring Boot 웹 앱"
date: 2023-12-21
tags: [웹, Boot, 연결, 앱, Spring]
categories: blog
---

## 사용자 입력:
> springboot로 만들어진 웹어플이 있는데, 아파치웹서버를 통해서 연결되어야해. 

## ChatGPT 응답:
> 아파치 웹 서버를 사용하여 Spring Boot로 만든 웹 애플리케이션에 연결하는 과정은 몇 가지 단계를 포함합니다. 기본적으로, Apache 웹 서버는 리버스 프록시로 작동하여 외부 요청을 Spring Boot 애플리케이션으로 전달합니다. 다음은 이를 설정하는 과정입니다:

1. **Spring Boot 애플리케이션 배포**: 애플리케이션을 서버에 배포하고, 실행합니다. 보통은 JAR 파일로 실행됩니다.

2. **Apache 웹 서버 설치**: 서버에 Apache 웹 서버를 설치합니다.

3. **모듈 활성화**: Apache에서 `mod_proxy` 및 `mod_proxy_http` 모듈을 활성화해야 합니다. 이 모듈들은 리버스 프록시 기능을 가능하게 합니다.

4. **Apache 설정 수정**: Apache의 설정 파일(예: `httpd.conf` 또는 `apache2.conf`)을 수정하여, 리버스 프록시 설정을 추가합니다. 이 설정은 외부에서 들어오는 요청을 Spring Boot 애플리케이션으로 전달하는 데 필요합니다.

   예시 설정:
   ```
   <VirtualHost *:80>
       ProxyPreserveHost On
       ProxyPass / http://localhost:8080/
       ProxyPassReverse / http://localhost:8080/
       ServerName www.yourdomain.com
   </VirtualHost>
   ```
   이 설정은 모든 요청을 `localhost`의 8080 포트로 전달합니다(여기서 Spring Boot 앱이 실행 중임).

5. **서버 재시작**: Apache 서버를 재시작하여 변경 사항을 적용합니다.

6. **보안 설정 고려**: SSL/TLS 설정, 방화벽 규칙, Apache 보안 모듈 등을 고려하여 애플리케이션의 보안을 강화합니다.

7. **테스트**: 설정이 올바르게 작동하는지 웹 브라우저를 통해 테스트합니다.

이 과정은 기본적인 설정을 가이드합니다. 실제 환경에서는 서버의 설정, 보안 요구 사항, 네트워크 구성에 따라 추가적인 조정이 필요할 수 있습니다.

