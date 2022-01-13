---
title: '[Distributed Systems] CAP 정리 및 BASE (CAP Theorem and BASE)'
author: da-nyee
date: 2022-01-14 00:14:22 +0900
categories: [TIL, Distributed Systems]
tags: [distributed systems, cap theorem, base]
---

## CAP Theorem?

- CAP 정리는 <b>분산 시스템이 C, A, P 성질을 다 만족할 수는 없다</b>는 의미이다.
- 현실에서는 네트워크 장애가 발생할 수 밖에 없다.
- 따라서 C 또는 A 중에 하나를 선택하고, P는 기본으로 가져간다.

### Consistency

- 일관성
- 클라이언트가 어떤 노드와 연결되었는지 상관없이, 즉 다수의 노드로부터 동일한 응답을 얻는다.
- 데이터가 한 노드에 기록될 때, 해당 데이터가 <b>쓰기 성공</b>으로 간주되기 전에 다른 노드로 즉시 전달/복제돼야 한다.

### Availability

- 가용성
- 클라이언트가 데이터를 요청했을 때, 하나 이상의 노드에 문제가 있더라도 응답을 정상적으로 받는다.
- 모든 <b>작업 중인</b> 노드는 예외없이 모든 요청에 대해 유효한 응답을 반환해야 한다.

### Partition Tolerance

- 분할 허용
- 네트워크 장애로 인해 두 노드 간의 통신이 불가능한 상황에서도 분산 시스템은 계속해서 작동해야 한다.
- 가용성이 <b>특정 노드</b>에 대한 것이라면, 분할 허용은 <b>특정 연결</b>에 관한 것이다.

![cap-theorem](https://user-images.githubusercontent.com/50176238/149297118-ae5d3472-871c-4e60-b5b6-74fa64a6a2d5.png)

<br/>

## BASE?

- BASE는 분산 시스템이 <b>성능과 가용성</b>을 위해 고려하는 속성이다.
- CAP 정리에서 AP 분산 시스템과 유사하다.

### Basically Available

- 분산 시스템은 항상 요청에 응답한다.
- 그러나, 매번 응답이 올바른 건 아니다.

### Soft-state

- 분산 시스템의 상태는 언제든지 변경된다.
- 클라이언트가 별도로 관리하여 상태를 유지할 수 있다.

### Eventually Consistency

- 분산 시스템의 일관성은 일시적으로 깨져도, 최종적으로는 보장된다.
- 데이터 업데이트가 한 노드에만 적용되고 다른 노드에는 전달되지 않았다면, 일관성이 잠시 무너진다. 시간이 흐른 뒤에는 모든 노드에 반영되어 일관성을 다시 가져간다.

<br/>

## References

- [CAP 정리란?](https://www.ibm.com/kr-ko/cloud/learn/cap-theorem)
- [CAP, ACID, BASE](https://ssup2.github.io/theory_analysis/CAP_ACID_BASE/)
- [NoSQL에 대해서 간단히 알아보자!](https://embian.wordpress.com/2013/06/27/nosql-2/)