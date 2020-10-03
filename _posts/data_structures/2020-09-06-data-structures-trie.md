---
title: '[Data Structures] 트라이 (Trie)'
author: da-nyee
date: 2020-09-06 23:19:17 +0900
categories: [TIL, Data Structures]
tags: [data structures, python, tree, trie]
---

## Introduction

여러 알고리즘 문제들을 풀다가 접한 `Trie` 자료구조.

처음에는 이름이 생소해서 Tree를 잘못 쓴 건 아닐까 생각했다.<br/>
근데 그건 아니었고, `Tree의 한 종류`로 `문자열 검색에 특화된 자료구조`였다!

Trie는 `자동 완성(Autocomplete)`를 생각하면 이해하기 쉽다.

엄청난 양의 데이터에서 특정 키워드로 자동 완성을 구현한다고 가정해보자.<br/>
문자열 검색에서 선형 구조인 List는 시간 복잡도가 크기 때문에 비효율적이다.<br/>
반면, `비선형 구조`인 Trie는 시간 복잡도가 작기 때문에 효율적이다.<br/>
따라서, 이런 경우에는 List보다는 Trie를 사용하는 것이 좋다.

## Trie?

Trie는 `검색`을 뜻하는 영단어 `retrieval`에서 나온 단어이다.<br/>
보통 Trie라고 부르지만, `Prefix Tree`라고도 한다.

Trie는 문자열 검색 시 `시간 복잡도`가 `O(m)` 밖에 되지 않는다는 `장점`이 있다. (m: 문자열 최대 길이)<br/>
한편, `공간 복잡도`는 `O(포인터 크기 * 포인터 배열 개수 * 총 노드 개수)`로 크다는 `단점`이 있다.<br/>
ex1. 숫자에 대해 Trie를 생성하면 0-9인 총 10개의 포인터 배열을 가져야 한다.<br/>
ex2. 알파벳에 대해 Trie를 생성하면 a-z인 총 26개의 포인터 배열을 가져야 한다.

## Code

```
# Node 구현
class Node(object):
    def __init__(self, key, data=None):
        self.key = key
        self.data = data
        self.children = {}

# Trie 구현
class Trie(object):
    def __init__(self):
        self.head = Node(None)
    
    # Trie에 string 삽입
    def insert(self, string):
        curr_node = self.head
        
        for char in string:
            if char not in curr_node.children:
                curr_node.children[char] = Node(char)
            curr_node = curr_node.children[char]
        
        # string의 last char라면 node의 data 필드에 string을 저장함
        curr_node.data = string

    # Trie에 string이 존재하는지 여부 파악
    def search(self, string):
        curr_node = self.head

        for char in string:
            if char in curr_node.children:
                curr_node = curr_node.children[char]
            else:
                return False
        
        # string의 last char에 다다랐을 때 node의 data 필드에 값이 있다면, Trie에 string이 존재함
        if curr_node.data != None:
            return True
    
    # Trie에서 prefix로 시작하는 string 탐색
    def starts_with(self, prefix):
        curr_node = self.head
        subtrie = None

        for char in prefix:
            if char in curr_node.children:
                curr_node = curr_node.children[char]
                subtrie = curr_node
            else:
                return None
        
        result = []
        stack = list(subtrie.children.values())

        # DFS로 prefix subtrie 순회
        while stack:
            curr = stack.pop()
            if curr.data != None:
                result.append(curr.data)
            
            stack += list(curr.children.values())
        
        return result
```

## References

- [파이썬에서 Trie (트라이) 구현하기](https://blog.ilkyu.kr/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%97%90%EC%84%9C-Trie-%ED%8A%B8%EB%9D%BC%EC%9D%B4-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [트라이(Trie) 자료구조](https://www.crocus.co.kr/1053)