---
title: '[Kotlin] Kotlin Basics - Basic Syntax'
author: da-nyee
date: 2022-01-08 01:32:32 +0900
categories: [TIL, Kotlin]
tags: [kotlin, basics]
---

- 코틀린의 기본 문법을 파헤쳐보자 !

<br/>

## 함수

### 리턴값이 있는 경우

```kotlin
// Application.kt
fun main(args: Array<String>) {

    runApplication<Application>(*args)

    print(sum(1, 2))
}

fun sum(a: Int, b: Int): Int {
    return a + b
}

// 결과
3
```

### 리턴값이 없는 경우

- Unit
    - 자바의 void와 같은 타입이다.
    - 명시를 생략할 수 있다.

```kotlin
// Application.kt
fun main(args: Array<String>) {

    runApplication<Application>(*args)

    print(sum(1, 2))
}

fun sum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b})
}

// 결과
3
kotlin.Unit
```

<br/>

## 변수

### val

- value
- 자바의 final처럼 불변값을 만들 때 사용한다.

```kotlin
// 초기화
val a: Int = 1

// 타입 추론
val b = 2

// 초기값이 주어지지 않으면, 타입 추론을 할 수 없다.
val c: Int
c = 3
```

### var

- variable
- 문자 그대로 변하는 수를 만들 때 사용한다.

```kotlin
// 초기화
var a: Int = 1
a += 1

// 타입 추론
var b = 5
b += 1
```

<br/>

## 클래스

- 만약 클래스에 생성자가 없다면, 코틀린이 자동으로 기본 생성자(매개변수가 없는 생성자 X)를 만들어준다.

```kotlin
// Rectangle.kt
class Rectangle(var width: Double, var height: Double) {

    var perimeter = (width + height) * 2
}

// Application.kt
fun main(args: Array<String>) {

    runApplication<Application>(*args)

    val rectangle = Rectangle(2.0, 5.0)

    println("The perimeter is ${rectangle.perimeter}")
}

// 결과
The perimeter is 14.0
```

<br/>

## 상속

- 상속할 때는 <b>콜론(:)</b> 키워드를 사용한다.
- 클래스는 default로 불변이다.
- 따라서, 상속을 할 수 있게 만드려면 <b>open</b> 키워드를 사용한다.

```kotlin
open class Shape

class Rectangle(var width: Double, var height: Double): Shape() {

    var perimeter = (width + height) * 2
}
```

<br/>

## 조건문

- 아래와 같이 2가지 방식으로 쓸 수 있다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    println(findBigger1(1, 2))
    println(findBigger2(1, 2))
}

fun findBigger1(a: Int, b: Int): Int {
    if (a > b) {
        return a
    }
    return b
}

fun findBigger2(a: Int, b: Int) = if (a > b) a else b

// 결과
2
2
```

<br/>

## 반복문

### for

- 코틀린은 자바의 for-each 문만 제공한다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val items = listOf("apple", "banana", "orange")

    for (item in items) {
        println(item)
    }
    println()

    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
}

// 결과
apple
banana
orange

item at 0 is apple
item at 1 is banana
item at 2 is orange
```

### while

- 자바의 while 문과 똑같다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val items = listOf("apple", "banana", "orange")
    var index = 0

    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }
}

// 결과
item at 0 is apple
item at 1 is banana
item at 2 is orange
```

<br/>

## 범위

- 자바 컬렉션의 contains()와 같다.

```kotlin
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val x = 10
    val y = 9

    // 1..y + 1 == 1 <= .. <= y + 1
    if (x in 1..y + 1) {
        println("fits in range")
    }
}

// 결과
fits in range
```

- 자바 컬렉션의 !contains()와 같다.

```kotlin
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val items = listOf("a", "b", "c")

    if (-1 !in 0..items.lastIndex) {
        println("-1 is out of range")
    }
    if (items.size !in items.indices) {
        println("the size is out of valid indices range, too")
    }
}

// 결과
-1 is out of range
the size is out of valid indices range, too
```

- 반복문에도 범위를 쓸 수 있다.

```kotlin
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    for (x in 1..5) {
        println(x)
    }
    println()

    for (x in 1..10 step 2) {
        println(x)
    }
    println()

    // 9 downTo 0 == 9 >= .. >= 0
    for (x in 9 downTo 0 step 3) {
        println(x)
    }
}

// 결과
1
2
3
4
5

1
3
5
7
9

9
6
3
0
```

<br/>

## 컬렉션

- 자바의 스트림과 비슷하다.
- 컬렉션에 <b>람다식({ })</b>을 쓸 수 있다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val items = listOf("apple", "banana", "avocado", "orange")

    items
        .filter { it.startsWith("a") }
        .sortedBy { it }
        .map { it.uppercase() }
        .forEach { println(it) }
}

// 결과
APPLE
AVOCADO
```

<br/>

## Nullable 값

- 데이터 타입 끝에 <b>?</b> 키워드를 붙이면, null을 허용한다는 뜻이다.
- 아래 코드는 리턴값이 Integer가 아니면, null을 반환한다.

```kotlin
fun parseInt(str: String): Int? {
    ...
}
```

<br/>

## 타입 체크

- 자바의 instanceof와 같다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    println(getLength("hi"))
}

fun getLength(obj: Any): Int? {
    if (obj is String) {
        return obj.length
    }
    return null
}
```

<br/>

## Reference

- [코틀린 공식 문서 - Basic syntax](https://kotlinlang.org/docs/basic-syntax.html)