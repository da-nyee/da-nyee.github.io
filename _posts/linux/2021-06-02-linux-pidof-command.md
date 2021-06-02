---
title: '[Linux] pidof 명령어 (pidof Command)'
author: da-nyee
date: 2021-06-02 17:17:02 +0900
categories: [TIL, Linux]
tags: [linux, pidof, command]
---

## 1.

특정 프로세스의 pid를 찾는 명령어

```
pidof {process_name}
```

<img width="388" alt="pidof" src="https://user-images.githubusercontent.com/50176238/120442202-527d1b80-c3c0-11eb-8253-943ca9ad37e8.png">

## 2.

특정 프로세스의 pid를 하나만 찾는 명령어

```
pidof -s {process_name}
```

<img width="408" alt="pidof_with_s" src="https://user-images.githubusercontent.com/50176238/120443373-873da280-c3c1-11eb-8734-55ec25e19d17.png">

## 3.

특정 스크립트의 pid를 찾는 명령어

```
pidof -x {script_name}
```

> `./{script_name}`으로 실행한 경우에만 `-x` 옵션으로 pid를 찾을 수 있다.<br/>
> `bash {script_name}`으로 실행한 경우에는 `-x` 옵션으로 pid를 찾을 수 없다.<br/>

## Reference

- [이름으로 process id 를 가져오는 pidof 명령어 사용법](https://www.lesstif.com/lpt/process-id-pidof-100204763.html)