---
title: '[Algorithms] 최대공약수, 최소공배수 (GCD, LCM)'
author: da-nyee
date: 2021-12-31 17:58:20 +0900
categories: [TIL, Algorithms]
tags: [algorithms, python, java, gcd, lcm]
---

## 최대공약수 (GCD, Greatest Common Divisor)

- 유클리드 호제법

### Python 코드

```python
def gcd(a, b):
    if a < b:
        a, b = b, a
    
    while b != 0:
        a, b = b, a % b
    
    return a
```

### Java 코드

```java
public static int gcd(int a, int b) {
    if (a < b) {
        int temp = a;
        a = b;
        b = temp;
    }

    while (b != 0) {
        int temp = a % b;
        a = b;
        b = temp;
    }

    return a;
}
```

<br/>

## 최소공배수 (LCM, Least Common Multiple)

- 두 수의 곱을 최대공약수로 나눈다.

### Python 코드

```python
def lcm(a, b):
    return a * b // gcd(a, b)
```

### Java 코드

```java
public static int lcm(int a, int b) {
    return a * b / gcd(a, b);
}
```

<br/>

## Reference

- [[알고리즘- 파이썬] 최대공약수(유클리드 호제법), 최소공배수](https://velog.io/@jwisgenius/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C-%ED%98%B8%EC%A0%9C%EB%B2%95-%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98)