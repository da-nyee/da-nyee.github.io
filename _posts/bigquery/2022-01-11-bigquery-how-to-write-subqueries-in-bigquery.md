---
title: '[BigQuery] BigQuery에서 서브쿼리를 작성하는 방법 (How to write subqueries in BigQuery)'
author: da-nyee
date: 2022-01-11 16:06:16 +0900
categories: [TIL, BigQuery]
tags: [bigquery, subqueries]
---

## BigQuery?

- 서버리스 멀티 클라우드 데이터 웨어하우스
- GCP에서 제공한다.
- 높은 확장성과 합리적인 비용을 갖추고 있다.

<br/>

## with 키워드

- BigQuery는 SQL의 서브쿼리를 지원하지 않는다.
- 그렇지만, <b>with</b> 키워드를 사용해서 동일한 결과를 얻을 수 있다.

<br/>

## 예시

- [깃-들다(우아한테크코스 팀 프로젝트)](https://github.com/woowacourse-teams/2021-pick-git)를 예시로 든다.
- 아래 이미지와 같이 DB 테이블이 구성됐다고 가정하자.

![pickgit](https://user-images.githubusercontent.com/50176238/148893016-b55752a4-abb0-47ce-ac40-b76479e0ba01.png)

### 최종 목표

- <b>게시물당 댓글 수와 좋아요 수를 구하고, 이를 댓글 수를 기준으로 내림차순 정렬</b>한다.
- 큰 문제(전체)를 작은 문제(부분)으로 나누어서 점진적으로 발전시킨다.

### 1.

- 게시물당 댓글 수를 쿼리한다.

```sql
select p.id as post_id, count(*) as comment_count
from Post as p
join Comment as c on p.id = c.post_id
group by p.id;
```

### 2.

- 게시물당 좋아요 수를 쿼리한다.

```sql
select p.id as post_id, count(*) as likes_count
from Post as p
join Likes as l on p.id = l.post_id
group by p.id;
```

### 3.

- 게시물당 댓글 수와 좋아요 수를 쿼리한다.

```sql
with comment as (
    select p.id as post_id, count(*) as comment_count
    from Post as p
    join Comment as c on p.id = c.post_id
    group by p.id
),
likes as (
    select p.id as post_id, count(*) as likes_count
    from Post as p
    join Likes as l on p.id = l.post_id
    group by p.id
)
select p.id as post_id, comment.comment_count as comment_count, likes.likes_count as likes_count
from Post as p
join comment on (
    p.id = comment.post_id
)
join likes on (
    p.id = likes.post_id
);
```

### 4.

- 댓글 수를 기준으로 내림차순 정렬을 쿼리한다.

```sql
with comment as (
    select p.id as post_id, count(*) as comment_count
    from Post as p
    join Comment as c on p.id = c.post_id
    group by p.id
),
likes as (
    select p.id as post_id, count(*) as likes_count
    from Post as p
    join Likes as l on p.id = l.post_id
    group by p.id
)
select p.id as post_id, comment.comment_count as comment_count, likes.likes_count as likes_count
from Post as p
join comment on (
    p.id = comment.post_id
)
join likes on (
    p.id = likes.post_id
)
order by comment_count desc;
```

<br/>

## References

- [GCP - BigQuery](https://cloud.google.com/bigquery)
- 닐 버디님