---
title: '[MySQL] CROSS JOIN'
author: da-nyee
date: 2023-07-23 00:56:40 +0900
categories: [TIL, MySQL]
tags: [mysql, cross join]
---

## CROSS JOIN

- Cartesian Product (n * m)
- on/where절(조건절)을 거의 사용하지 않는다.
- CROSS JOIN + ON/WHERE은 INNER JOIN과 동일한 결과를 반환한다.<br/>
따라서 이런 경우에는 직관적인 INNER JOIN을 사용하는 게 낫다.

<br/>

<img width=500 src="https://github.com/da-nyee/da-nyee.github.io/assets/50176238/0ac9e216-d175-4866-9a82-656fcd2789be">

<br/>

## References

- [How to write ON condition for CROSS JOIN in SQL SERVER](https://stackoverflow.com/questions/56254997/how-to-write-on-condition-for-cross-join-in-sql-server)
- [MySQL CROSS JOIN Keyword](https://www.w3schools.com/mysql/mysql_join_cross.asp)
- [Top 20 SQL JOINs Interview Questions and Answers (2023)](https://www.dataquest.io/blog/sql-joins-interview-questions-and-answers/)