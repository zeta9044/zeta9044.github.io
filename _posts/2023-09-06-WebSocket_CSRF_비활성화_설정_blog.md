---
layout: post
title: "WebSocket CSRF 비활성화 설정"
date: 2023-09-06
tags: [활성화, 비, WebSocket, CSRF, 설정]
categories: blog
---

## 사용자 입력:
> /**
 * Web소켓 보안설정
 */
@Configuration
class WebSocketSecurityConfig extends AbstractSecurityWebSocketMessageBrokerConfigurer {
    @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
                .simpDestMatchers("/qtrackStomp/**").hasAnyAuthority("ROLE_ADMIN", "ROLE_ANONYMOUS")  // "ROLE_ADMIN" 또는 "ROLE_ANONYMOUS" 권한을 가진 사용자에게 접근을 허용
                .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
    }
}
이 설정에서 csrf를 체크 못하게 하려면

## ChatGPT 응답:
> `AbstractSecurityWebSocketMessageBrokerConfigurer`의 `configureInbound` 메서드에서 CSRF (Cross-Site Request Forgery) 보호를 비활성화하려면 해당 설정에 메서드를 추가하여 CSRF를 비활성화해야 합니다.

아래는 CSRF를 비활성화하는 방법입니다:

```java
@Configuration
class WebSocketSecurityConfig extends AbstractSecurityWebSocketMessageBrokerConfigurer {

    @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
                .simpDestMatchers("/qtrackStomp/**").hasAnyAuthority("ROLE_ADMIN", "ROLE_ANONYMOUS")  // "ROLE_ADMIN" 또는 "ROLE_ANONYMOUS" 권한을 가진 사용자에게 접근을 허용
                .anyMessage().permitAll();  // 그 외 메시지는 모두 허용
    }

    @Override
    protected boolean sameOriginDisabled() {
        return true;  // CSRF 보호를 비활성화
    }
}
```

여기서 `sameOriginDisabled` 메서드를 재정의하여 `true`를 반환하게 하면 WebSocket에서 CSRF 보호를 비활성화하게 됩니다.

