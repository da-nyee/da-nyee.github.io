---
title: '[우아한테크코스] 다니의 학습 로그 - 레벨 1'
author: da-nyee
date: 2021-04-22 01:06:38 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, learning log, level1]
---

> [로또 미션/step1](https://github.com/woowacourse/java-lotto/pull/248)

## [TDD] 테스트 주도 개발 - 5
### 내용
- 테스트가 가능한 클래스와 불가능한 클래스를 구분 지음
- 테스트가 가능한 클래스에 대해 TDD를 수행함
  - 먼저, 실패하는 테스트 코드를 작성함
  - 다음으로, 컴파일 에러가 나지 않을 정도의 프로덕션 코드를 작성함
  - 다음으로, 기능 요구사항 대로 결과가 나타나는 프로덕션 코드를 작성함
  - 마지막으로, 리팩토링을 통해 코드 컨벤션을 확인하고 클린 코드가 되도록 변경함

### 링크
- https://github.com/da-nyee/java-lotto/tree/step1/src/test/java/domain

## [Java] 일급 컬렉션 - 4
### 내용
- LottoTicket, LottoTickets 객체를 도출하여 `규칙 8: 일급 콜렉션을 쓴다.`를 지킴
- 지금까지 잘못 알고 있던 개념을 다시 확인하고 이해함

### 링크
- [일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412)
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/LottoTicket.java
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/LottoTickets.java

## [Java] 원시값 및 문자열 포장 - 5
### 내용
- LottoNumber, Money, Profit 객체를 도출하여 `규칙 3: 모든 원시값과 문자열을 포장한다.`를 지킴
- 지난 미션까지는 방법을 잘 몰랐으나, 이번 미션에서 직접 코드로 구현하며 습득함

### 링크
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/LottoNumber.java
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/Money.java
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/Profit.java

## [Java] Enum - 4
### 내용
- Rank 객체를 도출하여 `java enum을 적용해 프로그래밍을 구현한다.`를 지킴
- 여태 enum은 상수를 모아두는 용도로만 사용했는데, 이번 기회를 통해 enum의 다양한 활용 방식을 이해함

### 링크
- [Java Enum 활용기](https://woowabros.github.io/tools/2017/07/10/java-enum-uses.html)
- https://github.com/da-nyee/java-lotto/blob/step1/src/main/java/domain/Rank.java

## [Java] 스트림 - 5
### 내용
- 가능한 외부 반복자를 지양하고, 내부 반복자를 지향하려고 노력함
- 스트링에 익숙해지고자 여러 스트림 API를 사용함

### 링크
- [[Java] 스트림 (Stream)](https://da-nyee.github.io/posts/java-stream/)

## [Java] 제네릭 - 2
### 내용
- 런타임 시 제네릭 정보가 지워진다는 것을 알게 됨
- 따라서, 메소드 오버로딩이 불가능함

### 링크
- [Name clash: Two methods have the same erasure](https://coderanch.com/t/706161/java/clash-methods-erasure)

## [Java] 초기화 블럭 - 3
### 내용
- 초기화 블럭과 static 초기화 블럭의 존재를 알게 됨
- 둘의 차이를 이해함

### 링크
- [[Java-14]초기화와 초기화블럭(Initialize Block)](https://kamang-it.tistory.com/entry/Java-14%EC%B4%88%EA%B8%B0%ED%99%94%EC%99%80-%EC%B4%88%EA%B8%B0%ED%99%94%EB%B8%94%EB%9F%ADInitialize-Block)

<br/>

> [로또 미션/step2](https://github.com/woowacourse/java-lotto/pull/322)

## [Java] 문자열 - 5
### 내용
- 제이슨의 강의를 듣고 학습한 내용을 블로그에 정리함
- 문자열 객체를 new로 생성하는 것과 ""로 생성하는 것의 차이를 알게 됨
- String Pool, StringBuilder 등 새로운 개념들을 알게 됨

### 링크
- [[Java] 문자열 (String)](https://da-nyee.github.io/posts/java-string/)

## [Java] stream findAny(), findFirst() 차이 - 3
### 내용
- Rank 객체에서 등수에 대한 정보를 반환하기 위해 stream API를 활용함
- 원래는 findAny()를 썼는데, 테스트 도중 이 메소드가 매번 같은 값(ex. 첫번째 값) 반환을 보장하지 못한다는 사실을 알게 됨
- 반면, findFirst()는 항상 첫번째 값 반환을 보장하기 때문에 이 메소드를 사용하도록 변경함

### 링크
- [Difference between findAny() and findFirst() in java Stream API](https://medium.com/@sampath.katari/difference-between-findany-and-findfirst-in-java-stream-api-fb8684253648)

## [Java] HashMap vs EnumMap - 3
### 내용
- 얼마 전에 크루 한 명이 EnumMap의 존재와 이 구현체가 HashMap보다 성능이 좋다는 사실을 알려줌
- 두 구현체의 특징을 찾아보고 기존에 HashMap으로 작성했던 코드를 EnumMap으로 변경함
- EnumMap은 Key 값으로 무조건 enum 클래스를 가져야 한다는 조건을 알게 됨

### 링크
- [EnumMap(EnumSet) 쓰면 좋을까? (vs HashMap)](https://siyoon210.tistory.com/142)
- [[자료구조] 코드로 알아보는 java의 EnumMap](https://sabarada.tistory.com/131)

<br/>

> [블랙잭 미션/step1](https://github.com/woowacourse/java-blackjack/pull/139)

## [Java] HashMap vs LinkedHashMap - 2
### 내용
- HashMap과 LinkedHashMap 둘 다 Map의 특징을 지닌다는 공통점이 있음
- HashMap은 데이터의 삽입 순서를 보장하지 않는 반면, LinkedHashMap은 삽입 순서를 보장한다는 차이점이 있음
- HashMap은 AbstractMap 클래스를 구현하는 한편, LinkedHashMap은 HashMap을 구현한다는 차이점도 있음

### 링크
- [[Java] HashMap vs LinkedHashMap](https://da-nyee.github.io/posts/java-hashmap-vs-likedhashmap/)

## [Java] 무한 스트림 - 3
### 내용
- 무한 스트림은 iterate() 또는 generate()를 이용해서 생성할 수 있음
- 무한 스트림을 언바운드(unbounded) 스트림이라고 표현함
- 무한 스트림을 사용할 때는 limit()과 같은 쇼트서킷(short circuit) 연산이 필요함

### 링크
- [모던 자바 인 액션](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162242025&orderClick=LEa&Kc=)

<br/>

> [블랙잭 미션/step2](https://github.com/woowacourse/java-blackjack/pull/211)

## [Java] flatMap - 2
### 내용
- map은 각각 별도의 스트림을 생성해서 처리하는 반면, flatMap은 하나의 스트림을 생성해서 처리한다.
- flatMap을 사용하면 중첩 루프를 없앨 수 있다.

### 링크
- [모던 자바 인 액션](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162242025)
- [[Java] flatMap으로 중첩 루프 없애는 방법 (How to remove nested loops using flatMap)](https://da-nyee.github.io/posts/java-how-to-remove-nested-loops-using-flatmap/)
- [b3a0d3a](https://github.com/woowacourse/java-blackjack/pull/211/commits/b3a0d3ad1490c44af5df9fcd6930f98cb326efdc)

<br/>

> [체스 미션/step1](https://github.com/woowacourse/java-chess/pull/193)

## [Design Pattern] Command Pattern - 2.5
### 내용
<p align="center"><img src="https://user-images.githubusercontent.com/50176238/111971007-41519c00-8b3f-11eb-8856-1ce30b280403.PNG"></p>

- 체스 실행 명령들을 Command로 추상화
- 상태 패턴이랑 비슷하다는 느낌을 받음

### 링크
- [[디자인패턴] 커맨드 패턴 ( Command Pattern )](https://victorydntmd.tistory.com/295)
- [java-chess/src/main/java/chess/domain/command/](https://github.com/da-nyee/java-chess/tree/step1/src/main/java/chess/domain/command)

<br/>

> [체스 미션/step2](https://github.com/woowacourse/java-chess/pull/258)

## [Java] try-with-resources - 3
### 내용
- [[Java] try-with-resources](https://da-nyee.github.io/posts/java-try-with-resources/)

### 링크
- [java-chess/src/main/java/chess/dao/TurnDao.java/](https://github.com/da-nyee/java-chess/blob/step2/src/main/java/chess/dao/TurnDao.java)