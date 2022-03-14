---
title: '[Java] try-with-resources'
author: da-nyee
date: 2021-04-12 01:15:22 +0900
categories: [TIL, Java]
tags: [java, try-with-resources]
---

## try-with-resources

- <b>try(..)에 선언된 객체</b>에 대해서 <b>try 문이 종료될 때 자동으로 자원을 해제해주는 기능</b>이다.
- <b>try 내의 객체가 AutoCloseable을 구현</b>했다면 <b>try 문을 벗어날 때</b> 객체의 <b>close()</b>를 호출한다.
    - 만약 try 내의 객체가 AutoCloseable을 구현하지 않았다면, 객체의 close()가 호출되지 않는다.
- try 문 외에 catch, finally 문도 사용할 수 있다.

## Pros

- 코드를 짧고 간결하게 만든다. 👉 코드를 읽기 쉽고 유지보수하기 쉽게 만든다.
- try-catch-finally처럼 명시적으로 close()를 호출하려면 많은 if 문과 try-catch 문을 사용해야 한다.
    - 실수로 close()를 깜빡하는 경우가 생길 수 있다.
    - try-with-resources를 사용하면 이런 자잘한 버그가 발생할 가능성이 적어진다.

## Code

- <b>try(..)</b>에 <b>Connection</b>, <b>PreparedStatement</b> 객체들을 선언 및 할당한다.
    - try(..)에서 선언한 변수들은 try 내에서 사용할 수 있다.
- <b>try 문을 벗어나면</b> try(..)에서 선언된 객체들의 <b>close()</b>를 호출한다.
    - 따라서, finally에서 명시적으로 close()를 호출할 필요가 없다.

```
try (
        Connection connection = getConnection();
        PreparedStatement pstm = connection.prepareStatement(query)
) {
    pstm.executeUpdate();
} catch (SQLException e) {
    e.printStackTrace();
}
```

## References

- [Java - Try-with-resources로 자원 쉽게 해제하기](https://codechacha.com/ko/java-try-with-resources/)
- [우아한테크코스 체스 미션 - 휴 코드 리뷰](https://github.com/woowacourse/java-chess/pull/258#discussion_r611063069)