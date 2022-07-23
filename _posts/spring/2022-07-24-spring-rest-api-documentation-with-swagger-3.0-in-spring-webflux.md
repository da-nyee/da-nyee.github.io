---
title: '[Spring] Spring WebFlux + Swagger 3.0 적용'
author: da-nyee
date: 2022-07-24 00:52:56 +0900
categories: [TIL, Spring]
tags: [spring, spring webflux, swagger 3.0]
---

## 의존성 추가

build.gradle에 필요한 의존성을 추가한다.<br/>

```gradle
// webflux
implementation 'org.springframework.boot:spring-boot-starter-webflux'

// swagger
implementation 'io.springfox:springfox-boot-starter:3.0.0'
implementation 'io.springfox:springfox-swagger-ui:3.0.0'
```

<br/>

## Swagger 설정

SwaggerConfig 클래스를 생성하고, Swagger 설정에 필요한 코드를 작성한다.<br/>

```java
// SwaggerConfig.java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .useDefaultResponseMessages(false)
            .consumes(getConsumeContentTypes())
            .produces(getProduceContentTypes())
            .apiInfo(apiInfo())
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.example.gommunity"))
            .paths(PathSelectors.any())
            .build();
    }

    private Set<String> getConsumeContentTypes() {
        Set<String> consumeContentTypes = new HashSet<>();

        consumeContentTypes.add("application/json;charset=UTF-8");
        consumeContentTypes.add("application/x-www-form-urlencoded");

        return consumeContentTypes;
    }

    private Set<String> getProduceContentTypes() {
        Set<String> produceContentTypes = new HashSet<>();

        produceContentTypes.add("application/json;charset=UTF-8");

        return produceContentTypes;
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
            .title("Gommunity API")
            .description("Gommunity API")
            .build();
    }
}
```

<br/>

## 문서 커스터마이징

Controller, Request/Response DTO에 Swagger 어노테이션을 붙여 문서를 커스터마이징한다.

```java
// UserCommandController.java
@Api(tags = {"유저 등록, 수정"})
@RequiredArgsConstructor
@RestController
@RequestMapping("/api/users")
public class UserCommandController {

    private final UserCommandService userCommandService;

    @PutMapping
    public ResponseEntity<Mono<UserUpsertResponse>> upsertUser(
        @Valid @RequestBody UserUpsertRequest request
    ) {
        return ResponseEntity.ok(userCommandService.upsertUser(request));
    }
}

// UserUpsertRequest.java
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class UserUpsertRequest {

    @ApiModelProperty(value = "유저 id")
    private String id;

    @ApiModelProperty(value = "유저 이름", example = "다니")
    @NotBlank
    private String name;

    @ApiModelProperty(value = "유저 나이", example = "26")
    @NotNull
    private Integer age;
}

// UserUpsertResponse.java
@Builder
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class UserUpsertResponse {

    @ApiModelProperty(value = "유저 id")
    private String id;
}
```

<br/>

## 실행 결과

여기까지 진행하고 애플리케이션을 실행하면, 아래처럼 Swagger 문서가 생성된다.<br/>

<img width="1498" alt="image" src="https://user-images.githubusercontent.com/50176238/180609494-df94e212-ecc5-48a4-8bd0-6ef21702b473.png">

하지만 여기에는 한 가지 문제가 있다.<br/>
Request Body는 잘 나오지만, Response Body는 빈 배열( {} )로 나온다.<br/>

WebFlux는 `Mono<T>` 또는 `Flux<T>`로 응답하는데, 이와 관련된 설정을 하지 않아 생기는 문제이다.<br/>

<br/>

## 해결 방법

SwaggerConfig 클래스의 api()에 `Mono<T>`, `Flux<T>`와 관련된 설정을 추가한다.<br/>

```java
// SwaggerConfig.java
@Bean
public Docket api() {
    return new Docket(DocumentationType.SWAGGER_2)
        .useDefaultResponseMessages(false)
        .consumes(getConsumeContentTypes())
        .produces(getProduceContentTypes())
        .apiInfo(apiInfo())
        .select()
        .apis(RequestHandlerSelectors.basePackage("com.example.gommunity"))
        .paths(PathSelectors.any())
        .build()
        .genericModelSubstitutes(Mono.class, Flux.class);   // 이 부분을 추가한다.
}
```

<br/>

## 최종 결과

애플리케이션을 다시 실행해보면, 이제 Response Body도 잘 나오는 것을 확인할 수 있다.<br/>

<img width="1498" alt="image" src="https://user-images.githubusercontent.com/50176238/180609591-78fbdc70-41c5-4a9a-b51e-b2374ae072ae.png">

<br/>

## Reference

- [spring-boot-starter-webflux swagger 3 설정](https://itstory.tk/entry/springbootstarterwebflux-swagger-3-%EC%84%A4%EC%A0%95)