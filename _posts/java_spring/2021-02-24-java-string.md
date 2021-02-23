---
title: '[Java] 문자열 (String)'
author: da-nyee
date: 2021-02-24 03:10:18 +0900
categories: [TIL, Java]
tags: [java, string]
---

## String

- 왜 `문자`가 아닌 `문자열`이라고 할까? `문자의 배열`이기 때문이다.
- 자바 프로그램이 실행되는 동안 `가장 많이 생성되는 객체`이다.

### String Pool

- 일종의 보관소
- 내부적으로 생성되는 String 객체를 instance로 보관하고 있다.
- 예를 들어, 내가 "abc"를 생성하고 다시 "abc"를 생성하려고 할 때, String Pool을 확인해서 있는지 없는지 체크하고, 있으면 그걸 그대로 꺼내온다.
- `new String()`으로 생성하면 내용이 동일해도 계속 생성해서 성능이 저하되는 반면, `""`으로 생성하면 `String Pool`에서 가져오니까 성능이 저하될 일이 없을 것 같다.

```
@Test
void createString() {
    final String one = new String("abc");
    final String two = new String("abc");           // new 키워드로 생성하지 말 것
    assertThat(one).isSameAs(two);                  // fail
}
```
```
@Test
void createString() {
    final String one = "abc";
    final String two = "abc";                       // ""로 생성할 것
    assertThat(one).isSameAs(two);                  // pass
}
```

### Byte Code

- `build` 또는 `out` 패키지에 모여 있다.

```
// src 패키지
@Test
void sumString() {
    String a = "a";
    String b = "b";
    String c = "c";
    System.out.println(a + b + c);
}

// build 패키지
@Test
void sumString() {
    String a = "a";
    String b = "b";
    String c = "c";
    System.out.println(a + b + c);
}
```
```
// src 패키지
@Test
void sumString() {
    final String a = "a";           // final 키워드 👉 극한의 최적화
    final String b = "b";
    final String c = "c";
    System.out.println(a + b + c);
}

// build 패키지
@Test
void sumString() {
    String a = "a";
    String b = "b";
    String c = "c";
    System.out.println("abc");      // Java compiler가 이렇게 만듦
                                    // 어차피 변하지 않는 상수임을 알고 있어서 그냥 바로 합친다.
}
```

### intern()

- String 객체에 `intern()`이 붙어 있으면 new 키워드로 생성하지 않고, `String Pool`에서 가져온다.

```
@Test
void createStringIntern() {
    String obj1 = new String("hello");
    String obj2 = "hello";
    String obj3 = obj1.intern();

    assertThat(obj1).isSameAs(obj2);        // fail
    assertThat(obj2).isSameAs(obj3);        // pass
}
```

## String + String vs StringBuilder

- `String은 불변(immutable)`하기 때문에 `String + String == 새로운 String 객체`를 생성한다.
    - String + String 하는 시점에 `메모리 할당과 메모리 해제`가 계속 발생한다.
- `StringBuilder는 가변(mutable)`하기 때문에 `기존 데이터에 새로운 데이터를 더하는 방식`을 취한다.
    - 속도가 더 빠르다.
    - `toString()` 하기 전까지 내부에서 instance를 String화 하지 않는다.
- 따라서, 긴 문자열을 더하는 상황이 발생하는 경우 `StringBuilder`를 활용해서 구현한다.

### Immutable

- `불변(immutable)`이란 객체를 생성한 후 `객체의 상태를 변경할 수 없는 것`을 의미한다.
- `String 객체`는 문자열의 상태 값을 변경할 수 없기 때문에 `불변 객체`라고 한다.

### Index Check

- Java는 Python과 다르게 `charAt(-1)` 했을 때 `Exception`이 발생한다.

```
@Test
void indexString() {
    final char curr1 = "abcde".charAt(0);
    assertThat(curr1).isEqualTo('a');       // pass

    final char curr2 = "abcde".charAt(-1);
    assertThat(curr2).isEqualTo('e');       // fail 👉 StringIndexOutOfBoundsException
}
```

## To join strings.. String vs StringBuilder?

- 경우에 따라 결정한다.
- Java compiler가 최적화를 해서 곧바로 만드는 경우가 있는 반면, 그렇지 않고 StringBuilder를 사용해야 하는 경우가 있다.
    - ex. `final 키워드`가 붙어 있으면 최적화를 해서 곧바로 만들어 준다.
        - 가급적 로컬 변수에 `final`을 붙이는 게 좋다. `for 최적화`
    - ex. `for 문`을 통해 반복시키면 `계속 String instance를 생성하나?` 최적화 없이 매번 생성하나?
        - `JDK 1.8`은 `내부에서 StringBuilder`를 만든다. 👉 따라서 `매번 생성하지 않는다!`
        - `JDK 1.5부터 변하지 않는다`고 한다.
- 숫자에 `_`를 붙이면 실생활의 `,`처럼 사용할 수 있다.
    - ex. 100000 == 100_000 (코드) == 100,000 (실생활)
- Byte code를 보면 알 수 있듯이,
    - `toString 여부`에 따라 `시간 차이가 발생`한다.
    - `append` 자체는 `큰 차이가 나지 않는다.`

```
@Test
void formatString() {
    final long start = System.currentTimeMillis();

    String a = "";
    for (int i = 0; i < 100_000; i++) {
        a += i;
    }

    final long end = System.currentTimeMillis();
    System.out.println(end - start);                // 32610
}

// Byte code
// toString()이 있다.
NEW java/lang/StringBuilder
DUP
INVOKESPECIAL java/lang/StringBuilder.<init> ()V
ALOAD 3
INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)Ljava/lang/StringBuilder;
ILOAD 4
INVOKEVIRTUAL java/lang/StringBuilder.append (I)Ljava/lang/StringBuilder;
INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
ASTORE 3
```
```
@Test
void formatString() {
    final long start = System.currentTimeMillis();

    final StringBuilder builder = new StringBuilder();
    for (int i = 0; i < 100_000; i++) {
        builder.append(i);
    }

    final long end = System.currentTimeMillis();
    System.out.println(end - start);                // 11
}

// Byte code
// toString()이 없다.
ALOAD 3
ILOAD 4
INVOKEVIRTUAL java/lang/StringBuilder.append (I)Ljava/lang/StringBuilder;
POP
```

> 🗣 파즈: (new StringBuilder()).append(a).append(i); 를 반복문만큼 하는데 a는 계속 길어져서 빌더 할당 시간이 계속해서 늘어나니까 반복문에서 효율이 안 나오나봐요.<br/>
> 🗣 씨유: 아까 파즈가 답을 했던 거 같은데요. 반복문 안에서 `+`를 사용하면 `StringBuilder 임시 객체를 계속 만들어서` 느려지는 거 같아요.<br/>

## References

- 제이슨 강의 - 자바 문법 및 개념 이해 on Feb 19
- [JAVA String, StringBuffer, StringBuilder 차이점](https://jeong-pro.tistory.com/85)
- [Java String intern()](https://www.javatpoint.com/java-string-intern)