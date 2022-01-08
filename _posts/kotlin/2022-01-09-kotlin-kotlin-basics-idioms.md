---
title: '[Kotlin] Kotlin Basics - Idioms'
author: da-nyee
date: 2022-01-09 00:48:41 +0900
categories: [TIL, Kotlin]
tags: [kotlin, basics]
---

- 코틀린의 관용구를 파헤쳐보자 !

<br/>

## DTO

- <b>data</b> 키워드를 사용한다.
- 아래 항목은 자동으로 생성된다.
    - getter
    - setter
        - val (X)
        - var (O)
    - equals()
    - hashCode()
    - toString()

```kotlin
data class UserDto(val name: String, val email: String)
```

<br/>

## 객체 타입 확인

```kotlin
when (obj) {
    is Foo  -> ...
    is Bar  -> ...
    else    -> ...
}
```

<br/>

## Read-only

### list

```kotlin
val items = listOf("a", "b", "c")
```

### map

- <b>key</b> to <b>value</b> 형태

```kotlin
val items = mapOf("a" to 1, "b" to 2, "c" to 3)
```

<br/>

## 범위 반복

### closed range

```kotlin
// 1..100 == 1 <= .. <= 100
for (i in 1..100) { ... }
```

### half-open range

```kotlin
// 1 until 100 == 1 <= .. < 100
for (i in 1 until 100) { ... }
```

<br/>

## Lazy

- <b>lazy</b> 키워드를 사용하면, 호출 시점에 초기화된다.

```kotlin
// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val item: String by lazy {
        println("Initialize the item")
        "hi"
    }

    println("Start")
    println("item 1: $item")
    println("item 2: $item")
    println("item 3: $item")
    println("End")
}

// 결과
Start
Initialize the item
item 1: hi
item 2: hi
item 3: hi
End
```

<br/>

## 싱글턴

- <b>object</b> 키워드를 사용한다.

```kotlin
object User {
    val name = "dani"
}
```

<br/>

## ?:

- 엘비스 연산자

### if-not-null

```kotlin
val files = File("Test").listFiles()

// 만약 files가 null이 아니라면, files의 사이즈를 출력한다.
println(files?.size)
```

### if-not-null-else

```kotlin
val files = File("Test").listFiles()

// 만약 files가 null이라면, "empty"를 출력한다.
println(files?.size ?: "empty")
```

### if-null-else

```kotlin
// items의 key에 name이 없다면, IllegalStateException을 던진다.
val items = ...
val name = items["name"] ?: throw IllegalStateException("이름이 없습니다.")

// emails가 비었다면, ""을 반환한다.
val emails = ...
val main = emails.firstOrNull() ?: ""
```

<br/>

## let

- <b>T?.let { }</b>에서 <b>let 블럭 안에는 non-null 값만</b> 들어갈 수 있다.
- 만약 null 값인 경우에는 앞에서 본 <b>?:</b>를 사용해서 기본값을 지정할 수 있다.

```kotlin
val user = ...
val name = user?.let { it.name } ?: "dani"
```

<br/>

## with

- 객체의 메서드를 여러 개 호출할 때, <b>그룹화</b>하는 용도로 사용한다.

```kotlin
// Person.kt
class Person {

    fun standUp()
    fun sitDown()
    fun moveForward(steps: Int)
    fun moveBack(steps: Int)
}

// Application.kt
fun main(args: Array<String>) {
    runApplication<Application>(*args)

    val person = Person()

    with(person) {
        standUp()
        (1..4).forEach {
            moveForward(10)
            moveBack(6)
        }
        sitDown()
    }
}
```

<br/>

## try-with-resources

### Java 코드

```java
public String readLine(String path) {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    } catch (IOException e) {
        ...
    }
}
```

### Kotlin 코드

```kotlin
fun readLine(path: String): String {
    BufferedReader(FileReader(path)).use { br ->
        return br.readLine()
    }
}
```

<br/>

## swap

- <b>also</b> 키워드를 사용한다.

```kotlin
var a = 1
var b = 2

a = b.also { b = a }

println("a: $a, b: $b")

// 결과
a: 2, b: 1
```

<br/>

## Reference

- [코틀린 공식 문서 - Idioms](https://kotlinlang.org/docs/idioms.html)