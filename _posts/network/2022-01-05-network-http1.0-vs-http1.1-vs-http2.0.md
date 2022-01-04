---
title: '[Network] HTTP 1.0 vs HTTP 1.1 vs HTTP 2.0'
author: da-nyee
date: 2022-01-05 00:57:34 +0900
categories: [TIL, Network]
tags: [network, http 1.0, http 1.1, http 2.0]
---

## HTTP 1.0

- 1개의 커넥션당 1개의 요청을 처리한다.
- 따라서 요청을 동시에 전송할 수 없고, 하나의 요청에 대한 응답이 돌아온 후에 다음 요청을 보낼 수 있다.
- 이는 네트워크 지연(latency)를 발생시킨다.

<br/>

## HTTP 1.1

- HTTP 1.0의 단점을 개선시키고자 HTTP Pipelining을 도입했다.
    - HTTP Pipelining은 1개의 커넥션에 1개 이상의 요청을 담아 전송하는 방식으로, 네트워크 지연을 개선했다.
- 하지만 HTTP Pipelining은 구현하기 어렵고, HOL(Head of Line)이 발생할 수 있다.
    - HOL은 앞선 요청에 의해 뒤의 요청이 지연되는 현상이다.
    - e.g. 가장 맨 앞의 요청이 처리 속도가 오래 걸리는 것이면, 그 뒤의 요청에 지연이 발생한다.
    - 왜냐하면, 서버는 커넥션에서 요청을 받은 순서대로 처리해야 하기 때문이다.
- 한편, HTTP 1.1은 헤더가 무겁다는 단점도 있다.
    - 클라이언트와 서버간에는 수많은 HTTP 요청이 발생한다.
    - 이때, 헤더의 정보는 대체로 동일하다.
    - HTTP 1.1은 이러한 중복 헤더를 계속 전송한다.
    - 결국, 불필요한 데이터를 보내는 것에 네트워크 리소스가 소비되는 문제가 발생한다.

<br/>

## HTTP 2.0

- HTTP 1.1의 성능을 개선하기 위해 등장했고, Multiplexed Streams를 사용한다.
    - Multiplexed Streams는 1개의 커넥션으로 동시에 여러 개의 메시지를 주고 받을 수 있는 방법이다.
    - 요청에 대한 응답은 요청 순서에 상관 없이 Stream으로 받기 때문에 HOL이 발생하지 않는다.
- 또한, HTTP 2.0에서는 헤더가 Compression이 됐다.
    - 여기서는 Header Table과 Huffman Encoding을 이용하는 HPACK 압축 방식을 활용하여 HTTP 1.1에서 헤더가 무거운 문제를 개선했다.
    - 클라이언트와 서버는 각각 Header Table을 관리한다.
    - 클라이언트에서 이전 요청과 동일한 헤더는 Table의 index만 보내고, 변경된 필드는 Huffman Encoding을 하고 보낸다.
    - 이로써 헤더의 크기를 경량화할 수 있었다.

<br/>

## Reference

- [[네트워크] HTTP 1.1 VS HTTP 2.0](https://ssungkang.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP-11-VS-HTTP-20)