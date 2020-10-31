---
title: '[Algorithms] 정렬 알고리즘 (Sorting Algorithms)'
author: da-nyee
date: 2020-10-31 20:48:31 +0900
categories: [TIL, Algorithms]
tags: [algorithms, python, bubble sort, selection sort, insertion sort, shell sort, merge sort, quick sort]
---

## Bubble Sort

```
def bubble_sort(arr=[4, 9, 2, 5, 7]):
    for i in range(1, len(arr)):
        for j in range(1, len(arr)):
            if arr[j-1] > arr[j]:
                arr[j-1], arr[j] = arr[j], arr[j-1]

    return arr

print("bubble sort:", bubble_sort())
```

```
bubble sort: [2, 4, 5, 7, 9]
```

## Selection Sort

```
def selection_sort(arr=[8, 4, 7, 2, 3]):
    for i in range(0, len(arr)):
        for j in range(i+1, len(arr)):
            if arr[i] > arr[j]:
                arr[i], arr[j] = arr[j], arr[i]

    return arr

print("selection sort:", selection_sort())
```

```
selection sort: [2, 3, 4, 7, 8]
```

## Insertion Sort

```
def insertion_sort(arr=[8, 9, 3, 1, 6]):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i-1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key

    return arr

print("insertion sort:", insertion_sort())
```

```
insertion sort: [1, 3, 6, 8, 9]
```

## Shell Sort

```
def gap_insertion_sort(arr, start, gap):
    for target in range(start+gap, len(arr), gap):
        val = arr[target]
        idx = target
        while idx > start:
            if arr[idx-gap] > val:
                arr[idx] = arr[idx-gap]
            else:
                break
            idx -= gap
        arr[idx] = val

def shell_sort(arr=[8, 4, 2, 3, 7]):
    gap = len(arr)//2
    while gap > 0:
        for start in range(0, gap):
            gap_insertion_sort(arr, start, gap)
        gap = gap//2

    return arr

print("shell sort:", shell_sort())
```

```
shell sort: [2, 3, 4, 7, 8]
```

## Merge Sort

```
def merge_sort(arr=[3, 2, 5, 9, 6]):
    if len(arr) > 1:
        mid = len(arr)//2
        larr, rarr = arr[:mid], arr[mid:]
        merge_sort(larr); merge_sort(rarr)

        li, ri, i = 0, 0, 0
        while li < len(larr) and ri < len(rarr):
            if larr[li] < rarr[ri]:
                arr[i] = larr[li]
                li += 1
            else:
                arr[i] = rarr[ri]
                ri += 1
            i += 1
        
        if li != len(larr):
            arr[i:] = larr[li:]
        else:
            arr[i:] = rarr[ri:]

    return arr

print("merge sort:", merge_sort())
```

```
merge sort: [2, 3, 5, 6, 9]
```

## Quick Sort

```
def quick_sort(arr=[8, 1, 7, 2, 5]):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr)//2]

    larr, rarr, marr = [], [], []
    for i in range(0, len(arr)):
        if arr[i] < pivot:
            larr.append(arr[i])
        elif arr[i] > pivot:
            rarr.append(arr[i])
        elif arr[i] == pivot:
            marr.append(arr[i])
    
    return quick_sort(larr)+marr+quick_sort(rarr)

print("quick sort:", quick_sort())
```

```
quick sort: [1, 2, 5, 7, 8]
```

## Time Complexity

|<center><b>Type</b></center>|<center><b>Best</b></center>|<center><b>Worst</b></center>|<center><b>Stable</b></center>|<center><b>Memory</b></center>|
|----------------------------|----------------------------|-----------------------------|------------------------------|------------------------------|
|<center>Bubble Sort</center>|<center>n<sup>2</sup></center>|<center>n<sup>2</sup></center>|<center>T</center>|<center>1</center>|
|<center>Selection Sort</center>|<center>n<sup>2</sup></center>|<center>n<sup>2</sup></center>|<center>F</center>|<center>1</center>|
|<center>Insertion Sort</center>|<center>n</center>|<center>n<sup>2</sup></center>|<center>T</center>|<center>1</center>|
|<center>Shell Sort</center>|<center>n</center>|<center>n<sup>1.5</sup> ~ n<sup>2</sup></center>|<center>F</center>|<center>1</center>|
|<center>Merge Sort</center>|<center>nlogn</center>|<center>nlogn</center>|<center>T</center>|<center>n</center>|
|<center>Quick Sort</center>|<center>nlogn</center>|<center>nlogn ~ n<sup>2</sup></center>|<center>F</center>|<center>logn ~ n</center>|

## References

- [파이썬으로 정렬 알고리즘 구현하기](http://ejklike.github.io/2017/03/04/sorting-algorithms-with-python.html)
- [파이썬을 이용한 정렬 알고리즘](https://seongjaemoon.github.io/python/2017/12/16/pythonSort.html)
- [[알고리즘] 삽입 정렬(insertion sort)이란](https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html)
- [Python 쉘정렬 구현](https://zetawiki.com/wiki/Python_%EC%89%98%EC%A0%95%EB%A0%AC_%EA%B5%AC%ED%98%84)
- [알고리즘 퀵 정렬(quick sort)란? 파이썬으로 구현까지](https://lsjsj92.tistory.com/482)