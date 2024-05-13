---
layout: post
title: "SpringBoot WebSocket 보안 설정"
date: 2023-09-05
tags: [설정, 보안, SpringBoot, WebSocket]
categories: blog
---

## 사용자 입력:
> springboot websocket 사용할 때, security context가 적용될려면?

## ChatGPT 응답:
> Spring Boot와 WebSocket을 함께 사용할 때, 보안 문제는 주요한 고려 사항입니다. Spring Security를 사용하여 WebSocket을 보호하는 방법에 대해 다음 단계를 따라 설명하겠습니다:

1. **의존성 추가**: `pom.xml` 또는 `build.gradle`에 Spring Security와 WebSocket 의존성을 추가합니다.

```xml
<!-- Spring Security -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- WebSocket -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

2. **Spring Security 설정**: `WebSecurityConfigurerAdapter`를 확장하는 클래스를 만들어 Spring Security를 설정합니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            // ... 다른 보안 설정 ...
            .csrf().disable()  // CSRF 보안을 비활성화 (WebSocket에 대해서는 필요할 수 있음)
            .authorizeRequests()
                .antMatchers("/ws/**").authenticated()  // "/ws/"로 시작하는 경로는 인증이 필요
                // ... 다른 경로 권한 설정 ...
            .and()
            .httpBasic();  // 기본 인증을 사용 (필요에 따라 다른 인증 방법을 사용할 수 있음)
    }
}
```

3. **WebSocket 보안 설정**: `WebSocketSecurity`를 활용하여 WebSocket의 보안을 설정합니다.

```java
@Configuration
public class WebSocketSecurityConfig extends AbstractSecurityWebSocketMessageBrokerConfigurer {

    @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
            .simpDestMatchers("/ws/**").authenticated()  // "/ws/"로 시작하는 목적지는 인증이 필요
            .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
    }

    // CORS 설정 시 아래 메서드를 오버라이드 해서 false로 설정해야 합니다.
    @Override
    protected boolean sameOriginDisabled() {
        return true;
    }
}
```

4. **인증 프로세스**: 사용자 인증을 위한 서비스 및 저장소를 설정합니다. 예를 들어, `UserDetailsService` 및 `PasswordEncoder`를 제공해야 할 수 있습니다.

이러한 단계를 거치면 Spring Boot 프로젝트에 WebSocket과 Spring Security를 통합할 수 있습니다. 하지만, 이것은 기본적인 설정이므로 실제 운영 환경에서는 추가적인 보안 고려 사항과 세부 설정이 필요할 수 있습니다.

## 사용자 입력:
> 위 내용을 설정하면, websocket에서 userdetail을 가져올 수 잇나?


## ChatGPT 응답:
> 네, WebSocket에서 Spring Security의 `Authentication` 객체를 통해 `UserDetails`에 접근할 수 있습니다. `Principal` 객체는 Spring Security의 `Authentication` 객체와 연관되어 있으므로, WebSocket 핸들러나 컨트롤러 내에서 `Principal` 객체를 파라미터로 받아 사용할 수 있습니다.

예를 들면:

### 1. WebSocket 핸들러에서 `Principal` 사용하기:

WebSocket 핸들러에서 `@MessageMapping`이나 `@SubscribeMapping`을 사용하는 경우, 메서드의 인자로 `Principal`을 추가하여 현재 인증된 사용자의 정보를 가져올 수 있습니다.

```java
@Controller
public class MyWebSocketController {

    @MessageMapping("/message")
    public void handleMessage(Principal principal, String message) {
        // principal.getName()을 사용하여 현재 인증된 사용자의 이름을 가져올 수 있습니다.
        String username = principal.getName();
        // ...
    }
}
```

### 2. `UserDetails` 가져오기:

`Principal`을 통해 `Authentication` 객체에 접근한 후, `UserDetails`를 가져올 수 있습니다.

```java
if (principal instanceof Authentication) {
    Authentication auth = (Authentication) principal;
    Object principalObj = auth.getPrincipal();

    if (principalObj instanceof UserDetails) {
        UserDetails userDetails = (UserDetails) principalObj;
        // userDetails에서 필요한 정보를 가져옵니다.
        String username = userDetails.getUsername();
        Collection<? extends GrantedAuthority> authorities = userDetails.getAuthorities();
        // ...
    }
}
```

이렇게 하면 WebSocket 핸들러 내에서 Spring Security를 통해 현재 인증된 사용자의 `UserDetails` 정보를 가져올 수 있습니다.

## 사용자 입력:
> 	@Override
	public void registerStompEndpoints(StompEndpointRegistry registry) {
		registry.addEndpoint("/qtrackStomp").setAllowedOrigins("*").withSockJS()
	}  이렇게 되어 있으면,      @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
                .simpDestMatchers("/ws/**").authenticated()  // "/ws/"로 시작하는 목적지는 인증이 필요
                .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
    } 에서    .simpDestMatchers("/ws/**").authenticated()  은 어떻게 바뀌어야 되지?

## ChatGPT 응답:
> WebSocket의 엔드포인트가 `/qtrackStomp`로 설정되어 있다면, `MessageSecurityMetadataSourceRegistry`에서의 경로도 이를 반영해야 합니다.

이 경우, `.simpDestMatchers("/ws/**").authenticated()` 부분의 경로를 `/qtrackStomp/**`로 바꿔야 합니다.

수정된 코드는 다음과 같습니다:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").authenticated()  // "/qtrackStomp/"로 시작하는 목적지는 인증이 필요
            .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
}
```

이렇게 설정하면 `/qtrackStomp` 엔드포인트 및 그 하위 경로로 들어오는 WebSocket 메시지는 인증을 요구하게 됩니다.

## 사용자 입력:
> "ROLE_admin".equals(ROLE_ANONYMOUS) 이경우 일때는 authenticated됐다고 처리하는 방법은?

## ChatGPT 응답:
> Spring Security에서 "ROLE_ANONYMOUS"는 일반적으로 인증되지 않은 사용자를 나타냅니다. 그러나 특정 요구 사항에 따라 "ROLE_ANONYMOUS"를 인증된 것으로 취급하려면, 사용자가 이 역할을 갖고 있을 때도 접근을 허용해야 합니다.

이를 위해 `MessageSecurityMetadataSourceRegistry` 설정을 조정할 수 있습니다. 특히 `.hasAuthority()` 또는 `.hasAnyAuthority()` 메서드를 사용하여 해당 권한을 가진 사용자에게 접근을 허용할 수 있습니다.

예를 들어:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").hasAnyAuthority("ROLE_ADMIN", "ROLE_ANONYMOUS")  // "ROLE_ADMIN" 또는 "ROLE_ANONYMOUS" 권한을 가진 사용자에게 접근을 허용
            .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
}
```

이렇게 설정하면 "ROLE_ADMIN" 또는 "ROLE_ANONYMOUS" 권한을 가진 사용자는 `/qtrackStomp` 경로로의 WebSocket 메시지를 보낼 수 있습니다.

그러나 "ROLE_ANONYMOUS"를 인증된 것으로 취급하는 것은 일반적인 방법이 아니므로, 이런 설정을 사용하기 전에 요구 사항과 보안 요건을 잘 검토해야 합니다.

## 사용자 입력:
> 위 조건을 합집합으로 되게 할 순 없어?

## ChatGPT 응답:
> 네, `MessageSecurityMetadataSourceRegistry`의 설정에서 여러 조건을 합집합으로 적용하는 것은 기본적으로 지원되지 않습니다. 그러나 이를 위한 회피 방법이 있습니다.

1. **Custom Access Decision Voter**: 

   직접 `AccessDecisionVoter`를 구현하여 원하는 로직을 정의할 수 있습니다. 이 `AccessDecisionVoter`는 인증된 사용자의 역할을 확인하고 원하는 조건을 기반으로 접근을 허용하거나 거부할 수 있습니다.

2. **Expression-Based Access Control**: 

   SpEL (Spring Expression Language)을 사용하여 접근 제어 표현식을 정의할 수 있습니다. 

예를 들어, Custom Access Decision Voter를 사용하는 방법은 다음과 같습니다:

1. **Custom Voter 구현**:

```java
public class CustomWebSocketAccessVoter implements AccessDecisionVoter<Message<Object>> {

    @Override
    public boolean supports(ConfigAttribute attribute) {
        return true;
    }

    @Override
    public boolean supports(Class<?> clazz) {
        return Message.class.isAssignableFrom(clazz);
    }

    @Override
    public int vote(Authentication authentication, Message<Object> object, Collection<ConfigAttribute> attributes) {
        // 여기서 원하는 로직을 정의하고 접근을 허용할지 거부할지 결정
        if (authentication.getAuthorities().stream()
            .anyMatch(grantedAuthority -> 
                "ROLE_ADMIN".equals(grantedAuthority.getAuthority()) || 
                "ROLE_ANONYMOUS".equals(grantedAuthority.getAuthority()))) {
            return ACCESS_GRANTED;
        }
        return ACCESS_DENIED;
    }
}
```

2. **Security Configuration에 Voter 추가**:

```java
@Configuration
public class WebSocketSecurityConfig extends AbstractSecurityWebSocketMessageBrokerConfigurer {

    @Autowired
    private CustomWebSocketAccessVoter customWebSocketAccessVoter;

    @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
            .simpDestMatchers("/qtrackStomp/**")
            .accessDecisionManager(accessDecisionManager())
            .authenticated();

        // 그 외 설정
    }

    @Bean
    public AccessDecisionManager accessDecisionManager() {
        List<AccessDecisionVoter<?>> decisionVoters = Arrays.asList(customWebSocketAccessVoter);
        return new AffirmativeBased(decisionVoters);
    }
}
```

이렇게 설정하면 `/qtrackStomp` 경로로의 WebSocket 메시지는 `CustomWebSocketAccessVoter`를 통해 검사되어 원하는 조건에 따라 접근이 허용되거나 거부됩니다.

