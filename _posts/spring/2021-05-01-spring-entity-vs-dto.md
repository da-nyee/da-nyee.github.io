---
title: '[Spring] Entity vs DTO'
author: da-nyee
date: 2021-05-01 00:35:02 +0900
categories: [TIL, Spring]
tags: [spring, entity, dto]
---

## Entity

- DB 테이블 내의 컬럼만을 필드로 가지는 클래스
- 상속을 받거나 구현체여서는 안 된다.
- DB 테이블 내에 존재하지 않는 컬럼을 가져서도 안 된다.

<img width="218" alt="piece_table" src="https://user-images.githubusercontent.com/50176238/116714341-a1761080-aa10-11eb-99dd-ab2f9ebcfcc1.png">

- JPA를 사용하는 경우 @Entity 어노테이션을 명시해야 한다. 👉 Entity 클래스임을 지정한다.
- DB 테이블과 `1:n`으로 매핑된다.

```
@Entity
class Piece {
    private Long id;
    private String piece_name;
    private String piece_position;
    private Long room_id;
}
```

- 테이블과 매핑되는 클래스 👉 무엇을 `저장할지`에 관련된다.

## DTO

- 데이터 전송 객체(Data Transfer Object)
- 주로 비동기 처리를 할 때 사용한다.
- 필요한 속성들만 추려 JSON 형식으로 파싱하여 보내줘야 하는 경우 데이터 가공 처리를 위해 사용한다.

```
@Getter
public class MoveResponseDto {
    private final boolean isMovable;
    private final String errorMessage;
}
```

- 주로 View layer와 매핑되는 클래스 👉 무엇을 웹 페이지에 `보여줄지`에 관련된다.

## References

- [Entity, VO, DTO](https://webdevtechblog.com/entity-vo-dto-666bc72614bb)
- [Difference between Entity and DTO](https://stackoverflow.com/questions/39397147/difference-between-entity-and-dto#:~:text=Entity%20is%20class%20mapped%20to,either%20male%2Ffemale%2Fother.)