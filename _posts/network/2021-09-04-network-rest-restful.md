---
title: '[Network] REST와 RESTful (REST and RESTful)'
author: da-nyee
date: 2021-09-04 02:21:19 +0900
categories: [TIL, Network]
tags: [network, rest, restful]
---

## REST?

- Representational State Transfer
- 웹의 장점을 최대한 활용할 수 있는 네트워크 기반의 아키텍처

### 구성 요소

- 행위
    - HTTP Method

```
GET, POST, PUT, PATCH, DELETE
```

- 자원
    - URI

```
/users, /comments
```

- 표현

```
/users/{id}, /comments/{id}
```
```
{
    "name": "dani",
    "major": "computer science"
}
```

<br/>

## RESTful?

아래의 6가지 조건을 충족해야 RESTful한 API를 작성했다고 할 수 있다.

1. 유니폼 인터페이스(Uniform Interface)
    - HTTP 표준을 따르면, 특정 언어 또는 기술에 종속되지 않고 모든 플랫폼(e.g. Web/AOS/IOS)에서 사용할 수 있다.
2. 무상태성(Stateless)
    - HTTP는 무상태 프로토콜이다. 그래서 REST도 무상태성을 갖는다.
    - HttpSession과 같은 컨텍스트 저장소에 상태 정보를 저장/관리하지 않고, API 서버는 단순하게 들어오는 요청을 처리만 하면 된다.
    - 세션과 같은 컨텍스트 정보를 신경쓸 필요가 없어 구현이 단순해진다.
3. 캐싱 가능(Cacheable)
    - HTTP 웹 표준을 그대로 사용하기 때문에 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    - HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
    - HTTP 표준에서 사용하는 Last-Modified(Time-Based) 또는 ETag(Content-Based)를 이용해서 캐싱을 구현할 수 있다.
4. 자체 표현 구조(Self-Descriptiveness)
    - 동사(Method) + 명사(URI)로 이루어져 있어 어떤 메소드에 무슨 행위를 하는지 알 수 있고,<br/>
    메시지를 JSON으로 표현하여 직관적으로 이해할 수 있는 구조를 가진다.
    - 따라서, REST API만 봐도 쉽게 이해할 수 있다.
5. 클라이언트-서버 구조(Client-Server Architecture)
    - 클라이언트는 사용자 인증 또는 컨텍스트(로그인 정보 등)을 관리하고,<br/>
    서버는 API를 제공하여 각각의 역할이 확실하게 구분된다.
    - 클라이언트와 서버에서 개발해야 하는 내용이 명확해지고, 서로간의 의존성이 줄어든다.
6. 계층형 구조(Hierarchical Architecture)
    - API 서버는 순수하게 비즈니스 로직을 수행한다.
    - 그 앞단에서 사용자 인증, 암호화(SSL), 로드 밸런싱 등을 하는 계층을 추가해서 구조상의 유연함을 만든다.

<br/>

## References

- [RESTful API란 ?](https://brainbackdoor.tistory.com/53)
- [REST API의 이해와 설계-#1 개념 소개](https://bcho.tistory.com/953)