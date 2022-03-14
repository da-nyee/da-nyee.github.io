---
title: '[Spring] DAO, DTO, VO'
author: da-nyee
date: 2020-10-26 00:39:22 +0900
categories: [TIL, Spring]
tags: [spring, dao, dto, vo]
---

## DAO

- `DAO(Data Access Object)`는 DB의 데이터에 접근하기 위한 객체이다.
- DB 접근 로직 / 비즈니스 로직을 분리하기 위한 용도로 사용한다.
- DB에 접속하여 데이터 CRUD(Create, Read, Update, Delete) 작업을 시행하는 클래스이다.
- DB 접근 로직은 코드 간결화, 모듈화, 유지보수 등의 목적을 위해 별도로 생성하는 것이 좋다.

### Additional Information 1

- DAO는 단일 데이터 접근, 갱신 개념이다.
- Service는 하나 이상의 DAO를 이용하여 비즈니스 로직을 처리한다. ➡ 트랜잭션 단위라고 생각하면 된다.

### Additional Information 2

- `Repository`는 JPA를 사용할 때 DAO의 역할을 대신한다.
- JPA와 같은 ORM을 사용하면 객체(Entity) 단위로 테이블을 관리한다.

## DTO

- `DTO(Data Transfer Object)`는 각 계층간 데이터를 교환하기 위한 객체이다.
- 어떠한 로직도 가지지 않는 순수한 데이터 객체이다.
- getter, setter 메소드만 가지고 있는 클래스이다.
- setter를 활용하기 때문에 `가변(mutable)의 성격`을 가지고 있는 클래스이다.

## VO

- `VO(Value Object)`도 각 계층간 데이터를 교환하기 위한 객체이다. ➡ DTO와 동일한 역할을 한다.
- 어떠한 로직도 가지지 않는 순수한 데이터 객체이다.
- getter 메소드만 가지고 있는 클래스이다.
- setter를 활용하지 않기 때문에 `불변(immutable)의 성격`을 가지고 있는 클래스이다.

## References

- [[JAVA] DAO, DTO, VO 차이](https://lemontia.tistory.com/591)
- [DAO, DTO(VO) 란 무엇일까?](https://iri-kang.tistory.com/5)
- [DAO와 Repository / DTO / VO](https://velog.io/@leyuri/DAO%EC%99%80-Repository-DTO-VO)