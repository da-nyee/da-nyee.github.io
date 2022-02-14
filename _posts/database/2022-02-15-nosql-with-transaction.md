---
title: '[Database] NoSQL에서도 트랜잭션 개념이 적용될까?'
author: da-nyee
date: 2022-02-15 02:35:38 +0900
categories: [TIL, Database]
tags: [database, nosql, transaction]
---

JpaRepository와 MongoRepository의 save() vs saveAll()을 고민하다가,<br/>
NoSQL에서도 트랜잭션을 지원하는지? 궁금증이 들었다.<br/>

> TMI

- JpaRepository에서 save()와 saveAll()은 DB를 찌르는 횟수는 동일하나, 프록시 객체 참조를 n번하는지 vs 1번하는지에 따라 성능 차이가 발생한다. 단, @Transactional 어노테이션을 붙였을 때 AOP로 인해 프록시 객체가 사용됐을 경우를 얘기한다.
- MongoRepository에서 save()는 매번 DB를 찌르는 반면, saveAll()은 한번에 배치로 DB를 찌르는 것에서 성능 차이가 있다.
- 결국, 각 Repository에서 두 메소드간의 성능 차이가 나타나는 이유는 다르다.
- 코드를 일일이 까보면서 확인했다.

아무튼 ACID(트랜잭션의 성질)을 생각하면 NoSQL은 Atomicity부터 지켜지기 힘들 것 같았다.<br/>
그래서 NoSQL에서의 트랜잭션이 가능한 말인가? 싶었고, 나름대로 답을 찾았다.<br/>

<br/>

## Conclusion

NoSQL이 처음 등장했을 때는 NoSQL에 트랜잭션이 아예 없었지만, 점차 트랜잭션에 대한 니즈가 생겼다.<br/>
현재는 NoSQL 따라 다르지만 트랜잭션을 지원한다. 단, RDBMS보다는 약한 수준으로 지원한다.<br/>

> RavenDB

- 최초로 트랜잭션을 지원했다.
- 물론 NoSQL 중에서를 의미한다.
- 한편, 몇몇 유저가 큰 범위의 복잡한 데이터를 업데이트할 때는 이 DB가 적합하지 않다고 보고했다.

> MongoDB

- 트랜잭션을 지원한다.
- 4.0 버전부터 다중 도큐먼트 ACID 트랜잭션, 4.2 버전부터 다중 컬렉션 ACID 트랜잭션 등이 추가됐다.
- 반면, [일부 제약사항](https://docs.mongodb.com/manual/core/transactions/#transactions-and-operations)도 존재한다.

따라서 NoSQL + 트랜잭션 조합이 불가능하지는 않다.<br/>
만약 가용성과 확장성(NoSQL의 특징)이 중요한데 무결성과 정확성도 중요하다면, 이 조합을 고려해도 좋을 것 같다.<br/>
그렇지만 트랜잭션이 강하게 지켜져야 한다면, 이때는 RDBMS를 선택해야 한다고 생각한다.<br/>

<br/>

## References

- [Transactions in NoSQL?](https://stackoverflow.com/questions/2212230/transactions-in-nosql)
- [Can NoSQL Database be Transactional?](https://bangdb.com/blog/transactional-database/)
- [What are ACID Transactions?](https://www.mongodb.com/basics/acid-transactions)
- [Transactions](https://docs.mongodb.com/manual/core/transactions/)