---
title: '[Spring] Interceptor - CORS Issue'
author: da-nyee
date: 2021-05-21 00:39:48 +0900
categories: [TIL, Spring]
tags: [spring, interceptor, cors, issue]
---

지하철 경로 조회 미션에서 JWT를 사용해서 로그인 시 토큰으로 인증을 받도록 구현했다.
이때, Interceptor를 이용해서 Controller에 요청이 들어가기 전에 유효한지 확인하게 만들었다.
몇몇 크루가 이 단계에서 `CORS 에러`를 만나게 됐고, 무엇이 문제인지 원인을 분석했다.
왜 이런 에러가 발생했는지 그 이유와 해결 방안을 정리하려 한다.

<br/>

## Issue

Interceptor에는 아래와 같은 로직이 있다.

```
@Component
public class LoginInterceptor implements HandlerInterceptor {
    private final JwtTokenProvider jwtTokenProvider;

    public LoginInterceptor(JwtTokenProvider jwtTokenProvider) {
        this.jwtTokenProvider = jwtTokenProvider;
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String token = AuthorizationExtractor.extract(request);
        jwtTokenProvider.validateToken(token);
        return true;
    }
}
```

이는 요청에서 토큰을 꺼내고, 해당 토큰이 유효한지 확인하는 로직이다.
만약 토큰이 유효하지 않으면 예외를 발생한다.
바로 여기서 CORS 에러가 있었다. 왜냐하면 클라이언트에서 서버로 요청을 보낼 때,
브라우저는 웹 어플리케이션(ex. 8081)이 다른 출처(ex. 8080)에 접근하기 전에 접근 권한이 있는지 `Preflight Request`를 보내 확인한다.
근데, Preflight Request는 미리 보내보는 요청이다. 즉, 해당 요청에는 `Authorization` 헤더와 토큰이 없다. 그래서 에러가 발생했다.
요청에 토큰이 없는데 토큰을 검사하려 하니 `NPE`가 발생했다.

<br/>

## Solution

간단하게 해결할 수 있다. 현재 요청이 `Preflight Request`인지 확인하는 로직을 추가한다.

```
@Component
public class LoginInterceptor implements HandlerInterceptor {
    private final JwtTokenProvider jwtTokenProvider;

    public LoginInterceptor(JwtTokenProvider jwtTokenProvider) {
        this.jwtTokenProvider = jwtTokenProvider;
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        if (isPreflightRequest(request)) {
            return true;
        }
        String token = AuthorizationExtractor.extract(request);
        jwtTokenProvider.validateToken(token);
        return true;
    }

    private boolean isPreflightRequest(HttpServletRequest request) {
        return isOptions(request) && hasHeaders(request) && hasMethod(request) && hasOrigin(request);
    }

    private boolean isOptions(HttpServletRequest request) {
        return request.getMethod().equalsIgnoreCase(HttpMethod.OPTIONS.toString());
    }

    private boolean hasHeaders(HttpServletRequest request) {
        return Objects.nonNull(request.getHeader("Access-Control-Request-Headers"));
    }

    private boolean hasMethod(HttpServletRequest request) {
        return Objects.nonNull(request.getHeader("Access-Control-Request-Method"));
    }

    private boolean hasOrigin(HttpServletRequest request) {
        return Objects.nonNull(request.getHeader("Origin"));
    }
}
```

Preflight Request는 `OPTIONS` 메소드와 `Access-Control-Request-Headers`, `Access-Control-Request-Method`, `Origin` 헤더를 포함한다.<br/>

이렇게 작성하면 Preflight Request인 경우 Interceptor를 바로 통과하고, 문제 없이 프로그램이 동작하게 할 수 있다.