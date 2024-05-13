---
layout: post
title: "WebSocket 권한 설정 변경"
date: 2023-09-06
tags: [설정, WebSocket, 권한, 변경]
categories: blog
---

## 사용자 입력:
> springboot websocket 설정에서     @Override
    protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
        messages
                .simpDestMatchers("/qtrackStomp/**").authenticated() // 인증된 사용자만 허용
                //.hasAnyAuthority("ROLE_ADMIN", "ROLE_ANONYMOUS")  // "ROLE_ADMIN" 또는 "ROLE_ANONYMOUS" 권한을 가진 사용자에게 접근을 허용



    }  이게 이렇게 했는데, ROLE_ADMIN 권한을 가진 사람에게 접근 권한을 추가할려면?

## ChatGPT 응답:
> `ROLE_ADMIN` 권한을 가진 사용자에게 접근 권한을 추가하려면 `hasAuthority` 또는 `hasAnyAuthority` 메서드를 사용할 수 있습니다.

만약 `ROLE_ADMIN` 권한만 가진 사용자에게 접근 권한을 주려면:
```java
.simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN")
```

만약 `ROLE_ADMIN` 권한 뿐만 아니라 다른 권한을 가진 사용자에게도 접근 권한을 주려면:
```java
.simpDestMatchers("/qtrackStomp/**").hasAnyAuthority("ROLE_ADMIN", "OTHER_ROLE")
```

여기서 `"OTHER_ROLE"`은 다른 권한 이름을 의미합니다.

따라서 당신의 코드에서 `ROLE_ADMIN` 권한을 가진 사용자에게 접근 권한을 추가하려면 다음과 같이 코드를 수정할 수 있습니다:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN");
}
```

이렇게 하면 `/qtrackStomp/**` 경로에 대한 접근은 `ROLE_ADMIN` 권한을 가진 사용자만 가능합니다.

## 사용자 입력:
> 위 코드가 authen된 사용자도 포함하는 코드인가?

## ChatGPT 응답:
> 아니요, 제가 제시한 코드는 오직 `ROLE_ADMIN` 권한을 가진 사용자만 `/qtrackStomp/**` 경로에 접근할 수 있게 합니다.

만약 인증된 모든 사용자와 `ROLE_ADMIN` 권한을 가진 사용자 모두에게 접근 권한을 주려면, 다음과 같이 `authenticated()`와 `hasAuthority("ROLE_ADMIN")`를 모두 사용해야 합니다:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").authenticated()
            .simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN");
}
```

이렇게 설정하면 `/qtrackStomp/**` 경로는 인증된 사용자와 `ROLE_ADMIN` 권한을 가진 사용자 모두 접근할 수 있습니다. 

그러나 이런 설정은 실제로 의미가 없습니다. 왜냐하면 `authenticated()` 메서드는 이미 모든 인증된 사용자에게 접근을 허용하므로, `hasAuthority("ROLE_ADMIN")`는 불필요한 추가 설정이 됩니다. 

따라서 원하는 동작을 수행하려면 `authenticated()`만 사용하면 됩니다.

## 사용자 입력:
> 원하는건 개발모드에서 로그인 유저없이 ROLE_ADMIN을 사용하기 때문이야. 다시 만들어 주겠어.

## ChatGPT 응답:
> 아, 이해했습니다. 개발 모드에서 인증 없이 `ROLE_ADMIN`을 사용하려면, 그리고 실제 운영 모드에서는 인증된 사용자만 접근하도록 하려면 다음과 같이 설정할 수 있습니다:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").authenticated()
            .simpDestMatchers("/qtrackStomp/dev/**").hasAuthority("ROLE_ADMIN");
}
```

이 설정에 따르면:

1. `/qtrackStomp/**` 경로는 인증된 모든 사용자가 접근할 수 있습니다.
2. 개발 모드에서의 `/qtrackStomp/dev/**` 경로는 `ROLE_ADMIN` 권한을 가진 사용자만 접근할 수 있습니다. 

이렇게 하면, 실제 운영 환경에서는 인증된 사용자만 메인 경로에 접근할 수 있고, 개발 환경에서는 `ROLE_ADMIN` 권한을 사용하여 추가 경로에 접근할 수 있습니다.

## 사용자 입력:
> 운영환경에서도 같은 걸 하려고 하는거야.

## ChatGPT 응답:
> 알겠습니다. 운영 환경에서도 인증된 사용자와 `ROLE_ADMIN` 권한을 가진 사용자 모두가 `/qtrackStomp/**` 경로에 접근할 수 있도록 설정하려면 다음과 같이 해야 합니다:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").authenticated()
            .simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN");
}
```

그러나 이 설정은 앞서 언급한 것처럼 중복됩니다. `authenticated()` 메서드는 이미 모든 인증된 사용자에게 접근을 허용하므로, `hasAuthority("ROLE_ADMIN")`는 불필요한 추가 설정이 됩니다.

만약 **인증되지 않은** 사용자도 `ROLE_ADMIN` 권한으로 접근하게 하려면 (즉, 로그인하지 않은 상태에서도 `ROLE_ADMIN` 권한을 테스트하려면), 별도의 설정이 필요합니다. 이를 위해선 Spring Security의 구성에서 일부 사용자를 `ROLE_ADMIN` 권한으로 임시 인증하는 로직을 추가해야 합니다.

예를 들어, 인증 필터를 사용하여 특정 IP 주소 또는 특정 조건에서 사용자를 `ROLE_ADMIN`으로 자동 인증할 수 있습니다. 

이러한 설정은 보안 리스크가 있을 수 있으므로 신중하게 구현하고, 외부에 노출되지 않도록 주의해야 합니다.

## 사용자 입력:
> 위에 순서가 관계있나? authen과 authrity

## ChatGPT 응답:
> 네, Spring Security의 규칙 순서는 중요합니다. 순서대로 평가되므로 첫 번째로 일치하는 규칙이 적용됩니다. 만약 여러 규칙이 있고 첫 번째 규칙이 모든 요청에 일치한다면, 그 후의 규칙은 절대로 평가되지 않습니다.

예를 들어:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").authenticated()
            .simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN");
}
```

이 설정에서는 첫 번째 규칙 `.authenticated()`가 `/qtrackStomp/**`에 해당하는 모든 요청에 일치하므로, 두 번째 규칙 `.hasAuthority("ROLE_ADMIN")`은 절대로 평가되지 않습니다. 따라서 이 설정은 실질적으로 `ROLE_ADMIN` 권한을 검사하지 않습니다.

반대 순서로 규칙을 배치하면:

```java
@Override
protected void configureInbound(MessageSecurityMetadataSourceRegistry messages) {
    messages
            .simpDestMatchers("/qtrackStomp/**").hasAuthority("ROLE_ADMIN")
            .simpDestMatchers("/qtrackStomp/**").authenticated();
}
```

이 경우, 첫 번째 규칙에서 `ROLE_ADMIN` 권한을 가진 사용자만 접근이 허용됩니다. 그러나 두 번째 규칙은 절대 평가되지 않기 때문에 실질적으로 `authenticated()` 규칙은 무시됩니다.

따라서 위의 설정 중 어느 것도 원하는 결과를 제공하지 않습니다. 

다양한 권한과 인증 요구 사항을 동시에 충족하려면 별도의 로직이 필요합니다.

## 사용자 입력:
> hasAuthority와 hasAnyAuthority의 차이는?

## ChatGPT 응답:
> `hasAuthority`와 `hasAnyAuthority`는 Spring Security에서 권한 기반의 접근 제어를 위한 메서드입니다. 이 두 메서드의 주요 차이는 다음과 같습니다:

1. **`hasAuthority`**:
    - 이 메서드는 사용자가 지정된 **단일** 권한을 가지고 있는지 검사합니다.
    - 예: `.hasAuthority("ROLE_ADMIN")`은 사용자가 "ROLE_ADMIN" 권한을 가지고 있는 경우에만 접근을 허용합니다.

2. **`hasAnyAuthority`**:
    - 이 메서드는 사용자가 지정된 권한 **목록 중 하나 이상**을 가지고 있는지 검사합니다.
    - 예: `.hasAnyAuthority("ROLE_ADMIN", "ROLE_USER")`는 사용자가 "ROLE_ADMIN" 또는 "ROLE_USER" 권한 중 하나 또는 둘 다를 가지고 있을 경우 접근을 허용합니다.

요약하면:
- `hasAuthority`는 사용자가 특정 한 권한을 가지고 있는지 확인합니다.
- `hasAnyAuthority`는 사용자가 주어진 권한 목록 중 어느 하나를 가지고 있는지 확인합니다.

