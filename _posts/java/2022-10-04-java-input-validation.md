---
title: '[Java] 객체지향적으로 입력 유효성 검증하기'
author: da-nyee
date: 2022-10-04 03:03:40 +0900
categories: [TIL, Java]
tags: [java, input validation]
---

스프링에 의존하지 않고, 즉 bean validation을 사용하지 않고 <b>입력 유효성을 검증</b>할 수 있는 방법은 없을까?<br/>
물론 있다.<br/>
조건문을 나열해서 확인할 수도 있고, 유효성 검증을 위한 객체를 생성해서 확인할 수도 있다.<br/>

이번 글에서는 <b>유효성 검증을 위한 객체를 생성해서 확인하는 방법</b>을 위주로 살펴보려고 한다.<br/>
단순한 로직이라면 조건문을 나열하는 방법이 빠르고 쉬울 수 있다.<br/>
하지만 <b>유지보수</b>와 <b>확장성</b>의 관점에서, 아무리 단순한 로직이더라도 유효성 검증을 위한 객체를 생성하는 것이 더 좋다고 생각한다.<br/>

<br/>

## 절차지향적인 코드

절차지향적으로 코드를 작성하면 어떤 문제가 있을까?<br/>

유효성 검증 로직이 간단한 경우에는 딱히 문제가 없을 수 있다.<br/>
하지만 유효성 검증 로직이 조금이라도 복잡해지면, 새로운 로직을 추가하거나 기존 로직을 수정할 때 어려움을 겪을 수 있다.<br/>

이 방법은 코드의 전체적인 가독성을 낮추고, 개발자가 실수할 가능성을 높인다.<br/>
결국 유지보수하기 어려운 코드로 만든다.<br/>

```java
public class Validator {

    public void validate(Request request) {
        if (request == null) {
            // Do something
        }
        if (request.getA()) {
            // Do something
        }
        if (request.getB()) {
            // DO something
        }
        if (request.getA() > 0) {
            // Do something
        }

        ...
    }
}
```

<br/>

## 객체지향적인 코드

한편, 객체지향적으로 코드를 작성하면 어떨까?<br/>

설명을 위해 간단한 `회원가입` 프로젝트를 생성했다.<br/>
자세한 코드는 [여기](https://github.com/da-nyee/playground/tree/main/src/main/java/validation)를 확인하자.<br/>

회원가입 정책은 다음과 같다.<br/>
▪️ `개인유저`와 `기업유저`가 가입할 수 있다.<br/>
▪️ 개인유저는 `아이디, 비밀번호, 이름`이 필수값이다.<br/>
▪️ 기업유저는 `아이디, 비밀번호`가 필수값이다.<br/>

<img width="246" alt="image" src="https://user-images.githubusercontent.com/50176238/192134006-c35f65c3-54f9-4c66-a32d-833d5a1c68eb.png">

<br/>

객체 의존관계는 다음과 같다.<br/>

<img width="530" alt="image" src="https://user-images.githubusercontent.com/50176238/192134303-666630b0-9137-4c2c-b3ce-c118f4e5cdad.png">

<br/>

각 필드마다 validator 클래스를 만들고, 모든 validator를 하나로 묶는 인터페이스로 validator를 한 단계 추상화한다.<br/>
인터페이스는 validate()와 isSatisfiedBy()를 가지고, 구현체는 유효성 검증 로직과 유효성 검증 조건을 구현한다.<br/>

isSatisfiedBy()를 예로 들면, `아이디`는 모든 유저에서 유효성을 검증하므로 무조건 true를 반환한다.<br/>
한편 `이름`은 개인유저에서만 유효성을 검증하므로 개인유저면 true, 기업유저면 false를 반환한다.<br/>

```java
// SignUpValidator.java
public interface SignUpValidator {

    List<InvalidResponse> validate(ValidationRequest request);

    boolean isSatisfiedBy(boolean isCompany);
}

// IdValidator.java
@Component
public class IdValidator implements SignUpValidator {

    private static final InvalidResponse INVALID_RESPONSE = new InvalidResponse("id", "유효하지 않은 id");

    @Override
    public List<InvalidResponse> validate(ValidationRequest request) {
        String id = request.getId();

        if (id == null || isBlank(id)) {
            return Collections.singletonList(INVALID_RESPONSE);
        }

        if (isLowercaseOrNumber(id)) {
            return Collections.emptyList();
        }
        return Collections.singletonList(INVALID_RESPONSE);
    }

    private boolean isBlank(String id) {
        return id.trim().isEmpty();
    }

    private boolean isLowercaseOrNumber(String id) {
        return id.matches("[a-z0-9]+");
    }

    @Override
    public boolean isSatisfiedBy(boolean isCompany) {
        return true;
    }
}

// PasswordValidator.java
...

// NameValidator.java
@Component
public class NameValidator implements SignUpValidator {

    private static final InvalidResponse INVALID_RESPONSE = new InvalidResponse("name", "유효하지 않은 name");

    @Override
    public List<InvalidResponse> validate(ValidationRequest request) {
        String name = request.getName();

        if (name == null || isBlank(name)) {
            return Collections.singletonList(INVALID_RESPONSE);
        }

        if (isKorean(name)) {
            return Collections.emptyList();
        }
        return Collections.singletonList(INVALID_RESPONSE);
    }

    private boolean isBlank(String name) {
        return name.trim().isEmpty();
    }

    private boolean isKorean(String id) {
        return id.matches("[가-힣]+");
    }

    @Override
    public boolean isSatisfiedBy(boolean isCompany) {
        return !isCompany;
    }
}
```

<br/>

모든 validator를 완성했다면, 필드 validator를 사용하는 클라이언트 클래스를 생성한다.<br/>
이 클래스는 List로 validator를 조합한다.<br/>
validate()에서 stream을 흘려 어떤 validator에게 유효성 검증을 위임할지 결정한다.(filter)<br/>
유효성 검증을 하고(map), 전체 유효성 검증 결과를 모아서 반환한다.(collect)<br/>

```java
// SignUpValidation.java
@Component
@RequiredArgsConstructor
public class SignUpValidation {

    private final List<SignUpValidator> validators;

    public List<InvalidResponse> validate(ValidationRequest request) {
        if (request == null) {
            return Collections.singletonList(new InvalidResponse("request", "유효하지 않은 request"));
        }

        return validators.stream()
            .filter(validator -> validator.isSatisfiedBy(request.isCompany()))
            .map(validator -> validator.validate(request))
            .flatMap(Collection::stream)
            .collect(Collectors.toList());
    }
}
```

<br/>

## 만약 중복되는 메서드가 있다면?

추가적으로, 모든 validator에 중복되는 메서드가 있다면 어떻게 해야 할까?<br/>
이때는 인터페이스와 구현체 사이에 <b>추상 메서드</b>를 둬서 해결할 수 있다.<br/>

단, 클래스는 단 1번만 상속할 수 있기 때문에 최대한 인터페이스를 활용하는 걸 추천한다.<br/>
구현은 n번 할 수 있다.<br/>

```java
public interface Validator {

    void duplicateValidate();
}

public abstract class AAAValidator implements Validator {

    @Override
    protected void duplicateValidate() {
        ...
    }
}

public class BBBValidator extends AAAValidator {
    ...
}

public class CCCValidator extends AAAValidator {
    ...
}
```

<br/>

## 결론

이번 예제를 만들면서 좀 더 <b>확장에 유연하고 응집도가 높은</b> 코드를 작성하기 위해 노력했다.<br/>

필드 validator는 현재 3개가 있지만, 기획이 변경되면 새로운 validator가 추가될 수 있다.<br/>
이때 상위 인터페이스를 구현하면 변경에 쉽게 대처할 수 있다.<br/>

각 validator는 특정 필드 입력값에 대한 유효성 검증이라는 책임만 다하면 되기 때문에 응집도가 높다.<br/>
클라이언트 클래스는 모든 validator를 사용하지만, 인터페이스에 의존하기 때문에 직접적인 결합도는 낮다.<br/>

결과적으로 절차지향적인 코드보다 <b>유지보수하기 좋고 가독성이 뛰어난</b> 코드가 됐다.<br/>

물론 이 방식이 최선은 아닐 수 있다.<br/>
그렇지만, 한 번쯤 활용해봐도 좋을 것 같다.<br/>