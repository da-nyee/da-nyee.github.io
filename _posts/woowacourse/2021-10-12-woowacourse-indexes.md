---
title: '[우아한테크코스] 인덱스 (Indexes)'
author: da-nyee
date: 2021-10-12 01:05:04 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, indexes]
---

## 실습환경 세팅

- Workbench 설치
- 도커 이미지 다운로드

```bash
$ docker run -d -p 33306:3306 brainbackdoor/data-subway:0.0.2
```

- Workbench에서 `localhost:33306 (ID: root, PW: masterpw)`로 접속

<br/>

## 인덱스 생성 및 삭제

### 인덱스 생성

```sql
CREATE INDEX `idx_programmer_id` ON `subway`.`programmer` (id);
```

### 인덱스 삭제

```sql
DROP INDEX `idx_programmer_id` ON `subway`.`programmer`;
```

<br/>

> 인덱스가 되는 PK 외의 컬럼만을 WHERE 조건으로 지정하는 검색

- 인덱스를 사용 X
- Full Table Scan을 함

<br/>

## Index Range Scan

### 인덱스 생성

```sql
CREATE INDEX `idx_programmer_country_open_source` ON `subway`.`programmer` (country, open_source);
```

### 실행계획 확인

- Index Range Scan을 위해서는 인덱스 선두 컬럼이 WHERE에 있어야 함
- e.g. (country, open_source)에서 country가 WHERE에 있어야 함

### 예시

> (country, open_source)에서 country가 WHERE에 없는 경우

- Table Full Scan을 함

```sql
EXPLAIN SELECT * FROM programmer WHERE open_source = 'Yes';
```

<img style="width: 520px; height: 34px" src="https://user-images.githubusercontent.com/50176238/136820830-e15a03fc-56ac-440c-9eb4-300901d3a5d9.png">

> (country, open_source)에서 country가 WHERE에 있는 경우

- Index Range Scan을 함

```sql
EXPLAIN SELECT * FROM programmer WHERE open_source = 'Yes' AND country LIKE 'Nigeria';
```

<img src="https://user-images.githubusercontent.com/50176238/136820967-bf6ec5fe-e04e-4f1b-ba01-2d1be27a9bcb.png">

<img style="width: 400px; height: 250px" src="https://user-images.githubusercontent.com/50176238/136814933-b382c432-4ed2-4bc3-832f-50b30de47c7a.png">

<br/>

## Index Full Scan

### 실행계획 확인

- Leaf 블록 전체를 스캔하는 경우

### 예시

- Index Full Scan을 함

```sql
EXPLAIN SELECT COUNT(*) FROM programmer;
```

<img style="width: 560px; height: 34px" src="https://user-images.githubusercontent.com/50176238/136819581-26d02731-470f-45b5-be91-b8321d10383a.png">

<img style="width: 400px; height: 240px" src="https://user-images.githubusercontent.com/50176238/136815341-8e7f6913-5028-4612-a808-5140860b0234.png">

<br/>

## Index Unique Scan

### 인덱스 생성

```sql
CREATE INDEX `idx_programmer_member_id` ON `subway`.`programmer` (member_id);
```

### 실행계획 확인

- 인덱스가 존재하는 컬럼에 중복값이 없는 경우

### 예시

> `=` 조건으로 검색하는 경우

- 데이터를 1건 찾는 순간 더 이상 탐색할 필요 X

```sql
EXPLAIN SELECT * FROM programmer WHERE member_id = 10;
```

<img style="width: 670px; height: 36px" src="https://user-images.githubusercontent.com/50176238/136821081-4a5500fd-ab19-4ca8-a3e2-badae396e158.png">

> `<, >, <=, >=` 조건으로 검색하는 경우

- 범위 검색
- 수직적 검색만으로는 모두 찾을 수 X
- Index Range Scan을 함

```sql
EXPLAIN SELECT * FROM programmer WHERE member_id < 10;
```

<img src="https://user-images.githubusercontent.com/50176238/136821161-dda8f079-f43e-48e1-8081-56be40d934dd.png">

<img style="width: 420px; height: 240px" src="https://user-images.githubusercontent.com/50176238/136815593-3f4f3f1e-5c0e-42b7-9ec0-bbfad0b820c1.png">

<br/>

## 테이블 액세스 최소화

### 인덱스 컬럼 추가
    
- 가장 일반적으로 사용하는 튜닝 기법
- 인덱스는 정렬되므로, WHERE에 해당하는 PK 범위를 줄여 랜덤 액세스 횟수를 줄임

### 예시

- 쿼리 결과에 해당하는 데이터는 BLAKE 1명인데, 이를 찾기 위해 테이블에 6번 액세스함
- 인덱스 구성을 (deptno, sal)로 변경하면 해결되는 문제지만, 실제 운영 환경에서 인덱스 구성을 변경하는 건 쉽지 X
- 따라서, 기존 인덱스에 (sal)을 추가하여 테이블 랜덤 액세스 횟수를 줄일 수 있음 (인덱스 스캔량을 줄이는 건 X)

```sql
SELECT * FROM emp WHERE deptno = 30 AND sal >= 2000
```

![table_access_minimization](https://user-images.githubusercontent.com/50176238/136816053-65c9e5ca-aa8d-4957-a596-7a71861df91b.png)

<br/>

## Reference

- 우아한테크코스 강의 자료