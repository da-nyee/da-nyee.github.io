---
title: '[Java] Array, Vector, ArrayList, LinkedList'
author: da-nyee
date: 2020-10-28 23:48:02 +0900
categories: [TIL, Java]
tags: [java, data structures, array, vector, arraylist, linkedlist]
---

## Array

- 정적 길이 제공
- 크기를 한번 결정하면 변경할 수 없다.
- 데이터 **탐색에 유리**하다. (**Random Access**) ➡ O(1)
- 데이터 **추가, 삭제에 불리**하다. ➡ O(n)

## Vector

- 동적 길이 제공
- 동기화 기능 제공
- 크기를 한번 결정해도 변경할 수 있다. ➡ capacity를 초과하면 현재 크기의 100%가 증가한다.
- 데이터 **탐색에 유리**하다. ➡ O(1)
- 데이터 **추가, 삭제에 불리**하다. ➡ O(n)
- Java 1.0에서 추가되었다.

## ArrayList

- 동적 길이 제공
- 동기화 기능 미제공
- 크기를 한번 결정해도 변경할 수 있다. ➡ capacity를 초과하면 현재 크기의 50%가 증가한다.
- 데이터가 순서대로 늘어선 Array 형식의 구조이다.
- 데이터 **탐색에 유리**하다. ➡ O(1)
- 데이터 **추가, 삭제에 불리**하다. ➡ O(n)
- 데이터 추가, 삭제 시 메모리를 재할당하기 때문에 Array보다 속도가 느리다.
- Java 1.2에서 추가되었다.

## LinkedList

- 동적 길이 제공
- 크기를 한번 결정해도 변경할 수 있다.
- 데이터가 순서대로 늘어선 것이 아니라, 데이터의 주소 값으로 연결되어 있는 구조이다.
- 데이터 **탐색에 불리**하다. ➡ O(n)
- 데이터 **추가, 삭제에 유리**하다. ➡ O(1)
- Java 1.2에서 추가되었다.

## References

- [[Java] Array / ArrayList / LinkedList 비교](https://m.blog.naver.com/tjsdk2769/221781313533)
- [Java의 LinkedList와 ArrayList에 대한 비교](https://www.holaxprogramming.com/2014/02/12/java-list-interface/)
- [자바 벡터(Vector)와 어레이리스트(ArrayList) 비교](https://yeolco.tistory.com/94)