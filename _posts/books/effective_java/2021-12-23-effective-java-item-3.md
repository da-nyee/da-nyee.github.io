---
title: '[Effective Java] 아이템 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라'
author: da-nyee
date: 2021-12-23 03:15:35 +0900
categories: [BOOKS, Effective Java]
tags: [books, effective java]
---

## 싱글턴 (Singleton)

- 인스턴스를 <b>오직 1개만</b> 생성할 수 있는 클래스 (e.g. 무상태(stateless) 객체 또는 설계상 유일해야 하는 컴포넌트)
- 클래스를 싱글턴으로 만들면, 이를 사용하는 클라이언트는 테스트하기 어려워질 수 있다. 👉 [왜 테스트하기 어려울까?](https://ssoco.tistory.com/65)

<br/>

## 어떻게 만들까?

### 1. public static final 필드 방식

- private 생성자는 public static final 필드를 초기화할 때 딱 1번만 호출된다.
- public/protected 생성자가 없으므로 해당 클래스의 객체가 전체 시스템에서 1개뿐임이 보장된다.
- 단, 권한이 있는 클라이언트는 리플렉션 API를 이용하여 private 생성자를 호출할 수 있다.
- 이러한 공격은 생성자를 수정하여 클래스의 2번째 인스턴스가 생성되려 할 때 예외를 발생함으로써 방지할 수 있다.

#### 장점

- 해당 클래스가 싱글턴임이 API에 명백히 드러난다.
- 간결하다.

#### 단점

- 직렬화하기 어렵다.
- 단순히 Serializable 인터페이스를 구현한다고 선언하는 것만으로는 부족하다.
- 모든 필드에 transient 키워드를 선언하고, readResolve()를 제공해야 한다.
- 아니면, 직렬화된 인스턴스를 역직렬화할 때마다 새로운 인스턴스가 만들어진다.

```java
public class Elvis {

    public static final Elvis INSTANCE = new Elvis();

    private Elvis() {
    }

    public void dance() {
        ...
    }
}
```

### 2. 정적 팩터리 방식

- getInstance()는 항상 같은 객체의 참조를 반환하여 클래스의 2번째 인스턴스는 결코 만들어지지 않는다.
- 한편, 이때에도 리플렉션 API를 통한 예외는 똑같이 적용된다.

#### 장점

- 요구사항이 변경되면, API를 바꾸지 않고도 싱글턴이 아니게 수정할 수 있다.
- 원한다면 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다.
- 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다. (e.g. Elvis::getInstance)

#### 단점

- 직렬화하기 어렵다.
- 이하동문

```java
public class Elvis {

    private static final Elvis INSTANCE = new Elvis();
    
    private Elvis() {
    }
    
    public static Elvis getInstance() {
        return INSTANCE;
    }

    public void dance() {
        ...
    }
}
```

### 3. 열거 타입 방식

- 조금 부자연스러울 수는 있다.
- 그렇지만, <b>대부분 상황에서는 원소가 1개뿐인 열거(enum) 타입이 싱글턴을 만드는 가장 좋은 방법이다.</b>

#### 장점

- 더 간결하다.
- 추가 노력 없이 직렬화할 수 있다.
- 리플렉션 API의 공격에도 클래스의 2번째 인스턴스가 생기는 일을 완벽히 막아준다.

#### 단점

- 싱글턴이 열거 타입 외의 클래스를 상속해야 한다면, 이는 활용할 수 없다.
- 그러나 열거 타입이 인터페이스를 구현하도록 선언할 수는 있다.

```java
public enum Elvis {
    INSTANCE;
    
    public void dance() {
        ...
    }
}
```