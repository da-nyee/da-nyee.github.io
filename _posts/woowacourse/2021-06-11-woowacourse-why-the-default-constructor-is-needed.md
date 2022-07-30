---
title: '[우아한테크코스] 기본 생성자가 필요한 이유 (Why the default constructor is needed) (feat. Jackson ObjectMapper + Reflection)'
author: da-nyee
date: 2021-06-11 20:56:26 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, default constructor, jackson, objectmapper, reflection]
---

## 개요

이번 방학 기간 동안 일부 크루들과 [회고 프로젝트](https://github.com/woowacourse-moltudy/15-mins-retrospective)를 진행하고 있다.<br/>
각자 구현하다 논의하고 싶은 사항이 생기면 issue를 등록한다.<br/>
오늘 `ObjectMapper를 위한 기본 생성자를 사용할 것인지`에 대한 이슈가 올라왔다.<br/>

<img width="940" alt="issue" src="https://user-images.githubusercontent.com/50176238/121677235-92df4680-caf0-11eb-83b5-1fb0cebdcae9.png">

나는 우선 아래와 같이 답변을 달았다.<br/>

<img width="940" alt="comment" src="https://user-images.githubusercontent.com/50176238/121677268-9d014500-caf0-11eb-8d0f-c24fff66183a.png">

내가 남긴 의견이지만,<br/>
아직 Reflection이 무엇인지도 모르는 상태여서 ObjectMapper와 Reflection에 관해 찾아보기로 했다.<br/>
또, 기본 생성자가 진짜 필요한가? 궁금증이 들기도 했고!<br/>

오후 내내 이 의문에 꽂혀서 한 3시간 찾아봤는데, 내용을 정리할 겸 포스팅으로 기록한다.<br/>

<br/>

## 사전 지식

### 직렬화/부호화

- Serialize
- Object to JSON
- `getter`를 사용한다.

### 역직렬화/복호화

- Deserialize
- JSON to Object
- `default constructor`를 사용한다. 👉 불변 객체로 못 만든다.

<br/>

## 생각 흐름

그럼 requestDto는 deserialize 과정이 필요하니까 가변적으로 만들어도,<br/>
responseDto는 serialize 과정이 필요하니까 불변적으로 만들어도 괜찮지 않을까?<br/>

마침 너잘이 이번 이슈에 아래와 같은 코멘트를 작성한 게 생각나서 연락했다!<br/>

<img width="939" alt="request-mutable-response-immutable" src="https://user-images.githubusercontent.com/50176238/121679390-3fbac300-caf3-11eb-8803-842ba0050116.png">

동시에 responseDto에 기본 생성자를 삭제하고, 미리 작성해둔 Controller 테스트를 실행했다.<br/>
결과는? 테스트가 잘 통과했다!<br/>

그런데, 너잘과 연락하며 인수 테스트에서 `???.as(??.class)`와 같은 코드를 자주 사용해서 기본 생성자가 필요한 걸 알았다.<br/>
해당 코드는 Java Reflection을 활용한 코드이다.<br/>

<br/>

## Reflection

Reflection은 접근 제어자와 상관 없이 클래스 객체를 동적으로 생성하는(런타임 시점) Java API이다.<br/>
참고로, Java는 정적 언어이다. 따라서, 컴파일 시점에 객체 타입을 결정한다.<br/>

Reflection과 기본 생성자에 관해 찾아보다 이런 문장을 마주쳤다.<br/>

> Almost all frameworks require a default(no-argument) constructor in your class because these frameworks use reflection to create objects by invoking the default constructor

Reflection은 무조건 기본 생성자가 필요하다는 내용이다.<br/>
왜냐하면 기본 생성자로 객체를 생성하기 때문이다.<br/>

왜 기본 생성자로 생성하지? 궁금해서 더 찾아봤다.<br/>

> Java Reflection 이란?<br/>
구체적인 클래스 타입을 알지 못해도, 그 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API

> 자바에서 제공하는 리플렉션(Reflection)은 C, C++과 같은 언어를 비롯한 다른 언어에서는 볼 수 없는 기능이다.
이미 로딩이 완료된 클래스에서 또 다른 클래스를 동적으로 로딩(Dynamic Loading)하여 생성자(Constructor), 멤버 필드(Member Variables) 그리고 멤버 메서드(Member Method) 등을 사용할 수 있도록 한다.

> java Reflection이 가져올 수 없는 정보 중 하나가 바로 생성자의 인자 정보들이다.
따라서 기본 생성자 없이 파라미터가 있는 생성자만 존재한다면 java Reflection이 객체를 생성할 수 없게 되는 것이다.

아하 그래서 기본 생성자가 필요하구나!<br/>
그런데, 생성자의 인자 정보를 못 가져오면 인자가 있는 생성자로 객체 생성을 어떻게 하지? 궁금해서 또 찾아봤다.<br/>

> Spring Data JPA 에서 Entity에 기본 생성자가 필요한 이유도 동적으로 객체 생성 시 Reflection API를 활용하기 때문이다.
Reflection API로 가져올 수 없는 정보 중 하나가 생성자의 인자 정보이다. 그래서 기본 생성자가 반드시 있어야 객체를 생성할 수 있는 것이다.
기본 생성자로 객체를 생성만 하면 필드 값 등은 Reflection API로 넣어줄 수 있다.

오호 기본 생성자로 객체를 생성하고,<br/>
클래스의 필드 정보를 가져올 수 있으니까 객체 생성 후에 필드 값을 할당하나 보다!<br/>

<br/>

## 결론

스스로 내린 결론은 인수 테스트까지 생각하면 아무래도 불변성 보장은 힘들 것 같다.<br/>
왜? Reflection은 기본 생성자가 필요하기 때문이다!<br/>

따라서, `private`로 빈 객체 생성(너잘이 우려한 문제)를 막는 게 최선이라 생각한다.<br/>
왜? Reflection은 접근 제어자와 상관 없기 때문이다!<br/>

<br/>

## References

- [ObjectMapper를 위한 기본 생성자를 사용할 것인가?](https://github.com/woowacourse-moltudy/15-mins-retrospective/issues/25)
- [Deserialize json with Java parameterized constructor](https://blogs.jsbisht.com/blogs/2016/09/12/deserialize-json-with-java-parameterized-constructor)
- [Java Reflection API에 대하여 (JPA에서 기본 생성자가 반드시 필요한 이유)](https://1-7171771.tistory.com/123?category=865872)
- [Reflection API 간단히 알아보자.](https://woowacourse.github.io/javable/post/2020-07-16-reflection-api/)