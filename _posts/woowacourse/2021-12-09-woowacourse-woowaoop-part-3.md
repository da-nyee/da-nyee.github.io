---
title: '[우아한테크코스] 우아한객체지향 - Part 3'
author: da-nyee
date: 2021-12-09 02:15:30 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, seminar, oop]
---

## 객체를 탐색하는 방법

### 객체를 참조한다.

- 강한 결합도
- 해당 방식은 Order에 있는 Shop을 통해 Shop을 바로 탐색할 수 있다.

![high-coupling](https://user-images.githubusercontent.com/50176238/145251707-69080ae8-bd00-4a16-8e54-3afcd2f7aed6.png)

### Repository를 사용한다.

- 약한 결합도
- 해당 방식은 ShopRepository의 findById()를 통해 Shop을 탐색할 수 있다.

![low-coupling](https://user-images.githubusercontent.com/50176238/145252029-b81491e2-9636-4adc-941e-288fa3c24d1a.png)

<br/>

## 패키지 사이의 의존성 사이클을 제거하는 방법

### 새로운 객체로 변환한다.

- 기존에는 Shop <-> Order 양방향 참조를 하고 있다.
- 이를 <b>중간 객체</b>(e.g. OptionGroup, Option)를 이용해서 의존성 사이클을 끊는다.
- 이후에는 Shop <- Order 단방향 참조를 하게 된다.

![package-with-cycle-1](https://user-images.githubusercontent.com/50176238/145238721-25e48e16-21f4-4de3-aaba-934a04463cda.png)
![package-without-cycle-1](https://user-images.githubusercontent.com/50176238/145245532-cbfd2ab3-1ed4-4dab-a7cf-560168ecdc87.png)

### 의존성을 역전한다. (DIP)

- 기존에는 Order <-> Delivery 양방향 참조를 하고 있다.
- 이를 <b>인터페이스와 구현체로 분리</b>해서 의존성 사이클을 없앤다.
- 이후에는 Order <- Delivery 단방향 참조를 하게 된다.

![package-with-cycle-2](https://user-images.githubusercontent.com/50176238/145247791-8c977ead-02de-4214-8ce1-35760f5dec01.png)
![package-without-cycle-2](https://user-images.githubusercontent.com/50176238/145248023-7553f9da-4f56-4939-bea2-c1c50ec1510e.png)

### 새로운 패키지를 추가한다.

- 기존에는 Shop <-> Order 양방향 참조를 하고 있다.
- 이는 Shop 패키지에 ShopEventHandler 클래스가 위치하고 있어 발생한다.
- 이를 <b>다른 패키지</b>(e.g. Billing)로 이동하여 의존성 사이클을 해결한다.
- 이후에는 Shop <- Order 단방향 참조, Order <- Billing 단방향 참조를 하게 된다.

![package-with-cycle-3](https://user-images.githubusercontent.com/50176238/145249184-689cf8ff-f25a-4877-8a19-db86236ef57f.png)
![package-without-cycle-3](https://user-images.githubusercontent.com/50176238/145249474-3594eb94-d9a9-4c30-a9e5-7b3f47df3111.png)

<br/>

## Reference

- [[우아한테크세미나] 190620 우아한객체지향 by 우아한형제들 개발실장 조영호님](https://www.youtube.com/watch?v=dJ5C4qRqAgA&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech)