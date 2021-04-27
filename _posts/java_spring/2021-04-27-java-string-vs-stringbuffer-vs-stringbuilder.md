---
title: '[Java] String vs StringBuffer vs StringBuilder'
author: da-nyee
date: 2021-04-27 17:16:26 +0900
categories: [TIL, Java]
tags: [java, string, stringbuffer, stringbuilder]
---

## String vs StringBuffer, StringBuilder

- String은 불변(immutable)
- StringBuffer, StringBuilder는 가변(mutable)

### String

- String은 내부의 문자열을 수정할 수 없다. 👉 크기가 고정되어 있기 때문이다.
- 따라서, 문자열을 수정할 때마다 새로운 문자열을 반환한다.

```
String str = "a";
System.out.println("str = " + str);

str += "b";
System.out.println("str = " + str);
```
```
// 결과
str = a
str = ab
```

- 기존 String 객체 `a`에 b를 추가하면 새로운 String 객체 `ab`가 생성된다.
    - `str` 변수는 `기존 String 객체와의 참조를 끊고 새로운 String 객체를 참조한다.`
    - 기존 String 객체는 참조되지 X 👉 GC의 메모리 해제를 기다린다.
- String으로 문자열을 합치는 연산을 할수록 사용하지 않는 String 객체 수가 늘어난다. 👉 프로그램 성능이 느려진다.

<img width="562" alt="string" src="https://user-images.githubusercontent.com/50176238/116208317-c4d45d80-a77b-11eb-87ff-e9dbb8b85e9c.png">

### StringBuffer, StringBuilder

- StringBuffer, StringBuilder는 내부의 문자열을 수정할 수 있다.
- 동일 객체 내에서 문자열을 변경하는 게 가능하다.

```
StringBuffer stringBuffer = new StringBuffer("a");
stringBuffer.append("b");
System.out.println("stringBuffer = " + stringBuffer);

StringBuilder stringBuilder = new StringBuilder("a");
stringBuilder.append("b");
System.out.println("stringBuilder = " + stringBuilder);
```
```
// 결과
stringBuffer = ab
stringBuilder = ab
```

- 두 클래스는 내부 Buffer에 문자열을 저장해두고 그 안에서 변경 작업을 할 수 있도록 설계되어 있다.
- 따라서, String처럼 새로운 객체를 만들지 않아도 문자열을 수정할 수 있다.
- 그러므로 `문자열을 변경하는 작업이 많을 경우` String < `StringBuffer, StringBuilder`

<img width="456" alt="stringbuffer_stringbuilder" src="https://user-images.githubusercontent.com/50176238/116208631-1381f780-a77c-11eb-8d2e-98940c075120.png">

## StringBuffer vs StringBuilder

- 두 클래스의 차이점은 `동기화의 유무`에 있다.
    - StringBuffer는 `synchronized` 키워드 O
        - 멀티 쓰레드 환경에서 안전하다. 👉 thread-safe O
        - 참고로, String은 불변이기 때문에 이 역시도 thread-safe O
    - StringBuilder는 `synchronized` 키워드 X
        - 멀티 쓰레드 환경에서 안전하지 않다. 👉 thread-safe X
        - 단일 쓰레드에서의 성능은 StringBuffer보다 우수하다.

```
// StringBuffer 👉 synchronized 키워드가 있다.
@Override
public synchronized StringBuffer append(Object obj) {
    toStringCache = null;
    super.append(String.valueOf(obj));
    return this;
}

// StringBuilder 👉 synchronized 키워드가 없다.
@Override
public StringBuilder append(Object obj) {
    return append(String.valueOf(obj));
}
```

## Conclusion

|String|StringBuffer|StringBuilder|
|:----:|:----------:|:-----------:|
|문자열 연산이 적은 경우|문자열 연산이 많은 경우|문자열 연산이 많은 경우|
|멀티 쓰레드 환경인 경우|멀티 쓰레드 환경인 경우|단일 쓰레드 환경인 경우<br/>or<br/>동기화를 고려하지 않아도 되는 경우|

## References

- [[Java] String, StringBuffer, StringBuilder 차이 및 장단점](https://ifuwanna.tistory.com/221)
- [[Java] String, StringBuffer, StringBuilder의 차이점과 사용이유](https://coding-factory.tistory.com/546)