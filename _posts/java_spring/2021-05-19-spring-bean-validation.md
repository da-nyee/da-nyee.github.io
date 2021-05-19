---
title: '[Spring] Bean Validation'
author: da-nyee
date: 2021-05-19 21:54:28 +0900
categories: [TIL, Spring]
tags: [spring, bean validation]
---

## Dependency

- 스프링부트에서 validation을 하기 위해 아래 의존성을 추가한다.

```
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

## @NotNull

- 필드 값으로 `null`을 허용하지 않는다. 👉 `""`, `" "`은 허용한다.

```
public class SectionRequest {
    @NotNull
    @Positive
    private Long upStationId;
    @NotNull
    @Positive
    private Long downStationId;
}
```

## @NotEmpty

- 필드 값으로 `null`, `""`을 허용하지 않는다. 👉 `" "`은 허용한다.
- String, Collection, Map, Array에 사용할 수 있다.

```
public class LineRequest {
    @NotEmpty
    private String name;
}
```

## @NotBlank

- 필드 값으로 `null`, `""`, `" "`을 허용하지 않는다. 👉 셋 중 validation 강도가 가장 높다.
- String에만 사용할 수 있다.

```
public class LineRequest {
    @NotBlank
    private String color;
}
```

## Other Annotations
### @Positive

- 필드 값이 0이 아닌 양수인지 확인한다.
- 숫자에만 사용할 수 있다.

### @PositiveOrZero

- 필드 값이 0 또는 양수인지 확인한다.
- 숫자에만 사용할 수 있다.

### @Negative

- 필드 값이 0이 아닌 음수인지 확인한다.
- 숫자에만 사용할 수 있다.

### @NegativeOrZero

- 필드 값이 0 또는 음수인지 확인한다.
- 숫자에만 사용할 수 있다.

### @Email

- 필드 값이 유효한 이메일 주소인지 확인한다. (ex. XXX@XXXX.XXX)

### @Size

- 필드 값이 min과 max 사이의 값인지 확인한다.
- String, Collection, Map, Array에 사용할 수 있다.

```
public class LineRequest {
    @Size(min = 2, max = 10)
    private String name;
}
```

### @Min

- 필드 값이 value 값 이상인지 확인한다.

```
public class Crew {
    @Min(value = 20)
    private int age;
}
```

### @Max

- 필드 값이 value 값 이하인지 확인한다.

```
public class Crew {
    @Max(value = 40)
    private int age;
}
```

## Controller

- RequestBody에 `@Valid` 어노테이션을 적용하면 Bean Validation을 사용할 수 있다.

```
@PostMapping("/{lineId}/sections")
public ResponseEntity<SectionResponse> createSection(@PathVariable Long lineId,
                                                     @Valid @RequestBody SectionRequest sectionRequest) {
    Section addedSection = sectionService.add(lineId, sectionRequest);
    SectionResponse sectionResponse = new SectionResponse(addedSection);
    return ResponseEntity
            .created(URI.create("/lines/" + lineId + "/sections/" + addedSection.getId()))
            .body(sectionResponse);
}
```

## References

- [Validating Form Input](https://spring.io/guides/gs/validating-form-input/)
- [Java Bean Validation Basics](https://www.baeldung.com/javax-validation)
- [[Spring Boot] @NotNull, @NotEmpty, @NotBlank 의 차이점 및 사용법](https://sanghye.tistory.com/36)