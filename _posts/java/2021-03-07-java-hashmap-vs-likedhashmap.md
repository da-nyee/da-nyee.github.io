---
title: '[Java] HashMap vs LinkedHashMap'
author: da-nyee
date: 2021-03-07 23:50:26 +0900
categories: [TIL, Java]
tags: [java, hashmap, linkedhashmap]
---

## 공통점

- 데이터를 <b>key-value 쌍</b>으로 저장한다.
- 비동기로 처리된다.

### Key

- <b>중복</b>을 <b>허용하지 않는다.</b>
- 하나의 null 값을 저장할 수 있다.

### Value

- <b>중복</b>을 <b>허용한다.</b>
- 여러 개의 null 값을 저장할 수 있다.

<br/>

## 차이점

### HashMap

- 데이터의 <b>삽입 순서</b>를 <b>보장하지 않는다.</b>
- AbstractMap 클래스를 상속하고, Map 인터페이스를 구현한다.

### LinkedHashMap

- 데이터의 <b>삽입 순서</b>를 <b>보장한다.</b>
- HashMap 클래스를 상속하고, Map 인터페이스를 구현한다.

<br/>

## 성능

### Create

- HashMap < LinkedHashMap (오래 걸림)

### Iterate

- (오래 걸림) HashMap > LinkedHashMap

### Access

- (오래 걸림) HashMap > LinkedHashMap

<br/>

## 결론

- HashMap보다 <b>LinkedHashMap의 성능이 약간 더 우세</b>하지만, <b>전체적인 성능에는 큰 차이가 없다.</b>
- <b>HashMap은 순서를 보장하지 않아도</b> 될 때, <b>LinkedHashMap은 순서를 보장해야</b> 될 때 사용한다.

<br/>

## References

- [LinkedHashMap vs HashMap](https://www.javatpoint.com/linkedhashmap-vs-hashmap-in-java)
- [JAVA HashMap VS LinkedHashMap (차이점, 성능차이, 사용방법)](https://web-inf.tistory.com/44)