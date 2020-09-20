---
title: '[Python] 양의 무한대(inf), 음의 무한대(-inf) 표시'
author: da-nyee
date: 2020-09-20 23:58:48 +0900
categories: [TIL, Python]
tags: [python, infinity]
---

## Introduction

알고리즘 문제 풀이 시 최소값, 최대값 구할 때 자주 사용하는 무한대.<br/>
두 가지 방법이 있다.

## 'float' Data Type

무한수는 float형에만 적용 가능하다. int형에는 적용 불가능하다.
```
>>> positive = float("inf")          # 양의 무한대
>>> print(positive)
inf

>>> negative = float("-inf")         # 음의 무한대
>>> print(negative)
-inf

>>> positive = int(float("inf"))
>>> print(positive)
OverflowError: cannot convert float infinity to integer         # 에러 발생

>>> negative = int(float("-inf"))
>>> print(negative)
OverflowError: cannot convert float infinity to integer         # 에러 발생
```

## Mathematical Function

해당 함수는 <b>Python 3.5 이상</b> 사용할 수 있다.
```
>>> import math

>>> positive = math.inf              # 양의 무한대
>>> print(positive)
inf

>>> negative = -math.inf             # 음의 무한대
>>> print(negative)
-inf
```