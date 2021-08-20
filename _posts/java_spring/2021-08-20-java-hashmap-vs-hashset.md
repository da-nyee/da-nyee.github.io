---
title: '[Java] HashMap vs HashSet'
author: da-nyee
date: 2021-08-20 23:13:15 +0900
categories: [TIL, Java]
tags: [java, hashmap, hashset]
---

## HashMap

- <b>Key : Value</b> 형태
- 내부는 <b>Node 객체</b>를 이용해서 구현되어 있다.

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    ...
}

public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(
    int hash, 
    K key, 
    V value, 
    boolean onlyIfAbsent, 
    boolean evict
) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);

    ...
}

public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
}

final Node<K,V> removeNode(
    int hash, 
    Object key, 
    Object value, 
    boolean matchValue, 
    boolean movable
) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;

        ...
    }

    ...
}
```

<br/>

## HashSet

- <b>Key : Value(Dummy value)</b> 형태
- 내부는 <b>HashMap</b>을 이용해서 구현되어 있다.

```java
private transient HashMap<E,Object> map;

// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();

public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}

public boolean remove(Object o) {
    return map.remove(o)==PRESENT;
}
```

<br/>

## References

- HashMap 내부 코드
- HashSet 내부 코드