---
title: '[Web] 정규표현식 (Regular Expressions)'
author: da-nyee
date: 2022-08-19 01:45:01 +0900
categories: [TIL, Web]
tags: [web, regular expressions]
---

## 정규표현식

문자열에서 특정 패턴을 만족하는 부분을 찾아낼 때 사용한다.<br/>

<br/>

## \d

- 숫자 대표문자
- digit
- 1글자만 찾는다.

<br/>

## \w

- 글자 대표문자
- word
- `a, b, c, 가, 나, 다, 1, 2, 3`과 같은 문자와 숫자를 포함한다.<br/>
특수문자는 포함하지 않지만, `_`는 포함한다.
- 1글자만 찾는다.

<br/>

## +

- 1개 이상

<br/>

## *

- 0개 이상
- e.g. `[1-9]\d*`는 자연수

<br/>

## ?

- 있거나 없거나
- e.g. `\d+-?\d+-?\d+`는 전화번호(숫자-숫자-숫자 or 숫자숫자숫자)

<br/>

## []

- 속한 것 중에 하나
- e.g. `[- ]?`는 - or (공백)이 있거나 없거나<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
`\d+[- ]?\d+[- ]?\d+`는 전화번호(숫자-숫자-숫자 or 숫자숫자숫자 or 숫자 숫자 숫자)

<br/>

## {숫자}

- `숫자`번 반복
- e.g. `\d{2}`는 숫자가 2번 반복

<br/>

## {숫자1, 숫자2}

- `숫자1`부터 `숫자2`까지 반복
- e.g. `\w{2,3}`은 문자가 2-3번 반복

<br/>

## [어떤 것-어떤 것]

- 범위 지정
- e.g. `[a-z]`는 소문자 알파벳만 선택<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
`[가-힣]`은 한글만 선택

<br/>

## [어떤 것-어떤 것]+

- 범위 지정인데 1개 이상
- e.g. `[a-z]+`는 소문자 알파벳이 1개 이상<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
`[가-힣]+`는 한글이 1개 이상(한글 단어)

<br/>

## 다양한 대표문자

### \s

- 공백 문자
- e.g. 스페이스, 탭, 뉴라인

### \S

- 공백 문자를 제외한 문자

### \D

- 숫자를 제외한 문자

### \W

- 글자 대표문자를 제외한 문자
- e.g. 특수문자, 공백 등

<br/>

`\대문자 알파벳`이면 `반대(여집합)`의 의미인 것 같다.<br/>

<br/>

## 프로그래밍 언어와 정규표현식 사용

### Java, C#

- 대표문자를 표현할 때 `\`를 2번 써야 한다.
- `escape` 때문이다.
- e.g. `\\d`, `\\w` 등등

### JavaScript, Python

- 대표문자를 표현할 때 `\`를 1번만 써도 된다.
- `raw string`을 지원하기 때문이다.
- e.g. `\d`, `\w` 등등

<br/>

## Reference

- [프로그래머스 스쿨 - 정규표현식](https://school.programmers.co.kr/learn/courses/11)