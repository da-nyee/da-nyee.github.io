---
title: '[Operating System] 동기 vs 비동기 (Synchronous vs Asynchronous)'
author: da-nyee
date: 2020-10-12 23:26:26 +0900
categories: [TIL, Operating System]
tags: [operating system, synchronous, asynchronous]
---

## Introduction

> Synchronous vs Asynchronous:<br/>
> 처리해야 하는 작업들을 **어떠한 흐름**으로 처리할 것인가에 대한 관점이다.<br/>

> Blocking vs Non-blocking:<br/>
> 처리돼야 하는 하나의 작업이 **전체적인 작업 흐름**을 **막느냐, 안 막느냐**에 대한 관점이다.

## Synchronous

- Request 이후 Response를 받아야만 다음 동작이 이루어지는 방식이다.
- 어떠한 작업을 처리할 동안에는 다른 작업은 정지한다.
- 실제 CPU가 느려지는 것은 아니다. 그러나, 전체적인 시스템 효율이 저하될 수 있다.

## Asynchronous

- Request 이후 Response를 받지 않아도 다음 동작이 이루어지는 방식이다.
- 어떠한 작업을 처리할 동안 다른 작업을 할 수 있다.
- 따라서, 컴퓨터 자원을 효율적으로 사용할 수 있다.
- Request 시 할 일이 끝난 후 처리 결과를 알려주는 **콜백 함수(Callback Function)**을 함께 사용한다.
- 콜백을 했을 때, 호출받은 함수는 바로 응답(확인)을 수행한다.
- 해당 응답은 처리 결과에 대한 응답이 아닌, Request에 대한 확인 동작일 뿐이다.
- 이는 DOS와 같은 단일 OS에서는 불가능하다. Windows와 같은 Multitask 환경에서만 가능하다.<br/>

👉 콜백 함수(Callback Function): 사용자가 아닌 일을 마친 시스템이 호출하는 형태이다.<br/>

![image](https://user-images.githubusercontent.com/50176238/95754670-55ff4d80-0cde-11eb-92d2-a004a9606fba.png)

## Pros and Cons

### Synchronous

🙂 설계가 매우 간단하다. 직관적이다.<br/>
😕 결과가 주어질 때까지 아무것도 못하고 대기해야 한다.

### Asynchronous

🙂 결과가 주어지기 전에도 다른 작업을 할 수 있다. 자원을 효율적으로 사용할 수 있다.<br/>
😕 Synchronous보다 설계가 복잡하다. 덜 직관적이다.

## Examples

### 1. Asynchronous + Non-blocking

1-1. 개발팀장이 사원 1에게 업무 A, 사원 2-업무 B, 사원 3-업무 C를 지시한다. **비동기적 작업 지시**<br/>
1-2. 개발팀장은 다른 업무를 보기 시작한다.<br/>
1-3. 각 사원들은 자기가 맡은 역할을 끝내는 대로 개발팀장에게 보고한다. **논블로킹 작업 처리**

### 2. Synchronous + Non-blocking

2-1. 개발팀장이 사원 1에게 업무 A를 지시한다.<br/>
2-2. 개발팀장은 사원 1이 업무를 **끝마칠 때까지** 뒤에서 기다린다. **동기적 작업 지시**<br/>
2-3. 개발팀장은 사원 1이 업무를 끝내면 이를 확인한다.<br/>
2-4. 개발팀장이 사원 2에게 업무 B를 지시한다.<br/>
2-5. 개발팀장은 사원 2가 업무를 **끝마칠 때까지** 뒤에서 기다린다. **동기적 작업 지시**<br/>
2-6. 개발팀장은 사원 2가 업무를 끝내면 이를 확인한다.

### 3. Asynchronous + Blocking

3-1. 개발팀장이 사원 1에게 업무 A를 지시 후 사원 2에게 업무 B를 지시하고 싶었지만, **비동기적 작업 지시**<br/>
3-2. 개발팀장을 사원 1이 **붙잡는다.** **블로킹 작업 처리**<br/>
3-3. 사원 1은 자신의 일이 다 끝날 때까지 개발팀장을 놓아주지 않는다.<br/>
3-4. 사원 1의 업무가 끝나고 보고를 받고 나서야, 개발팀장은 사원 2에게 업무 B를 지시한다.

### 4. Synchronous + Blocking

2번의 Synchronous + Non-blocking과 동일하게 진행된다.<br/>

👉 **Synchronous vs Asynchronous는 개발팀장의 입장**,<br/>
👉 **Blocking vs Non-blocking은 사원의 입장**에서 생각해야 한다.<br/>
👉 만약 상사가 개발팀장에게 업무를 지시했다면, **Blocking vs Non-blocking 관점**에서도 생각할 수 있다.

## Conclusion

- **Asynchronous + Non-blocking**이 다른 경우들에 비해 갖는 **장점은 독보적**이다.
- **"비동기"로 처리**했다. == **수행하고자 하는 작업이 "Non-blocking"임을 내포**하는 말이다.
- 만약 **작업이 순차적으로 처리**되어야 한다면, **Synchronous** 또는 **Blocking**을 사용해야 한다.

## References

- [[용어정리] 동기방식&비동기방식 비교](https://jieun0113.tistory.com/73)
- [동기와 비동기의 개념과 차이](https://private.tistory.com/24)
- [동기 vs 비동기, 블로킹 vs 논블로킹 쉽게 이해하기](https://siyoon210.tistory.com/147)