---
title: '[Algorithms] 이분 탐색 (Binary Search)'
author: da-nyee
date: 2020-10-24 02:25:24 +0900
categories: [TIL, Algorithms]
tags: [algorithms, python, search, binary search]
---

## Binary Search?

Binary Search는 `오름차순 정렬된 자료`에서 `특정 데이터`를 찾는 알고리즘이다.<br/>
Binary Search의 `시간 복잡도`는 `O(logN)`이다.

## Linear Search?

Linear Search는 `정렬되지 않은 자료`에서도 `특정 데이터`를 찾을 수 있는 알고리즘이다.<br/>
Linear Search의 `시간 복잡도`는 `O(N)`이다.

## Comparison

Binary Search는 특정 데이터를 기준으로 `탐색 범위`를 `반씩 줄여간다.`<br/>
Linear Search는 특정 데이터가 나타날 때까지 `탐색`을 `순차적으로 이어간다.`<br/>

`Binary Search`의 `데이터 탐색 속도`가 Linear Search보다 `빠르다.` (단, `오름차순 정렬된 자료`여야 한다.)

## Code

```
# array는 **오름차순 정렬**된 리스트여야 한다.
def binary_search(array, target):
    low, high = 0, len(array)-1

    while low <= high:
        mid = (low+high)//2

        # 목표값을 찾은 경우
        if array[mid] == target:
            # 참을 반환한다.
            return True
        # 현재값 < 목표값인 경우
        elif array[mid] < target:
            # low를 업데이트한다.
            low = mid+1
        # 현재값 > 목표값인 경우
        elif array[mid] > target:
            # high를 업데이트한다.
            high = mid-1
    
    # 목표값을 찾지 못한 경우
    # 거짓을 반환한다.
    return False
```

## Reference

- [강의노트 15-2. [탐색] 이진탐색(Binary Search) 알고리즘](https://wayhome25.github.io/cs/2017/04/15/cs-16/)