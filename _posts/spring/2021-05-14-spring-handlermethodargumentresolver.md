---
title: '[Spring] HandlerMethodArgumentResolver'
author: da-nyee
date: 2021-05-16 16:19:42 +0900
categories: [TIL, Spring]
tags: [spring, handlermethodargumentresolver]
---

## HandlerMethodArgumentResolver?

- Strategy interface for resolving method parameters into argument values in the context of a given request.
- 컨트롤러 메소드에서 특정 조건에 맞는 파라미터가 있을 때 원하는 값을 바인딩 해주는 인터페이스

```
public interface HandlerMethodArgumentResolver
```
```
boolean supportsParameter(MethodParameter parameter)

Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory)
```

## Example

### 1. Write Annotation

- 어노테이션 클래스를 지정한다.
- `@AuthenticationPrincipal` 어노테이션을 생성한다.

```
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface AuthenticationPrincipal {
}
```

### 2. Write DTO

- DTO로 직렬화를 구현한다.

```
public class LoginMember {
    private Long id;
    private String email;

    public LoginMember() {
    }

    public LoginMember(Long id, String email) {
        this.id = id;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public String getEmail() {
        return email;
    }
}
```

### 3. Write Resolver

- AuthenticationPrincipalArgumentResolver
    - HandlerMethodArgumentResolver 인터페이스를 구현하는 클래스
    - supportsParameter(), resolveArgument()를 구현해야 한다.
- supportsParameter()
    - 파라미터가 Resolver에 의해 수행될 수 있는 타입인지 `true/false`를 리턴하는 메소드
    - 만약, `true`를 리턴하면 resolveArgument()를 실행한다.
- resolveArgument()
    - 실제로 파라미터와 바인딩할 객체를 리턴하는 메소드
    - `NativeWebRequest`로 클라이언트 요청이 담긴 파라미터를 컨트롤러보다 먼저 받아 작업을 수행할 수 있다.

```
public class AuthenticationPrincipalArgumentResolver implements HandlerMethodArgumentResolver {
    private final AuthService authService;

    public AuthenticationPrincipalArgumentResolver(AuthService authService) {
        this.authService = authService;
    }

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.hasParameterAnnotation(AuthenticationPrincipal.class);
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) {
        HttpServletRequest request = (HttpServletRequest) webRequest.getNativeRequest();
        Member member = authService.findMemberByToken(AuthorizationExtractor.extract(request));
        return new LoginMember(member.getId(), member.getEmail());
    }
}
```

### 4. Register Resolver

- 앞에서 작성한 `AuthenticationPrincipalArgumentResolver`를 스프링에 등록한다.

```
@Configuration
public class AuthenticationPrincipalConfig implements WebMvcConfigurer {
    private final AuthService authService;

    public AuthenticationPrincipalConfig(AuthService authService, LoginInterceptor loginInterceptor) {
        this.authService = authService;
    }

    @Override
    public void addArgumentResolvers(List argumentResolvers) {
        argumentResolvers.add(createAuthenticationPrincipalArgumentResolver());
    }

    @Bean
    public AuthenticationPrincipalArgumentResolver createAuthenticationPrincipalArgumentResolver() {
        return new AuthenticationPrincipalArgumentResolver(authService);
    }
}
```

### 5. Write Controller

- 컨트롤러 메소드 파라미터에 `@AuthenticationPrincipal`을 사용해서 `LoginMember` 정보를 가져온다.

```
@GetMapping("/me")
public ResponseEntity<MemberResponse> findMemberOfMine(@AuthenticationPrincipal LoginMember loginMember) {
    MemberResponse member = memberService.findMember(loginMember);
    return ResponseEntity.ok().body(member);
}
```

## References

- [HandlerMethodArgumentResolver (Spring Framework 5.3.6 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodArgumentResolver.html)
- [HandlerMethodArgumentResolver란?](https://webcoding-start.tistory.com/59)
