---
title: '[Spring] @Transactional(rollbackFor={exceptionClass})'
author: da-nyee
date: 2022-05-23 04:03:30 +0900
categories: [TIL, Spring]
tags: [spring, transactional, rollbackfor]
---

스프링에서 `@Transactional`을 사용하면 기본적으로 모든 예외에 대해 롤백하는 줄 알았다.<br/>
근데 아니었다.<br/>

@Transactional에는 `rollbackFor` 옵션이 있다.<br/>
요 옵션의 주석을 보면 다음과 같다.<br/>

> By default, a transaction will be rolling back on RuntimeException and Error but not on checked exceptions (business exceptions). See org.springframework.transaction.interceptor.DefaultTransactionAttribute.rollbackOn(Throwable) for a detailed explanation.

기본적으로 `RuntimeException`과 `Error`에 대해서만 롤백하고, `Exception`에 대해서는 롤백하지 않는다.<br/>
즉 unchecked exception이 발생하면 롤백하고, checked exception이 발생하면 커밋한다.<br/>

<br/>

주석을 따라 `DefaultTransactionAttribute` 클래스를 확인했다.<br/>
요 클래스에는 많은 메소드가 있는데, 그 중 `rollbackOn()`의 주석을 읽어봤다.<br/>

> The default behavior is as with EJB: rollback on unchecked exception
({@link RuntimeException}), assuming an unexpected outcome outside of any
business rules. Additionally, we also attempt to rollback on {@link Error} which
is clearly an unexpected outcome as well. By contrast, a checked exception is
considered a business exception and therefore a regular expected outcome of the
transactional business method, i.e. a kind of alternative return value which
still allows for regular completion of resource operations.

> This is largely consistent with TransactionTemplate's default behavior,
except that TransactionTemplate also rolls back on undeclared checked exceptions
(a corner case). For declarative transactions, we expect checked exceptions to be
intentionally declared as business exceptions, leading to a commit by default.

결국 unchecked exception은 비즈니스상 예상하지 못해 롤백하고, checked exception은 의도했을 수 있어 커밋한다.<br/>

<br/>

여기까지 @Transactional의 기본적인 롤백 전략을 이해했다.<br/>

요 전략이 실제로 잘 적용되는 건지 궁금했다.<br/>
직접 코드를 통해 확인하고 싶었다.<br/>
그래서 학습 테스트를 진행했다.<br/>

학습 테스트는 아래에서 볼 수 있다.<br/>

<br/>

## 학습 테스트

### 코드 작성

Member 엔티티를 생성했다.<br/>

```java
// Member.java
@Entity
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    protected Member() {
    }

    public Member(String name) {
        this.name = name;
    }

    // getter
}
```

<br/>

MemberController에 Member를 저장하는 API를 만들었다.<br/>

```java
// MemberController.java
@RestController
public class MemberController {

    private final MemberService memberService;

    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @PostMapping("/api/members")
    public ResponseEntity<Void> save(String name) throws Exception {
        memberService.save(name);
        return ResponseEntity.ok().build();
    }
}
```

<br/>

MemberService에 각 예외를 분기 처리하는 로직을 구현했다.<br/>

```java
// MemberService.java
@Service
@Transactional
public class MemberService {

    private static final Logger LOG = LoggerFactory.getLogger(MemberService.class);

    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public void save(String name) throws Exception {
        Member member = new Member(name);
        memberRepository.save(member);

        // 예외는 아무거나 던졌다.
        if ("커밋".equals(name)) {
            LOG.info("[Exception] member 저장 성공 - 커밋 O, 롤백 X");
            throw new ActionException();
        }
        if ("롤백 1".equals(name)) {
            LOG.info("[RuntimeException] member 저장 실패 - 커밋 X, 롤백 O");
            throw new IllegalStateException();
        }
        if ("롤백 2".equals(name)) {
            LOG.info("[Error] member 저장 실패 - 커밋 X, 롤백 O");
            throw new IllegalAccessError();
        }
        LOG.info("member 저장 성공 - 커밋 O, 롤백 X");
    }
}
```

<br/>

MemberRepository도 정의했다.<br/>

```java
// MemberRepository.java
public interface MemberRepository extends JpaRepository<Member, Long> {

}
```

<br/>

### 결과 확인

포스트맨에서 API를 호출하고, DB에서 결과를 확인했다.<br/>

<br/>

1. localhost:8080/api/members?name=다니
2. localhost:8080/api/members?name=롤백 1
3. localhost:8080/api/members?name=롤백 2
4. localhost:8080/api/members?name=커밋

주석 내용대로 unchecked exception은 롤백됐고, checked exception은 커밋됐다.<br/>
ID 값이 1, 4인 걸 보면 알 수 있다.<br/>

<img width="130" src="https://user-images.githubusercontent.com/50176238/169704579-58b8571a-c7a4-4be6-a151-1e415ff009ed.png">

<br/>

### rollbackFor 추가

어떤 예외가 발생해도 롤백하고 싶어 rollbackFor에 `Exception.class`를 지정했다.<br/>
Exception.class는 모든 예외의 상단에 위치해 있기 때문이다.<br/>

```java
@Service
@Transactional(rollbackFor = {Exception.class})
public class MemberService {

    ...
}
```

<br/>

1. localhost:8080/api/members?name=다니
2. localhost:8080/api/members?name=롤백 1
3. localhost:8080/api/members?name=롤백 2
4. localhost:8080/api/members?name=커밋
5. localhost:8080/api/members?name=데이지

요기서는 Exception.class를 롤백 조건에 명시하여 모든 예외에 대해 롤백이 일어났다.<br/>
ID 값이 1, 5인 것을 보면 알 수 있다.<br/>

<img width="136" src="https://user-images.githubusercontent.com/50176238/169704755-05fcd564-df6d-43dd-b90b-750603f1d7c2.png">

<br/>

## 결론

@Transactional의 기본적인 롤백 전략은 unchecked exception이다.<br/>
하지만 커스텀하게 변경할 수 있다.<br/>
특정 예외에 대해 롤백하고 싶다면, 해당 예외 클래스를 rollbackFor에 적어주면 된다.<br/>

새로운 사실을 알았으니 앞으로 잘 활용해야겠다.<br/>
요것과 관련한 이슈가 생겼을 때, 디버깅을 좀 더 쉽게 할 수 있을 것 같다.<br/>

<br/>

## References

- Transactional 내부 코드
- DefaultTransactionAttribute 내부 코드
- [Spring 학습 테스트](https://github.com/da-nyee/jpa-learning-test/tree/test4/rollback-for)
- [응? 이게 왜 롤백되는거지?](https://techblog.woowahan.com/2606/)