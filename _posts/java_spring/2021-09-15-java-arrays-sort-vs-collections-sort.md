---
title: '[Java] Arrays.sort() vs Collections.sort()'
author: da-nyee
date: 2021-09-15 22:41:28 +0900
categories: [TIL, Java]
tags: [java, sort]
---

## Arrays.sort()

- Dual-Pivot Quick Sort 활용 👉 O(nlogn) ~ O(n^2)

### Dual-Pivot Quick Sort

- One-Pivot Quick Sort보다 빠른 편
- 배열이 이미 정렬되어 있는 경우에는 시간 복잡도가 여전히 O(n^2)

```java
// Arrays.java
public static void sort(int[] a) {
    DualPivotQuicksort.sort(a, 0, a.length - 1, null, 0, 0);
}
```

<br/>

## Collections.sort()

- Merge Sort or Tim Sort(Insertion Sort + Merge Sort) 활용 👉 O(nlogn)

### Tim Sort

- O(nlogn) 정렬 알고리즘의 단점을 최대한 극복
- Merge Sort보다 적은 추가 메모리를 사용하기 때문에 공간 복잡도가 낮음

```java
// Collections.java
public static <T extends Comparable<? super T>> void sort(List<T> list) {
    list.sort(null);
}

// List.java
default void sort(Comparator<? super E> c) {
    Object[] a = this.toArray();
    Arrays.sort(a, (Comparator) c);
    ListIterator<E> i = this.listIterator();
    for (Object e : a) {
        i.next();
        i.set((E) e);
    }
}

// Arrays.java
public static <T> void sort(T[] a, Comparator<? super T> c) {
    if (c == null) {
        sort(a);
    } else {
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a, c);
        else
            TimSort.sort(a, 0, a.length, c, null, 0, 0);
    }
}

private static <T> void legacyMergeSort(T[] a, Comparator<? super T> c) {
    T[] aux = a.clone();
    if (c==null)
        mergeSort(aux, a, 0, a.length, 0);
    else
        mergeSort(aux, a, 0, a.length, 0, c);
}
```

<br/>

## References

- Arrays, Collections 내부 코드
- [Dual pivot Quicksort](https://www.geeksforgeeks.org/dual-pivot-quicksort/)
- [Tim sort에 대해 알아보자](https://d2.naver.com/helloworld/0315536)