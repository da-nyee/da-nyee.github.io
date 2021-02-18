---
title: '[Java] 스트림 (Stream)'
author: da-nyee
date: 2021-02-19 02:02:36 +0900
categories: [TIL, Java]
tags: [java, stream]
---

## Stream

- Java 8부터 추가
- 컬렉션 및 배열의 저장 요소를 하나씩 참조해서 <b>람다식</b>으로 처리할 수 있도록 해주는 반복자

### Features

- 람다식으로 요소 처리 코드를 제공한다.
    - 스트림이 제공하는 대부분의 요소 처리 메소드는 함수적 인터페이스 매개 타입을 가진다.
    - 따라서, <b>람다식</b> 또는 <b>메소드 참조</b>를 이용해서 요소 처리 내용을 매개값으로 전달할 수 있다.

```
students.stream()
            .forEach(s -> {
                String name = s.getName();
                int score = s.getScore();
                System.out.println(name + ": " + score);
            });
```

- 내부 반복자를 사용하므로 병렬 처리가 쉽다.
    - 외부 반복자(external iterator): 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴 (ex. index를 이용하는 for 문, Iterator를 이용하는 while 문)
    - 내부 반복자(internal iterator): 컬렉션 내부에서 요소들을 반복시키고, 개발자는 요소당 처리해야 할 코드만 제공하는 코드 패턴
    - 내부 반복자를 사용해서 얻는 이점은?
        - 컬렉션 내부에서 어떻게 요소를 반복시킬 것인가는 컬렉션에게 맡겨둔다.
        - 개발자는 요소 처리 코드에만 집중할 수 있다.
        - 또한, 내부 반복자는 요소들의 반복 순서를 변경하거나, 멀티 코어 CPU를 최대한 활용하기 위해 요소들을 분배시켜 <b>병렬 작업</b>을 할 수 있게 도와준다.
        - 따라서, 하나씩 처리하는 순차적 외부 반복자보다는 <b>효율적으로 요소를 반복</b>시킬 수 있다.
    - 스트림은 람다식으로 요소 처리 내용만 전달하고, 반복은 컬렉션 내부에서 일어난다.
    - 스트림을 사용하면 코드가 간결해지고, 요소의 병렬 처리가 컬렉션 내부에서 진행되어 일석이조의 효과를 가져온다.
    - 병렬 처리 스트림을 이용하면?
        - <b>런타임 시</b> 하나의 작업을 서브 작업으로 자동으로 나눈다.
        - 서브 작업의 결과를 자동으로 결합해서 최종 결과물을 생성한다.

```
public class Application {
    public static void main(String[] args) {
        List<String> names = Arrays.asList(
                "홍길동", "어피치", "네이비", "옐로우", "그레이"
        );

        // 순차 처리
        names.stream()
                .forEach(Application::print);
        System.out.println();

        // 병렬 처리
        names.parallelStream()
                .forEach(Application::print);
    }

    public static void print(String name) {
        System.out.println(name + ": " + Thread.currentThread().getName());
    }
}
```
```
// 결과
홍길동: main
어피치: main
네이비: main
옐로우: main
그레이: main

네이비: main
옐로우: ForkJoinPool.commonPool-worker-2
그레이: main
홍길동: ForkJoinPool.commonPool-worker-2
어피치: ForkJoinPool.commonPool-worker-1
```

- 중간 처리와 최종 처리를 할 수 있다.
    - <b>중간 처리에서는 매핑, 필터링, 정렬</b> 등을 수행한다.
    - <b>최종 처리에서는 반복, 카운팅, 평균, 총합</b> 등의 집계 처리를 수행한다.

```
public class Application {
    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
                new Student("홍길동", 10),
                new Student("어피치", 20),
                new Student("네이비", 30)
        );

        double averageScore = students.stream()
                .mapToInt(Student::getScore)    // 중간 처리
                .average()                      // 최종 처리
                .getAsDouble();

        System.out.println("평균 점수: " + averageScore);
    }
}
```
```
// 결과
평균 점수: 20.0
```

### Types

- <b>java.util.stream</b> 패키지에 stream API들이 포진되어 있다.

![stream_types](https://user-images.githubusercontent.com/50176238/108312237-bc9af780-71f9-11eb-9111-8833622b323e.PNG)<br/>

- <b>BaseStream 인터페이스</b>에는 모든 스트림에서 사용할 수 있는 <b>공통 메소드들</b>이 정의되어 있다. 코드에서 직접 사용하지는 않는다.
- <b>Stream, IntStream, LongStream, DoubleStream</b>은 BaseStream 인터페이스의 <b>구현체들</b>이다. 코드에서 직접 사용된다.
    - Stream: 객체 요소를 처리하는 스트림
    - IntStream: int 요소를 처리하는 스트림
    - LongStream: long 요소를 처리하는 스트림
    - DoubleStream: double 요소를 처리하는 스트림
- 숫자 범위로부터 스트림을 얻는 방법?
    - IntStream의 <b>rangeClosed()</b>를 이용한다.
        - <b>첫 번째 매개값부터 두 번째 매개값</b>까지 순차적으로 제공한다.
    - IntStream의 <b>range()</b>를 이용한다.
        - <b>첫 번째 매개값부터 두 번째 매개값-1</b>까지 순차적으로 제공한다.

```
public class Application {
    public static int sum;

    public static void main(String[] args) {
        IntStream.rangeClosed(1, 100)
                .forEach(value -> sum += value);
        System.out.println("총합: " + sum);
    }
}
```
```
// 결과
총합: 5050
```

### Methods

- 필터링: distinct(), filter()
- 매핑: flatMapXXX(), mapXXX(), asXXXStream(), boxed()
- 정렬: sorted()
- 반복: peek(), forEach()
- 매칭: allMatch(), anyMatch(), noneMatch()
- 기본 집계: sum(), count(), average(), max(), min()
- 커스텀 집계: reduce()
- 수집: collect()

## Reference

- [이것이 자바다](https://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968481475&orderClick=JAj)