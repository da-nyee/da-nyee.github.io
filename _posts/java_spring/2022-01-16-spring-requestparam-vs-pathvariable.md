---
title: '[Spring] @RequestParam vs @PathVariable'
author: da-nyee
date: 2022-01-16 01:21:03 +0900
categories: [TIL, Spring]
tags: [spring, requestparam, pathvariable]
---

- @RequestParam과 @PathVariable은 <b>Request URI</b>에서 값을 얻는다는 공통점이 있다.
- 한편, 값을 얻는 방식에는 차이점이 있다. 어떻게 다를까?

<br/>

## @RequestParam

- Query String으로부터 값을 얻는다. 👉 key=value 형식이다.
- e.g. https://pick-git.com/posts?id=1

```java
@GetMapping("/api/posts")
public ResponseEntity<ReadResponse> read(@RequestParam Long id) {
    ReadResponse response = PostAssembler
        .readResponse(postService.read(PostAssembler.readRequestDto(id)));
    
    return ResponseEntity.ok(response);
}
```

<br/>

## @PathVariable

- URI Path로부터 값을 얻는다.
- e.g. https://pick-git.com/posts/1

```java
@GetMapping("/api/posts/{id}")
public ResponseEntity<ReadResponse> read(@PathVariable Long id) {
    ReadResponse response = PostAssembler
        .readResponse(postService.read(PostAssembler.readRequestDto(id)));

    return ResponseEntity.ok(response);
}
```

<br/>

## Conclusion

- API URI를 설계할 때, 적절한 방법을 선택해서 명세하자.
- 필요에 따라서는 두 방법을 혼합해서 사용할 수도 있다.

<br/>

## Reference

- [Spring @RequestParam vs @PathVariable Annotations](https://www.baeldung.com/spring-requestparam-vs-pathvariable)