---
title: '[Spring] JdbcTemplate queryForObject() - Return Value Issue'
author: da-nyee
date: 2021-06-16 04:06:52 +0900
categories: [TIL, Spring]
tags: [spring, jdbctemplate, queryforobject(), return value, issue]
---

## Introduction

회고 프로젝트에서 DB 레코드를 조회하는 로직을 구현했다.<br/>
이때, 파라미터로 넘어오는 id를 기준으로 `findById()`를 작성했다.<br/>
해당 메소드에서 Optional을 반환하게 만들었는데, 여기서 예외 핸들링을 제대로 해주지 않아 문제가 발생했다.<br/>
어떤 문제였고 어떻게 해결했는지 간단하게 정리하려 한다.<br/>

## Issue

```
public Optional<Member> findById(Long id) {
    String query = "SELECT id, name FROM MEMBER WHERE id = ?";

    return this.jdbcTemplate.query(query, ROW_MAPPER, id)
            .stream()
            .findAny();
}
```

다른 크루가 기존에 작성했던 코드이다.<br/>
나는 이 메소드에서 어색한 부분을 느꼈다.<br/>
하나의 Member를 반환하는데 왜 queryForObject()가 아닌 query()를 사용했지? 란 생각이 들었다.<br/>

<br/>

```
public Optional<Member> findById(Long id) {
    String query = "SELECT id, name FROM MEMBER WHERE id = ?";

    return Optional.ofNullable(this.jdbcTemplate.queryForObject(query, ROW_MAPPER, id));
}
```

그래서 위와 같이 변경했다.<br/>
쿼리 결과가 null일 수 있으니까 `Optional.ofNullable()`을 활용했다.<br/>
조회 시 데이터가 없으면 `Optional<null>`이 반환될 거라 생각했다.<br/>

근데, 완전히 잘못 생각하고 있었다.<br/>
오히려 `EmptyResultDataAccessException`이 발생했다.<br/>

<br/>

```
public Optional<Member> findById(Long id) {
    String query = "SELECT id, name FROM MEMBER WHERE id = ?";

    try {
        return Optional.ofNullable(this.jdbctemplate.queryForObject(query, ROW_MAPPER, id));
    } catch (EmptyResultDataAccessException e) {
        return Optional.empty();
    }
}
```

해당 코드처럼 try-catch 구문으로 조회 결과가 없으면 예외를 잡도록 구현해야 했다.<br/>
catch 문에서는 `Optional.empty()`를 반환한다.<br/>
조회 결과로 null이 나오는 경우는 해당 데이터에 null이 들어있을 때 같다.<br/>

나는 이 부분에서 try-catch 구문을 꼭 써야 할까? 의문이 들었는데,<br/>
손너잘과 얘기한 뒤 한번은 쓸 수 밖에 없다고 결론지었다.<br/>

손너잘이 DAO에 try-catch 구문을 안 쓸 수 있는 방법도 소개해줬다.<br/>

> JdbcTemplate을 감싸는 다이나믹 프록시를 생성하고, 여기서 try 구문으로 queryForObject()를 수행하고, 예외가 발생하면 catch 구문으로 Optional.empty()를 반환하게 한다.<br/>

아무튼 최소 한번은 try-catch 구문이 필요하다.<br/>

이런 흐름으로 문제 해결을 완료했다!<br/>