---
title: '[Java] flatMap으로 중첩 루프 없애는 방법 (How to remove nested loops using flatMap)'
author: da-nyee
date: 2021-03-11 23:53:28 +0900
categories: [TIL, Java]
tags: [java, flatmap, nested loops]
---

## Code with Nested Loops

```
List<String> symbols = Arrays.asList("스페이드", "하트", "다이아몬드", "클로버");
List<String> numbers = Arrays.asList("A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K");

List<String> cards = new ArrayList<>();
for (String symbol : symbols) {
    for (String number : numbers) {
        cards.add(symbol + number);
    }
}
```

## Code with flatMap

```
List<String> symbols = Arrays.asList("스페이드", "하트", "다이아몬드", "클로버");
List<String> numbers = Arrays.asList("A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K");

List<String> cards = symbols.stream()
        .flatMap(symbol -> numbers.stream()
                .map(number -> symbol + number))
        .collect(toList());
```

## Reference

- 모던 자바 인 액션 스터디 - 검프 문제 on Mar 11