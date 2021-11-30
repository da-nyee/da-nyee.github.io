---
title: '[우아한테크코스] 우아한객체지향 - Part 1'
author: da-nyee
date: 2021-12-01 02:34:08 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, seminar, oop]
---

## 의존성 (Dependency)

- 의존성이 있다?
- B가 변경될 때, A도 변경될 수 있다. == <b>변경 가능성</b>이 있다. ➡️ 무조건 변경되는 건 X
- 의존성은 <b>변경</b>과 관련이 있다.

![dependency](https://user-images.githubusercontent.com/50176238/144046250-249b70a2-7566-42b3-a841-dac44189e588.png)

<br/>

## 클래스 의존성

### 연관 관계 (Association)

- A에서 B로 이동할 수 있다. == A가 B로 가는 경로를 가지고 있다.

![association](https://user-images.githubusercontent.com/50176238/144049279-6d8b876b-73b2-4361-ba2c-ab9430fda7e4.png)

```java
class A {

    private B b;
}
```

### 의존 관계 (Dependency)

- 파라미터에 해당 타입이 나오거나 리턴 타입에 해당 타입이 나오거나 메소드 안에서 해당 타입의 인스턴스를 생성한다.

![dependency](https://user-images.githubusercontent.com/50176238/144049628-61b78eb9-1e64-4c3f-8624-05805cda3e4a.png)

```java
class A {

    public B method(B b) {
        return new B();
    }
}
```

### 상속 관계 (Inheritance)

- B의 구현을 A가 상속/계승받는 것이다.
- B가 변경될 때, A도 같이 변경된다.


![inheritance](https://user-images.githubusercontent.com/50176238/144050208-2364dc1d-d8cf-4e35-8b33-0ccd067e1f85.png)

```java
class A extends B {
}
```

### 실체화 관계 (Realization)

- 인터페이스를 구현하는 관계를 의미한다.

![realization](https://user-images.githubusercontent.com/50176238/144050881-e9903561-e3a3-4b2d-a440-6062f1a02a1d.png)

```java
class A implements B {
}
```

### 상속 관계 vs 실체화 관계

- 상속 관계는 구현이 변경되면 영향을 받을 수 있다.
- 실체화 관계는 인터페이스의 operation signature가 변경되었을 때만 영향을 받는다.

<br/>

## 패키지 의존성

- 패키지에 포함되어 있는 클래스 사이의 의존성을 말한다.
- 패키지 A가 패키지 B에 의존한다. == 패키지 B에 있는 클래스가 변경될 때, 패키지 A에 있는 클래스도 변경될 수 있다.

![package-dependency](https://user-images.githubusercontent.com/50176238/144052219-cbb6ff94-e2bf-430f-8146-979303db13d7.png)

<br/>

## 좋은 의존성을 위한 가이드

### 양방향 의존성을 피한다.

- B가 변경될 때 A도 변경되고, A가 변경될 때 B도 변경되는 경우? A와 B가 원래는 하나의 클래스라고 봐도 무방한 것을 어거지로 찢어둔 걸로 봐도 된다.
- 항상 A와 B간의 관계를 동기화시켜줘야 하는 문제가 있다.
- 성능 이슈도 잦다. ➡️ A와 B의 sync를 맞추는 것에서 버그가 많이 발생한다.
- 물론 양방향 연관관계가 필요한 경우가 있지만, 가급적이면 <b>단방향 연관관계</b>로 설계한다.

```java
// 양방향
class A {

    private B b;

    public void setB(B b) {
        this.b = b;
        this.b.setA(this);
    }
}

class B {

    private A a;

    public void setA(A a) {
        this.a = a;
    }
}

// 단방향
class A {

    private B b;

    public void setB(B b) {
        this.b = b;
    }
}

class B {
}
```

### 다중성이 적은 방향을 선택한다.

- 컬렉션을 인스턴스 변수로 두면, 다양한 문제(e.g. JPA 성능 이슈)가 발생할 수 있다.
- 객체들의 관계를 유지하기 위한 여러 노력이 필요하다.
- 그러므로 다중성이 적은 방향, 즉 아래의 코드처럼 B에서 A로 단방향 참조를 가지는 게 좋다.

```java
// 일대다
class A {

    private Collection<B> bs;
}

class B {
}

// 다대일
class A {
}

class B {

    private A a;
}
```

### 의존성이 필요없다면 제거한다.

- A와 B간의 의존성이 정말로 불필요하다면, 아예 없애는 게 가장 좋다.

```java
// 단방향
class A {
    
    private B b;
}

class B {
}

// 없음
class A {
}

class B {
}
```

### 패키지 사이의 의존성 사이클을 제거한다.

- 패키지 사이에는 양방향 의존성, 즉 사이클이 있으면 안된다.

#### DO NOT
![package-bi-directional](https://user-images.githubusercontent.com/50176238/144084706-3ff4ef18-6e73-4dd5-99b7-6cd0631f01af.png)

#### DO
![package-uni-directional](https://user-images.githubusercontent.com/50176238/144084906-c220b6a4-d1c9-4447-9c7b-71b9af85e88b.png)

<br/>

## Reference

- [[우아한테크세미나] 190620 우아한객체지향 by 우아한형제들 개발실장 조영호님](https://www.youtube.com/watch?v=dJ5C4qRqAgA&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech)