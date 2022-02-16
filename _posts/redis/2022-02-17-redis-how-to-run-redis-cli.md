---
title: '[Redis] Redis CLI를 실행하는 방법 (How to run Redis CLI)'
author: da-nyee
date: 2022-02-17 01:46:20 +0900
categories: [TIL, Redis]
tags: [redis, redis-cli]
---

## 기본 설정

### 다운로드

공식 사이트에서 압축 파일을 받고, 압축을 푼다.<br/>

```bash
% wget https://download.redis.io/releases/redis-6.2.6.tar.gz
% tar xzf redis-6.2.6.tar.gz
```

### 설치

아래 명령어를 순서대로 입력한다.<br/>

```bash
% cd redis-6.2.6
% make

...
Hint: It's a good idea to run 'make test' ;)
```

### 테스트

맨 마지막에 `\o/`로 시작하는 귀여운 문장이 나타나면 성공이다 !

```bash
% make test

...
\o/ All tests passed without errors!
```

<br/>

## 서버 - 클라이언트 연결

터미널을 총 2개(서버용 1개, 클라이언트용 1개) 준비한다.<br/>

### 서버 접속

한 터미널에 아래 명령어를 입력하고, 중간에 종료하지 않고 그대로 둔다.<br/>

```bash
% src/redis-server
58314:C 17 Feb 2022 01:25:56.621 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
58314:C 17 Feb 2022 01:25:56.621 # Redis version=6.2.6, bits=64, commit=00000000, modified=0, pid=58314, just started
58314:C 17 Feb 2022 01:25:56.621 # Warning: no config file specified, using the default config. In order to specify a config file use src/redis-server /path/to/redis.conf
58314:M 17 Feb 2022 01:25:56.623 * Increased maximum number of open files to 10032 (it was originally set to 2560).
58314:M 17 Feb 2022 01:25:56.623 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 58314
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

58314:M 17 Feb 2022 01:25:56.625 # Server initialized
58314:M 17 Feb 2022 01:25:56.626 * Ready to accept connections
```

### 클라이언트 확인

다른 터미널에 아래 명령어를 입력해서 요청을 보냈을 때, `PONG`이라는 귀여운 응답이 돌아오면 성공이다 !

```bash
% src/redis-cli ping
PONG
```

<br/>

## Reference

- [Redis - Download](https://redis.io/download)