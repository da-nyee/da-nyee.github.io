---
title: '[Redis] HyperLogLog'
author: da-nyee
date: 2022-04-26 02:42:06 +0900
categories: [TIL, Redis]
tags: [redis, hyperloglog]
---

실시간 unique count는 어떻게 계산할까?<br/>

애플리케이션에서 자료구조를 사용해서 데이터를 저장하고, 같은 데이터가 들어오면 저장하지 않는 방법이 있다.<br/>
하지만 이 방법은 대용량 데이터를 다루는 애플리케이션에서는 적절하지 않다.<br/>
메모리 공간을 많이 차지해서 다소 무거울 수 있고, 더 중요한 비즈니스 로직을 처리하기 위한 리소스를 쓰기 때문이다.<br/><br/>

그렇다면 더 효율적인 방식은 없을까?<br/>

Redis를 활용하면 조금 더 쉽고 가볍게 문제를 해결할 수 있다.<br/>
물론 Set과 같은 자료구조를 쓰는 방식은 아니다.<br/>
Redis에는 <b>HyperLogLog</b> 자료형이 있다.<br/>
이 자료형은 unique count를 세고, 메모리를 적게 차지하고 결과의 오차율도 낮다.<br/><br/>

이어지는 내용에서 HyperLogLog의 개념과 명령어를 알아보자.<br/>

<br/>

## HyperLogLog?

- Redis에 있는 자료형
- <b>key : value</b> 형태로 데이터를 저장한다.
    - key는 고유한 값이다.
    - value는 unique count 값이다.
- HyperLogLog는 특정 데이터를 저장하지 않고, unique count를 계산해서 저장한다.
- 따라서 어떤 데이터가 저장되었는지 확인할 수 없다.

<br/>

## 명령어

### PFADD

- 1개의 key와 n개의 value를 한번에 입력할 수 있다.
- key에 value가 <b>없으면 1</b>을, <b>있으면 0</b>을 반환한다.

```bash
# src/redis-cli PFADD {key} {value1} {value2} ..

% src/redis-cli PFADD woowacourse dani suri
(integer) 1
% src/redis-cli PFADD woowacourse dani
(integer) 0
% src/redis-cli PFADD woowacourse pomo 
(integer) 1
% src/redis-cli PFADD woowacourse mazzi
(integer) 1
% src/redis-cli PFADD woowacourse suri pomo mazzi
(integer) 0

% src/redis-cli PFADD backend dani better solong
(integer) 1

% src/redis-cli PFADD frontend sunny ella
(integer) 1
```

### PFCOUNT

- key에 몇 개의 value가 저장되었는지, 즉 unique count를 센다.

```bash
# src/redis-cli PFCOUNT {key}

% src/redis-cli PFCOUNT woowacourse
(integer) 4
```

### PFMERGE

- 2개 이상의 key를 1개의 key로 합친다.
- 이때도 unique count를 구해서, 전체 value에서 중복되는 value는 1번만 더한다.
    - e.g. woowacourse와 backend에 각각 dani가 있어, 둘을 합칠 때 dani는 1번만 count됐다.

```bash
# src/redis-cli PFMERGE {key1} {key2} {key3} ..

% src/redis-cli PFMERGE woowacourse backend frontend
OK

% src/redis-cli PFCOUNT woowacourse
(integer) 8
```

<br/>

## References

- [Redis - HyperLogLog](https://redis.com/redis-best-practices/counting/hyperloglog/)
- [HyperLogLog Introduction](http://redisgate.kr/redis/command/hyperloglog.php)
- [[Redis] HyperLogLog](https://minholee93.tistory.com/entry/Redis-HyperLogLog)