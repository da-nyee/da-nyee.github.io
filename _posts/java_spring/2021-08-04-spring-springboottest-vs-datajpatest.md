---
title: '[Spring] @SpringBootTest vs @DataJpaTest'
author: da-nyee
date: 2021-08-04 02:02:51 +0900
categories: [TIL, Spring]
tags: [spring, springboottest, datajpatest]
---

## @SpringBootTest

- ApplicationContext에 모든 Bean들을 등록한다.
- SpringBoot 어플리케이션을 실행했을 때와 동일하게 컨테이너에 Bean들을 등록한다.

```
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@DirtiesContext(classMode = ClassMode.BEFORE_EACH_TEST_METHOD)
class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    ...
}
```

<br/>

## @DataJpaTest

- ApplicationContext에 JPA에 필요한 설정들만 등록한다.
- 기본적으로 in-memory embedded DB(ex. H2)를 사용한다.
- Component Scan을 하지 않아 컨테이너에 `@Component` 빈들이 등록되지 않는다.

```
@DataJpaTest
@DirtiesContext(classMode = ClassMode.BEFORE_EACH_TEST_METHOD)
class UserServiceIntegrationTest {

    // @Service(@Component) 어노테이션이 붙어 있는 클래스
    // 따라서, @DataJpaTest에서 @Autowired 할 수 X
    private UserService userService;

    @Autowired
    private UserRepository userRepository;

    @BeforeEach
    void setUp() {
        userService = new UserService(userRepository);
    }

    ...
}
```

<br/>

## Comparison

- @SpringBootTest는 `@Component` 어노테이션이 붙은 클래스를 사용해야 하는 경우에 활용하자.
- @DataJpaTest는 위의 경우가 아닐 때 고려해보자. 👉 ApplicationContext에 `@Component` 빈들을 등록하지 않아 테스트 실행 속도가 빨라진다.

<br/>

## Performance

실제로 약 10개의 테스트 코드로 성능 테스트를 해봤는데,<br/>

- @SpringBootTest는 12sec 447ms
- @DataJpaTest는 823ms

테스트 실행 속도에 약 11초 정도의 차이가 있었다.<br/>

<br/>

## References

- [[#232] 유저 테스트 코드 리팩토링](https://github.com/woowacourse-teams/2021-pick-git/pull/249#discussion_r676153817)
- [40. Testing](https://docs.spring.io/spring-boot/docs/1.4.2.RELEASE/reference/html/boot-features-testing.html)
- [@SpringBootTest와 @DataJpaTest 차이점](https://krksap.tistory.com/1013)
- [@SpringBootTest Vs @WebMvcTest & @DataJpaTest &service unit tests, what is the best?](https://stackoverflow.com/questions/59097035/springboottest-vs-webmvctest-datajpatest-service-unit-tests-what-is-the-b)