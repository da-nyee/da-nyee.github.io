---
title: '[Effective Java] 아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라'
author: da-nyee
date: 2021-12-15 01:58:38 +0900
categories: [BOOKS, Effective Java]
tags: [books, effective java]
---

- 클래스의 인스턴스를 얻는 수단은 (1) public 생성자와 (2) 정적 팩터리 메서드(static factory method)가 있다.

```java
public final class Boolean implements Serializable, Comparable<Boolean> {

    public static final Boolean TRUE = new Boolean(true);
    public static final Boolean FALSE = new Boolean(false);

    @HotSpotIntrinsicCandidate
    public static Boolean valueOf(boolean b) {
        return b ? TRUE : FALSE;
    }
}
```

<br/>

## 장점

### 이름을 가질 수 있다.

- 생성자는 반환될 객체의 특성을 제대로 설명하지 못한다.
- 정적 팩터리 메서드는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.

```java
// 값이 소수인 BigInteger를 반환한다.

// 생성자
BigInteger(int, int, Random)

// 정적 팩터리 메서드
BigInteger.probablePrime()
```

- 생성자는 중복되는 시그니처를 사용할 수 없다.
- 정적 팩터리 메서드는 이름만 잘 변경하면 중복되는 시그니처를 사용할 수 있다.

```java
// 생성자
Pizza(int, String)

// 정적 팩터리 메서드
Pizza.of(int, String)
Pizza.valueOf(int, String)
```

### 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.

- 불변 클래스(immutable class)는 미리 인스턴스를 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 방법으로 불필요한 객체 생성을 피할 수 있다.
- 이는 (특히 생성 비용이 큰) 같은 객체가 자주 요청되는 상황에서 성능을 상당히 끌어올려 준다.
- 인스턴스 통제(instance-controlled) 클래스는 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다.
- 이렇게 하는 이유는 (1) 클래스를 싱글턴(singleton)으로 만들 수 있고 (2) 클래스를 인스턴스화 불가(noninstantiable)로 만들 수 있고 (3) 불변값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수 있기 때문이다.

### 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

- 반환할 객체의 클래스를 자유롭게 선택할 수 있는 <b>엄청난 유연성</b>을 선물한다.
- 이를 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있다.
- 그래서 API를 만들 때 API를 작게 유지할 수 있다.
- 해당 유연성은 인터페이스를 정적 팩터리 메서드의 반환 타입으로 사용하는 인터페이스 기반 프레임워크를 만드는 핵심 기술이다.

### 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

- 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환해도 상관없다.
- 예를 들어, EnumSet 클래스는 정적 팩터리 메서드만 제공한다. OpenJDK에서는 원소의 수에 따라 RegularEnumSet 클래스 또는 JumboEnumSet 클래스의 인스턴스를 반환한다.

### 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

- 대표적인 서비스 제공자 프레임워크(service provider framework)로는 JDBC(Java Database Connectivity)가 있다.
- 이러한 프레임워크는 3개의 핵심 컴포넌트와 1개의 컴포넌트로 이뤄진다. 각각 서비스 인터페이스(service interface), 제공자 등록 API(provider registration API), 서비스 접근 API(service access API), 그리고 서비스 제공자 인터페이스(service provider interface)이다.
- JDBC에서는 <b>Connection</b>이 서비스 인터페이스 역할을, <b>DriverManager.registerDriver</b>가 제공자 등록 API 역할을, <b>DriverManager.getConnection</b>이 서비스 접근 API 역할을, 그리고 <b>Driver</b>가 서비스 제공자 인터페이스 역할을 수행한다.

<br/>

## 단점

### 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

- 상속을 하기 위해서는 public 또는 protected 생성자가 필요하다.
- 이 제약은 <b>상속보다 컴포지션을 사용</b>하도록 유도한다는 점에서 오히려 장점으로 받아들일 수도 있다.

### 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

- 생성자처럼 API 설명에 명확히 드러나지 않는다.
- 사용자는 정적 팩터리 메서드 방식의 클래스를 인스턴스화할 방법을 알아내야 한다.

<br/>

## 네이밍 컨벤션

### from

- 매개변수가 1개이고, 해당 타입의 인스턴스를 반환하는 메서드

```java
Data date = Date.from(instant);
```

### of

- 매개변수가 n개이고, 적합한 타입의 인스턴스를 반환하는 메서드

```java
Set<Rank> cards = EnumSet.of(JACK, QUEEN, KING);
```

### valueOf

- from과 of의 자세한 버전

```java
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

### instance / getInstance

- 매번 새로운 인스턴스를 생성해서 반환할 수도, 같은 인스턴스를 반환할 수도 있다.

```java
User user = User.getInstance(name);
```

### create / newInstance

- 매번 새로운 인스턴스를 생성해서 반환한다.

```java
Object array = Array.newInstance(classObject, arrayLength);
```

### getType

- getInstance와 같지만, 자신 클래스의 인스턴스가 아닌 다른 클래스의 인스턴스를 반환한다.

```java
FileStore fileStore = Files.getFileStore(path);
```

### newType

- newInstance와 같지만, 자신 클래스의 인스턴스가 아닌 다른 클래스의 인스턴스를 반환한다.

```java
BufferedReader bufferedReader = Files.newBufferedReader(path);
```

### type

- getType과 newType의 간결한 버전

```java
List<Complaint> litany = Collections.list(legacyLitany);
```