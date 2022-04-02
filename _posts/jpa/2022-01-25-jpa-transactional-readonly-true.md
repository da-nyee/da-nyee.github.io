---
title: '[JPA] @Transactional(readOnly = true)'
author: da-nyee
date: 2022-01-25 02:01:11 +0900
categories: [TIL, JPA]
tags: [jpa, transactional, readonly]
---

읽기 연산으로만 이루어진 서비스 로직에 붙이는 @Transactional(readOnly = true).<br/>
어떤 특징을 가지고 있는지 알아보자 !<br/>

<br/>

## 읽기 전용 트랜잭션 사용

- 스프링 프레임워크는 <b>트랜잭션에 readOnly = true 옵션</b>이 있으면, 하이버네이트 세션의 <b>플러시 모드를 MANUAL로 설정한다.</b>
- 이렇게 하면 플러시를 강제로 호출하지 않는 한, 플러시가 일어나지 않는다.
- 따라서 트랜잭션을 커밋해도 영속성 컨텍스트를 플러시하지 않는다.
- 그러므로 엔티티의 등록, 수정, 삭제(쓰기 연산)은 동작하지 않는다.
- 한편, 플러시할 때 일어나는 스냅샷 비교와 같은 무거운 로직을 수행하지 않아 성능이 향상된다.
- 트랜잭션을 시작했으므로 트랜잭션 시작, 로직 수행, 트랜잭션 커밋의 과정은 진행된다.
- 단지 영속성 컨텍스트를 플러시하지 않을 뿐이다.

<br/>

## 플러시 모드 설정

- 엔티티 매니저의 플러시 모드에는 AUTO, COMMIT만 있고, MANUAL은 없다.
- 반면, 하이버네이트 세션(org.hibernate.Session)의 플러시 모드에는 MANUAL이 있다.
- 이는 플러시를 강제로 호출하지 않으면 플러시가 절대 일어나지 않는다.
- 참고로 JPA 엔티티 매니저(인터페이스)를 하이버네이트(구현체)가 구현했다.
- 엔티티 매니저의 unwrap()을 호출하면 하이버네이트 세션을 구할 수 있다.

```java
Session session = entityManager.unwrap(Session.class);
```

<br/>

## Reference

- [자바 ORM 표준 JPA 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788960777330&orderClick=LEA&Kc=)