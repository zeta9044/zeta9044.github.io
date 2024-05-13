---
layout: post
title: "Data input error."
date: 2023-03-20
tags: [Data, input, error]
categories: blog
---

## 사용자 입력:
> com.datastreams.qtrack.exception.exception.QtrackException: ������ �׸� ���Ͽ� ���������� �����ϹǷ� ������ �� �����ϴ�
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method) ~[na:1.8.0_202]
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62) ~[na:1.8.0_202]
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45) ~[na:1.8.0_202]
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423) ~[na:1.8.0_202]
	at org.codehaus.groovy.reflection.CachedConstructor.invoke(CachedConstructor.java:83) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.ConstructorSite$ConstructorSiteNoUnwrapNoCoerce.callConstructor(ConstructorSite.java:105) ~[groovy-all-2.4.13.jar:2.4.13]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.callConstructor(AbstractCallSite.java:255) ~[groovy-all-2.4.13.jar:2.4.13]
	at com.datastreams.qtrack.preferences.web.QtrackAnalysisJobController.deleteAnalysis(QtrackAnalysisJobController.groovy:113) ~[main/:na]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_202]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_202]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_202]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_202]
	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205) ~[spring-web-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:133) ~[spring-web-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:97) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:854) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:765) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:967) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:901) ~[spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970) [spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:872) [spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:707) [javax.servlet-api-3.1.0.jar:3.1.0]
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846) [spring-webmvc-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:790) [javax.servlet-api-3.1.0.jar:3.1.0]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52) [tomcat-embed-websocket-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.springframework.boot.web.filter.ApplicationContextHeaderFilter.doFilterInternal(ApplicationContextHeaderFilter.java:54) [spring-boot-1.5.22.RELEASE.jar:1.5.22.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at javax.servlet.FilterChain$doFilter.call(Unknown Source) [javax.servlet-api-3.1.0.jar:3.1.0]
	at com.datastreams.qtrack.jwt.web.JwtRequestFilter.doFilterInternal(JwtRequestFilter.groovy:76) [main/:na]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at com.datastreams.qtrack.jwt.web.CustomFilter.doFilter(CustomFilter.java:51) [main/:na]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at javax.servlet.FilterChain$doFilter.call(Unknown Source) [javax.servlet-api-3.1.0.jar:3.1.0]
	at com.datastreams.mdosa.common.servlet.HiddenHttpHeaderMethodFilter.doFilterInternal(HiddenHttpHeaderMethodFilter.groovy:32) [iruda-core-security-3.0.0.alpha6-SNAPSHOT.jar:3.0.0.alpha6-SNAPSHOT-b0f40720]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.3.25.RELEASE.jar:4.3.25.RELEASE]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-8.5.43.jar:8.5.43]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-8.5.43.jar:8.5.43]



## ChatGPT 응답:
> It seems like you are encountering an exception in your Java application. The error message is not in English, but it roughly translates to: "Due to insufficient information in the input data, it is impossible to proceed with the analysis."

The error is a custom exception called `QtrackException`, and it is being thrown in the `deleteAnalysis` method in the `QtrackAnalysisJobController.groovy` file at line 113. 

To debug this issue, I suggest you do the following:

1. Check the input data being passed to the `deleteAnalysis` method to ensure that it contains all the required information. It's possible that some required fields are missing or have incorrect values.
2. Inspect the code in the `deleteAnalysis` method around line 113 to identify any potential issues that might be causing the exception. You can add some logging statements to help with the debugging process.
3. Add proper validation and error handling to your code. This way, when an issue occurs, the error message will be more informative, allowing you to identify the root cause more quickly.
4. If the issue is still unclear, you may want to share more information about the application or the specific data that is causing the problem.

## 사용자 입력:
> spring message를 excetionController로 넘기는데, security 오류가 있다면 ? 중간에 커스텀 코드가 있다.

## ChatGPT 응답:
> If you are using Spring and have a custom exception controller to handle exceptions, and you suspect there might be a security issue, you can follow these steps to diagnose and resolve the problem:

1. **Check the security configuration**: Verify that your Spring Security configuration is set up correctly, allowing access to the necessary endpoints for authenticated and unauthenticated users. Ensure that the custom code you have in place is not inadvertently blocking access to the exception controller.

2. **Inspect custom security code**: Review your custom security code to ensure it is not causing the issue. Look for any potential misconfigurations, incorrect assumptions, or access control issues that could be preventing the proper handling of exceptions.

3. **Enable debug logging**: Enable debug logging for Spring Security to get more insight into what might be causing the issue. In your `application.properties` or `application.yml` file, add the following line:

   ```
   logging.level.org.springframework.security: DEBUG
   ```

   This will provide more detailed log messages related to Spring Security, which can help you identify any potential issues.

4. **Test different scenarios**: Test different scenarios, such as authenticated and unauthenticated requests, to see if the exception controller is working as expected in various cases. This can help you narrow down the problem and identify any issues related to security configuration or custom code.

5. **Handle security-related exceptions**: Make sure that your exception controller is set up to handle security-related exceptions, such as `AccessDeniedException` and `AuthenticationException`. This will ensure that any security issues are properly reported and handled by your application.

By following these steps, you should be able to diagnose and resolve any security issues that might be affecting your exception controller. If you are still having trouble, consider providing more information about your security configuration, custom code, and any error messages or logs that you have encountered.

## 사용자 입력:
> bamboo에서 빌드하기 전에, repository branch를 선택하고 진행하게 만들 수 있나?

## ChatGPT 응답:
> Bamboo, Atlassian's continuous integration and deployment tool, does not have a built-in feature to select a repository branch interactively before starting a build. However, you can achieve this functionality using Bamboo's "branch plans" feature and a bit of manual configuration.

Here's how you can set up Bamboo to build specific branches on demand:

1. **Create a plan**: First, create a plan in Bamboo for your main branch (e.g., `master` or `main`).

2. **Enable branch plans**: Go to the plan configuration and navigate to the "Branches" tab. Enable "Branch plans" by clicking on "Create plan branches for matching branches." You can specify a branch pattern, such as `*`, to match all branches or provide a more specific pattern to match only certain branches.

3. **Manual build trigger**: In the plan configuration, navigate to the "Triggers" tab. Remove any existing triggers (such as "Repository triggers the build when changes are committed") and add a "Manual" trigger. This will ensure that the build only starts when manually triggered by a user.

4. **Start a build for a specific branch**: Now, whenever you want to build a specific branch, go to the plan's main page and click on the "Branches" tab. You will see a list of all branches matching your specified pattern. Click the "Run" button (▶) next to the branch you want to build.

By following these steps, you can manually select and build a specific branch in Bamboo. Keep in mind that this approach requires you to create branch plans for all branches you want to build manually.

## 사용자 입력:
> 한글로 설명해

## ChatGPT 응답:
> Bamboo는 Atlassian의 지속적인 통합 및 배포 도구입니다. Bamboo에는 빌드를 시작하기 전에 저장소 브랜치를 대화식으로 선택하는 기능이 내장되어 있지 않습니다. 그러나 Bamboo의 "브랜치 계획" 기능과 수동 설정을 사용하여 이 기능을 구현할 수 있습니다.

Bamboo를 사용하여 특정 브랜치를 요청에 따라 빌드하려면 다음 단계를 수행하세요.

1. **계획 생성**: 먼저 Bamboo에서 메인 브랜치(예: `master` 또는 `main`)에 대한 계획을 생성합니다.

2. **브랜치 계획 활성화**: 계획 구성으로 이동하여 "브랜치" 탭을 찾아갑니다. "일치하는 브랜치에 대한 계획 브랜치 생성"을 클릭하여 "브랜치 계획"을 활성화합니다. 모든 브랜치와 일치하는 패턴인 `*`을 지정하거나 특정 브랜치만 일치하도록 더 구체적인 패턴을 제공할 수 있습니다.

3. **수동 빌드 트리거**: 계획 구성에서 "트리거" 탭으로 이동합니다. 기존 트리거(예: "변경 사항이 커밋될 때 저장소가 빌드를 트리거함")를 제거하고 "수동" 트리거를 추가합니다. 이렇게 하면 사용자가 수동으로 트리거할 때만 빌드가 시작됩니다.

4. **특정 브랜치를 위한 빌드 시작**: 이제 특정 브랜치를 빌드하려면 계획의 메인 페이지로 이동하여 "브랜치" 탭을 클릭합니다. 지정된 패턴과 일치하는 모든 브랜치의 목록이 표시됩니다. 빌드하려는 브랜치 옆에 있는 "실행" 버튼(▶)을 클릭합니다.

이 단계를 따르면 Bamboo에서 수동으로 특정 브랜치를 선택하고 빌드할 수 있습니다. 이 방법은 수동으로 빌드하려는 모든 브랜치에 대해 브랜치 계획을 생성해야 한다는 점을 명심하세요.

## 사용자 입력:
> bamboo에서 tag로 빌드하려면?

## ChatGPT 응답:
> Bamboo에서 Git 태그를 사용하여 빌드를 설정하려면 다음 단계를 수행하세요:

1. **새 계획 생성**: 먼저 Bamboo에서 새 계획을 생성합니다. 저장소와 빌드 명령을 설정하세요.

2. **태그 기반 트리거 설정**: 계획 구성으로 이동한 다음 "트리거" 탭을 클릭합니다. "트리거 추가"를 클릭하고 "Repository Polling" 또는 "Repository Triggered"를 선택합니다. "Trigger conditions" 섹션에서 "Change detection options"을 클릭하여 드롭다운 메뉴를 엽니다.

3. **태그 패턴 설정**: "태그" 옵션을 선택한 다음 태그 패턴을 입력합니다. 예를 들어, 모든 태그와 일치하도록 설정하려면 `refs/tags/*`를 입력합니다. 특정 태그를 대상으로 설정하려면 해당 태그의 이름을 사용하세요 (예: `refs/tags/v1.0.0`).

4. **저장**: 설정을 저장하고 계획 구성을 닫습니다.

이제 Bamboo가 설정된 패턴에 맞는 태그가 저장소에 추가되면 자동으로 빌드를 시작합니다. 그러나 Bamboo는 태그를 선택하여 수동으로 빌드를 시작하는 기능을 제공하지 않습니다. 대신 다음 방법을 사용하여 수동으로 빌드를 시작할 수 있습니다:

1. 원하는 태그에 해당하는 브랜치를 로컬 Git 저장소에서 체크아웃하세요.
2. 이 브랜치를 원격 저장소에 새 브랜치로 푸시하세요.
3. Bamboo에서 브랜치를 선택하여 수동으로 빌드를 시작합니다.

참고로, 이 방법은 원격 저장소에서 추가적인 브랜치를 생성해야 하며, 빌드가 완료된 후 브랜치를 삭제할 수 있습니다.

## ChatGPT 응답:
> Bamboo에서 태그를 사용하여 빌드를 수행하려면 다음 단계를 따르세요:

1. **태그를 위한 전용 브랜치 계획 생성**: 메인 브랜치를 위한 Bamboo 계획을 생성한 후, 태그에 대한 전용 브랜치 계획을 만들어야 합니다. 계획 구성에서 "브랜치" 탭으로 이동하고 "브랜치 계획"을 활성화합니다. 이 경우, 태그와 일치하는 브랜치 패턴을 지정해야 합니다. 대부분의 경우, 이 패턴은 `refs/tags/*`입니다.

2. **수동 빌드 트리거 설정**: 태그를 사용하여 빌드를 수동으로 시작하려면, 계획 구성에서 "트리거" 탭으로 이동하고 기존 트리거를 제거한 후 "수동" 트리거를 추가합니다.

3. **태그 빌드 시작**: 이제 특정 태그를 사용하여 빌드를 시작할 준비가 되었습니다. 계획의 메인 페이지에서 "브랜치" 탭으로 이동하면 지정된 패턴에 일치하는 태그 목록이 표시됩니다. 빌드할 태그 옆에 있는 "실행" 버튼(▶)을 클릭하세요.

이렇게 하면 Bamboo에서 태그를 사용하여 빌드를 수행할 수 있습니다. 주의할 점은, 이 설정은 수동으로 빌드를 시작하도록 설정되므로 자동 빌드가 수행되지 않는다는 것입니다.

## 사용자 입력:
> bamboo버전 아는 법

## ChatGPT 응답:
> Bamboo의 버전을 확인하는 방법은 웹 인터페이스를 사용하는 것입니다. 다음 단계에 따라 Bamboo 버전을 확인하세요:

1. 웹 브라우저를 사용하여 Bamboo 인스턴스에 로그인합니다.

2. 우측 상단에 있는 프로필 사진 또는 아이콘을 클릭합니다. 이렇게 하면 드롭다운 메뉴가 나타납니다.

3. 드롭다운 메뉴에서 "시스템 정보" 또는 "System Info"를 클릭합니다.

4. 시스템 정보 페이지로 이동하면, "Bamboo Information" 또는 "Bamboo 정보" 섹션을 찾으십시오. 여기에서 "Bamboo Version" 또는 "Bamboo 버전"을 확인할 수 있습니다.

Bamboo 정보 섹션에는 버전 외에도 다양한 시스템 정보가 포함되어 있으므로, 추가적인 정보가 필요한 경우 이 페이지를 참조하십시오.

## 사용자 입력:
> bamboo 6.7.1에서 tag를 이용해서 빌드하는 방법

## ChatGPT 응답:
> Bamboo 6.7.1에서 태그를 사용하여 빌드를 수행하려면 다음 단계를 따르세요:

1. **태그를 위한 전용 브랜치 계획 생성**: 메인 브랜치를 위한 Bamboo 계획을 생성한 후, 태그에 대한 전용 브랜치 계획을 만들어야 합니다. 계획 구성에서 "브랜치" 탭으로 이동하고 "브랜치 계획"을 활성화합니다. 이 경우, 태그와 일치하는 브랜치 패턴을 지정해야 합니다. 대부분의 경우, 이 패턴은 `refs/tags/*`입니다.

2. **수동 빌드 트리거 설정**: 태그를 사용하여 빌드를 수동으로 시작하려면, 계획 구성에서 "트리거" 탭으로 이동하고 기존 트리거를 제거한 후 "수동" 트리거를 추가합니다.

3. **태그 빌드 시작**: 이제 특정 태그를 사용하여 빌드를 시작할 준비가 되었습니다. 계획의 메인 페이지에서 "브랜치" 탭으로 이동하면 지정된 패턴에 일치하는 태그 목록이 표시됩니다. 빌드할 태그 옆에 있는 "실행" 버튼(▶)을 클릭하세요.

이렇게 하면 Bamboo 6.7.1에서 태그를 사용하여 빌드를 수행할 수 있습니다. 주의할 점은, 이 설정은 수동으로 빌드를 시작하도록 설정되므로 자동 빌드가 수행되지 않는다는 것입니다.

참고로 Bamboo 6.7.1에서의 사용자 인터페이스나 단계가 최신 버전의 Bamboo와 약간 다를 수 있습니다. 위의 단계를 따르되, 사용자 인터페이스의 차이를 고려하십시오.

## 사용자 입력:
> 태그와 일치하는 브랜치 패턴을 지정하는 부분이 없는데.

## ChatGPT 응답:
> 죄송합니다. 제가 앞서 제공한 방법은 정확한 정보가 아닙니다. Bamboo 6.7.1에서는 직접 태그를 사용하여 빌드하는 기능이 없습니다. 그러나 아래와 같은 간접적인 방법을 사용하여 Bamboo에서 태그를 사용하여 빌드를 수행할 수 있습니다.

1. **스크립트 작성**: 먼저, 리포지토리의 최신 태그를 가져와 해당 태그에 대한 빌드를 수행하는 쉘 스크립트 또는 배치 파일을 작성합니다. 이 스크립트는 Bamboo에서 실행할 때 필요한 빌드 도구를 사용하여 빌드 작업을 수행해야 합니다.

2. **Bamboo 계획 생성**: Bamboo에서 새 계획을 생성하고 소스 코드 저장소를 설정합니다. 저장소 설정을 완료한 후, 해당 계획에 스크립트 작업을 추가합니다.

3. **스크립트 작업 구성**: 작성한 스크립트를 Bamboo에서 실행하도록 스크립트 작업을 구성합니다. 작업 구성에서 스크립트 파일 경로를 지정하거나 인라인 스크립트를 입력할 수 있습니다.

4. **빌드 트리거 설정**: 필요한 경우 계획 구성에서 "트리거" 탭으로 이동하여 트리거를 설정합니다. 예를 들어, 저장소 변경 사항에 대한 자동 빌드를 원한다면 변경 사항 감지 트리거를 추가하십시오.

5. **빌드 실행**: 이제 태그를 사용하여 빌드를 실행할 준비가 되었습니다. 계획을 실행하면 스크립트 작업이 실행되어 최신 태그를 가져와 빌드를 수행합니다.

이 방법을 사용하면 Bamboo 6.7.1에서 태그를 사용하여 빌드를 수행할 수 있습니다. 다만 이 방법은 명시적으로 태그를 사용하는 기능이 아니라 스크립트를 사용하여 간접적으로 태그를 사용하는 방법입니다. 최신 버전의 Bamboo를 사용하면 더 직접적인 태그 지원을 이용할 수 있습니다.

