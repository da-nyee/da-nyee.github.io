---
title: '[MySQL] Lock 이모저모'
author: da-nyee
date: 2022-06-30 02:09:11 +0900
categories: [TIL, MySQL]
tags: [mysql, innodb, lock]
---

<b>InnoDB 스토리지 엔진을 기준</b>으로 설명한다.<br/>

<br/>

## 읽기 잠금

- Shared Lock(S Lock)
- T1 트랜잭션은 r1 행을 읽을 때 S Lock을 획득한다.
- T2 트랜잭션은 해당 행을 읽을 수 있지만(이때도 S Lock을 획득), 쓸 수는 없다.
- 어떤 행에 S Lock이 걸리면, 해당 행에 S Lock은 걸 수 있고 X Lock은 걸 수 없다.(한 행에 여러 S Lock을 걸 수 있다.)

<br/>

## 쓰기 잠금

- Exclusive Lock(X Lock)
- T1 트랜잭션은 r1 행에 쓸 때 X Lock을 획득한다.
- T2 트랜잭션은 해당 행을 읽을 수 없고, 쓸 수도 없다. T1이 커밋/롤백할 때까지(X Lock을 풀 때까지) 기다려야 한다.
- 어떤 행에 X Lock이 걸리면, 해당 행에 S Lock과 X Lock을 모두 걸 수 없다.

<br/>

## 비관적 잠금

- Pessimistic Lock
- <b>SELECT ... FOR UPDATE</b> 쿼리
- 어떤 행을 읽고 쓰는 쿼리이다.
- 어떤 트랜잭션이 해당 행에 대해 X Lock을 걸어, 해당 트랜잭션이 끝날 때까지 다른 트랜잭션은 접근할 수 없다.
- 쉽게 <b>\`이 행은 내꺼니까 연산이 끝날 때까지 접근할 수 없어\`</b>로 생각할 수 있다.
- 비관적 잠금은 <b>동시성 문제를 해결할 수 있는 방안 중 하나</b>이다.
- 데이터 안정성을 확보할 수 있지만, 성능에 영향을 줄 수 있다.

<img width="706" src="https://user-images.githubusercontent.com/50176238/176254784-fc53c98f-611e-451d-a458-04f133ed9d71.png">

<br/>

## 미세팁

- InnoDB 스토리지 엔진에서는 Gap Lock과 Next Key Lock으로 Repeatable Read 격리 수준에서도 Phantom Read가 발생하지 않는다.
따라서 Serializable 격리 수준까지 올리지 않아도 된다.
- 하지만 SELECT ... FOR UPDATE 쿼리를 쓰면 Repeatable Read 격리 수준에서도 Phantom Read가 발생할 수 있다.(예외적인 상황)
- SELECT ... FOR UPDATE 쿼리는 SELECT하는 행에 X Lock을 걸어야 한다.
- 그런데 Undo Log의 행에는 잠금을 걸 수 없다.
- 결국 해당 쿼리로 가져오는 행은 Undo Log의 변경 전 데이터가 아니고, 현재 테이블의 데이터여서 Phantom Read가 나타날 수 있다.

<br/>

## References

- [MySQL 공식문서 - 15.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
- [MySQL 공식문서 - 15.7.2.4 Locking Reads](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html)
- [Real MySQL 8.0](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791158392703)
- [우리 프로젝트에서 동시성 이슈를 해결하는 방법 - 3. 비관적 잠금](https://wannte.tistory.com/22)