---
title: '[우아한테크코스] 다니의 학습 로그 - 레벨 2'
author: da-nyee
date: 2021-06-22 17:46:16 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, learning log, level2]
---

> [체스 미션/step1](https://github.com/woowacourse/jwp-chess/pull/234)

## [Spring] @Controller vs @RestController - 3
### 내용
- @Controller
    - 전통적인 Spring MVC 컨트롤러
    - 주로 View를 반환하기 위해 사용한다.
        - 컨트롤러가 View를 반환할 때는 ViewResolver가 동작한다.
        - ViewResolver 설정에 맞는 View를 찾아 렌더링한다.
     - 경우에 따라 Data를 반환할 수도 있다.
        - @ResponseBody 어노테이션을 함께 사용해야 한다. 👉 이를 통해 컨트롤러도 데이터를 Json 형태로 반환할 수 있다.
        - 컨트롤러가 Data를 반환할 때는 HttpMessageConverter가 동작한다.
        - HttpMessageConverter에는 여러 Converter가 등록되어 있어 반환돼야 하는 객체에 따라 활용되는 Converter가 달라진다.
          - ex. 단순 문자열 👉 StringHttpMessageConverter
          - ex. 객체 👉 MappingJackson2HttpMessageConverter
        - 스프링은 클라이언트의 HTTP Accept 헤더 + 서버의 컨트롤러 반환 타입 정보를 조합해서 적절한 HttpMessageConverter를 선택하여 이를 처리한다.
- @RestController
    - @Controller + @ResponseBody
    - 주로 객체 데이터를 Json 형태로 반환하기 위해 사용한다.

### 링크
- [[Spring] @Controller와 @RestController 차이](https://mangkyu.tistory.com/49)
- [jwp-chess/src/main/java/chess/controller/ChessController.java](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/controller/ChessController.java)
- [jwp-chess/src/main/java/chess/controller/ChessApiController.java](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/controller/ChessApiController.java)
<br/>

## [Spring] WebMvcConfigurer: addViewControllers() - 3
### 내용
- ex. `/`를 입력했을 때 `/`에 해당하는 View로 바로 이동하는 방법
    - WebMvcConfigurer의 addViewControllers 메소드를 오버라이딩해서 처리할 수 있다.

### 링크
- [jwp-chess/src/main/java/chess/controller/config/WebConfig.java](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/controller/config/WebConfig.java)
<br/>

## [Spring] @ControllerAdvice (+ @ExceptionHandler) - 4
### 내용
- @ExceptionHandler
    - @Controller, @RestController가 적용된 Bean 내에서 발생하는 예외를 잡아 하나의 메소드에서 처리하는 기능을 담당한다.
    - 해당 어노테이션을 작성하고 파라미터로 캐치하고 싶은 예외 클래스를 등록해서 사용한다.
        - 두 개 이상의 예외 클래스도 등록할 수 있다.
        - ex. @ExceptionHandler(NullPointerException.class, IllegalArgumentException.class)
    - 주의사항
        - @Controller, @RestController에서만 적용할 수 있다. 👉 @Service와 같은 Bean에는 적용할 수 없다.
        - 반환 타입은 자유롭게 명시할 수 있다.
            - 컨트롤러 내부 메소드들은 여러 타입을 반환한다. 👉 해당 타입과 전혀 다른 타입을 반환해도 상관 없다.
        - @ExceptionHandler를 등록한 컨트롤러에만 적용할 수 있다.
        - 메소드의 파라미터도 자유롭게 받아올 수 있다.
            - ex. @ExceptionHandler(RuntimeException.class)로 등록하고 메소드의 파라미터로 Exception을 받아와도 상관 없다.
- @ControllerAdvice, @RestControllerAdvice
    - 모든 @Controller에서 == 전역에서 발생할 수 있는 예외를 잡아 처리하는 어노테이션
    - 새로운 클래스 파일을 만든 후 클래스 상단에 해당 어노테이션을 붙여 사용한다.
    - 객체 데이터를 Json 형식으로 반환하고 싶다면 @RestControllerAdvice == @ControllerAdvice + @ResponseBody
    - 메소드 상단에 @ExceptionHandler 어노테이션을 붙여 핸들링하고 싶은 예외를 잡아 처리한다.
        - ex. @ExceptionHandler(RuntimeException.class)
    - @ControllerAdvice는 ViewResolver를 통해 예외 처리 페이지로 리다이렉트 시키려는 경우에 사용한다.
    - @RestControllerAdvice는 API 서버여서 예외 응답으로 객체를 반환하는 경우에 사용한다.

### 링크
- [@ControllerAdvice, @ExceptionHandler를 이용한 예외처리 분리, 통합하기(Spring에서 예외 관리하는 방법, 실무에서는 어떻게?)](https://jeong-pro.tistory.com/195)
- [jwp-chess/src/main/java/chess/exception/ChessAdvice.java](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/exception/ChessAdvice.java)
- [jwp-chess/src/main/java/chess/service/ChessService.java/move()](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/service/ChessService.java#L149)
<br/>

## [Java] Jackson: Json <-> Object 변환 - 2
### 내용
- Serialization
    - Object -> Json 변환
- Deserialization
    - Json -> Object 변환

### 링크
- [[Java] Jackson으로 Json <-> Object 변환(Transformation)하기](https://ramees.tistory.com/33)
- [jwp-chess/src/main/java/chess/service/ChessService.java/jsonFormatChessBoard()](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/service/ChessService.java#L104)
<br/>

## [Web] HTTP 상태코드 - 2
### 내용
- 100번대 👉 정보
    - 요청을 받았으며 프로세스를 계속한다.
- 200번대 👉 성공
    - 요청을 성공적으로 받았으며 인식하고 수용했다.
    - ex. HttpStatus.OK
- 300번대 👉 리다이렉션
    - 요청 완료를 위해 추가 작업이 필요하다.
- 400번대 👉 클라이언트 오류
    - 요청의 문법이 잘못되었거나 요청을 처리할 수 없다.
    - ex. HttpStatus.NOT_FOUND
- 500번대 👉 서버 오류
    - 서버가 명백히 유효한 요청에 대해 충족을 실패했다.
    - ex. HttpStatus.BAD_GATEWAY

### 링크
- [HTTP 상태 코드](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C#:~:text=100(%EA%B3%84%EC%86%8D)%3A%20%EC%9A%94%EC%B2%AD%EC%9E%90%EB%8A%94,%EB%8A%94%20%EC%9D%B4%EB%A5%BC%20%EC%8A%B9%EC%9D%B8%ED%95%98%EB%8A%94%20%EC%A4%91%EC%9D%B4%EB%8B%A4.)
- [jwp-chess/src/main/java/chess/controller/ChessApiController.java/turn()](https://github.com/da-nyee/jwp-chess/blob/step1/src/main/java/chess/controller/ChessApiController.java#L30)

<br/>

> [체스 미션/step2](https://github.com/woowacourse/jwp-chess/pull/299)

## [Spring] Entity vs DTO - 3
### 내용
- [[Spring] Entity vs DTO](https://da-nyee.github.io/posts/spring-entity-vs-dto/)

### 링크
- [jwp-chess/src/main/java/chess/dto/](https://github.com/da-nyee/jwp-chess/tree/step2/src/main/java/chess/dto)
- [jwp-chess/src/main/java/chess/dao/dto/response/](https://github.com/da-nyee/jwp-chess/tree/step2/src/main/java/chess/dao/dto/response)

<br/>

> 배포 인프라 미션

## [Web] 웹의 동작 과정 - 4
### 내용
- [브라우저에서 google.com를 요청할 때 통신 과정이 어떻게 이루어질까요?](https://da-nyee.github.io/posts/woowa-course-deployment-quizzes-and-answers/#1)

### 태그
{Web}, {Network}, {HTTP}, {HTTPS}

## [Infrastructure] Reverse Proxy - 4
### 내용
- [Reverse Proxy를 별도로 구성함으로써 얻는 이점을 설명해주세요.](https://da-nyee.github.io/posts/woowa-course-operation-quizzes-and-answers/#4)

### 태그
{Infrastructure}, {Reverse Proxy}, {Nginx}

<br/>

> [지하철 노선도 관리 미션/step1](https://github.com/woowacourse/atdd-subway-map/pull/77)

## [Spring] Logging: Logback - 3.5
### 내용
#### Logback?
- 자바 오픈소스 로깅 프레임워크
- SLF4J 구현체
- 스프링부트는 기본으로 등록되어 있어 사용 시 별도로 라이브러리를 추가하지 않아도 된다.
- spring-boot-starter-web 안에 spring-boot-starter-logging 구현체가 있다.

#### 로그 레벨
- 로그 레벨은 `TRACE < DEBUG < INFO < WARN < ERROR` 순
- 만약, 로그 레벨을 INFO로 설정하면?
  - 해당 레벨보다 더 높은 레벨인 WARN, ERROR도 함께 기록된다.
  - 해당 레벨보다 더 낮은 레벨인 TRACE, DEBUG는 함께 기록되지 않는다.
- 개발 서버는 DEBUG, 운영 서버는 INFO, 로컬 서버는 TRACE 또는 DEBUG 정도 사용한다.

### 링크
- [[스프링부트 (5)] Spring Boot 로그 설정(1) - Logback](https://goddaehee.tistory.com/206)
- [스프링의 로깅 라이브러리 - logback!](https://kouzie.github.io/spring/logback/#)

<br/>

> [지하철 노선도 관리 미션/step2](https://github.com/woowacourse/atdd-subway-map/pull/141)

## [Spring] Bean Validation - 3
### 내용
- [[Spring] Bean Validation](https://da-nyee.github.io/posts/spring-bean-validation/)

### 태그
{Spring}, {Bean Validation}, {validation}, {annotation}

<br/>

> [경로 조회 / 로그인 미션/step1](https://github.com/woowacourse/atdd-subway-path/pull/57)

## [Spring] HandlerMethodArgumentResolver - 4
### 내용
- [[Spring] HandlerMethodArgumentResolver](https://da-nyee.github.io/posts/spring-handlermethodargumentresolver/)

### 태그
{Spring}, {HandlerMethodArgumentResolver}, {interface}, {annotation}

## [Spring] Interceptor - 4
### 내용
#### Interceptor?
![spring-mvc-lifecycle](https://user-images.githubusercontent.com/50176238/122893531-61cd0480-d381-11eb-81eb-6bb40c9993c8.jpeg)

- Dispatcher Servlet과 Controller 사이에서 HttpServletRequest, HttpServletResponse를 가로채는 역할을 한다.
- `HandlerInterceptor` 인터페이스
  - preHandle(): Controller 실행 전 호출 / View 생성되지 않은 상태 / boolean 반환
  - postHandle(): Controller 실행 후 호출 / View 생성되지 않은 상태 / void 반환
  - afterCompletion(): 모든 request 처리 완료 후 호출 / View 생성된 상태 / void 반환

#### Configuration
- `WebMvcConfigurer` 인터페이스를 구현한 클래스에서 `addInterceptors()`를 오버라이딩해서 Interceptor를 사용한다.
  - addInterceptors(): Interceptor를 등록할 URI 경로를 지정한다.
  - excludeInterceptors(): Interceptor를 제외할 URI 경로를 지정한다.

```
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(subwayInterceptor)
            .addPathPatterns(
                    "/api/members/me/**",
                    "/api/stations/**",
                    "/api/lines/**",
                    "/api/paths"
            );
}
```

### 링크
- [Spring Interceptor / 로그인 확인 / Spring boot](https://ecsimsw.tistory.com/entry/Spring-Interceptor-Spring-boot)
- [Introduction to Spring MVC HandlerInterceptor](https://www.baeldung.com/spring-mvc-handlerinterceptor)

<br/>

> [경로 조회 / 로그인 미션/step2](https://github.com/woowacourse/atdd-subway-path/pull/128)

## [Spring] Interceptor - CORS Issue - 3.5
### 내용
- [[Spring] Interceptor - CORS Issue](https://da-nyee.github.io/posts/spring-interceptor-cors-issue/)

### 태그
{Spring}, {Interceptor}, {CORS}, {issue}, {Preflight Request}

<br/>

> [협업 미션/step1](https://github.com/woowacourse/atdd-subway-fare/pull/22)

## [Linux] Commands - 3
### 내용
- [[Linux] netstat 명령어 (netstat Command)](https://da-nyee.github.io/posts/linux-netstat-command/)
- [[Linux] pidof 명령어 (pidof Command)](https://da-nyee.github.io/posts/linux-pidof-command/)

### 태그
{Linux}, {netstat}, {pidof}, {Commands}

## [Network] OSI Model: OSI 7 Layers - 4
### 내용
- [[Network] L2 오류 제어 vs L4 오류 제어 (L2 Error Handling vs L4 Error Handling)](https://da-nyee.github.io/posts/network-l2-error-handlingvs-l4-error-handling/)

### 태그
{Network}, {OSI Model}, {OSI 7 Layers}, {L2}, {Data Link Layer}, {L4}, {Transport Layer}, {Error Handling}

## [Java] Gradle Dependency Configurations - 4
### 내용
- [[Java] Gradle Dependency Configurations](https://da-nyee.github.io/posts/java-gradle-dependency-configurations/)

### 태그
{Java}, {Gradle}, {Dependency}, {Configurations}, {compileOnly}, {runtimeOnly}, {implementation}, {api}