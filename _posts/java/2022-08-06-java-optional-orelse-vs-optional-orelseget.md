---
title: '[Java] Optional.orElse() vs Optional.orElseGet()'
author: da-nyee
date: 2022-08-06 02:35:11 +0900
categories: [TIL, Java]
tags: [java, optional, orelse, orelseget]
---

요즘 Spring Reactive로 작성된 코드를 Spring MVC로 변경하면서 Optional을 자주 활용하고 있다.<br/>
예를 들면 `switchIfEmpty()`를 `orElseGet()`으로 대체한다.<br/>
문득 `orElse()`도 있는데, `orElseGet()` 대신 사용해도 괜찮지 않나? 라는 생각이 들었다.<br/>
하지만 두 메소드가 따로 구현된 이유가 있을 것이고, 둘의 차이를 잘 모르는데 아무거나 쓸 수는 없었다.<br/>
그래서 이번 기회에 제대로 이해하고, 어떤 상황에 어떤 것을 써야 하는지 알아야겠다 싶어 정리해본다.<br/>

<br/>

## orElse()

메소드 시그니처를 보면, 매개변수로 `T`를 받고 리턴값으로 `T`를 반환한다.<br/>
메소드 구현을 보면, 값이 null이 아니면 값을 반환하고 null이면 매개변수를 반환한다.<br/>

```java
public T orElse(T other) {
    return this.value != null ? this.value : other;
}
```

<br/>

## orElseGet()

메소드 시그니처를 보면, 매개변수로 Supplier를 받고 리턴값으로 `T`를 반환한다.<br/>
(참고로 Supplier는 매개변수를 받지 않고 리턴값을 반환하는 함수형 인터페이스이다.)<br/>
메소드 구현을 보면, 값이 null이 아니면 값을 반환하고 null이면 Supplier의 `get()`을 호출한다.<br/>

```java
public T orElseGet(Supplier<? extends T> supplier) {
    return this.value != null ? this.value : supplier.get();
}
```

<br/>

## 학습 테스트

소스코드를 까보기만 했을 때는 값이 null일 때 처리하는 로직이 다른 것 외에는 무슨 차이가 있는지 파악하기 어려웠다.<br/>
좀 더 깊이 있게 이해하기 위해 학습 테스트를 진행했다.<br/>

### 코드 작성

다음과 같이 애플리케이션을 만들었다.<br/>
요새 어디든지 떠나고 싶어서, 여행지를 정해주는 매우 간단한 애플리케이션을 구현해봤다.<br/>

```java
// Application.java
public class Application {

    public static void main(String[] args) {
        Trip trip = new Trip();

        // 로직
    }
}

// Trip.java
public class Trip {

    public String choose() {
        System.out.println("여행지 선택 중");

        Destination destination = new Destination();
        String city = destination.choose();

        System.out.println("여행지 선택 완료");

        return city;
    }
}

// Destination.java
public class Destination {

    public String choose() {
        return "Victoria, Canada";
    }
}
```

<br/>

### 값이 null이 아닐 때

#### orElse()

Optional의 값이 null이 아닌데, `orElse()`의 trip.choose()가 실행됐다.<br/>
결과는 기대대로 'San Francisco, USA'로 나왔다.<br/>

```java
// Application.java
public class Application {

    public static void main(String[] args) {
        Trip trip = new Trip();

        String city = Optional.of("San Francisco, USA").orElse(trip.choose());

        System.out.println(city);
    }
}
```

```bash
> Task :Application.main()
여행지 선택 중
여행지 선택 완료
San Francisco, USA
```

#### orElseGet()

Optional의 값이 null이 아니어서, `orElseGet()`의 trip.choose()는 실행되지 않았다.<br/>
결과는 예상대로 'San Francisco, USA'로 나왔다.<br/>

```java
// Application.java
public class Application {

    public static void main(String[] args) {
        Trip trip = new Trip();

        Object city = Optional.of("San Francisco, USA").orElseGet(trip::choose);

        System.out.println(city.toString());
    }
}
```

```bash
> Task :Application.main()
San Francisco, USA
```

<br/>

### 값이 null일 때

#### orElse()

Optional의 값이 null이어서, `orElse()`의 trip.choose()가 실행됐다.<br/>
결과는 기대대로 'Victoria, Canada'가 나왔다.<br/>

```java
public class Application {

    public static void main(String[] args) {
        Trip trip = new Trip();

        String city = Optional.empty().orElse(trip.choose());

        System.out.println(city);
    }
}
```

```bash
> Task :Application.main()
여행지 선택 중
여행지 선택 완료
Victoria, Canada
```

#### orElseGet()

Optional의 값이 null이어서, `orElseGet()`의 trip.choose()가 실행됐다.<br/>
결과는 예상대로 'Victoria, Canada'가 나왔다.<br/>

```java
public class Application {

    public static void main(String[] args) {
        Trip trip = new Trip();

        Object city = Optional.empty().orElseGet(trip::choose);

        System.out.println(city.toString());
    }
}
```

```bash
> Task :Application.main()
여행지 선택 중
여행지 선택 완료
Victoria, Canada
```

<br/>

## 결론

`orElse()`는 Optional의 값이 무엇이든 항상 로직이 실행된다.<br/>
`orElseGet()`은 Optional의 값이 null일 때만 로직이 실행된다.<br/>

만약 요번 예제처럼 `orElse()`/`orElseGet()`에 어떤 객체를 생성하는 코드가 들어갈 때 `orElse()`를 쓴다면,
Optional의 값이 null이 아닌 경우 불필요한 객체가 계속 생성되는 문제가 발생한다.<br/>
이는 성능에 영향을 주고, 힙 메모리 공간을 불필요하게 차지하게 만든다.<br/>
그러므로 Optional의 값이 null일 때만 호출되는 `orElseGet()`을 쓰는 게 좋다.<br/>

그동안 두 메소드의 차이를 정확하게 인지하지 못하고 사용해왔다.<br/>
요번 기회에 확실하게 알게 됐고, 앞으로는 `orElseGet()`을 지향하며 상황에 맞는 메소드를 써야겠다.<br/>

<br/>

## References

- Optional 내부 코드 & 학습 테스트
- [Java Optional – orElse() vs orElseGet()](https://www.baeldung.com/java-optional-or-else-vs-or-else-get)