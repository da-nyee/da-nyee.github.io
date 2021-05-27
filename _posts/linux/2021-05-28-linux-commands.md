---
title: '[Linux] netstat 명령어 (netstat Command)'
author: da-nyee
date: 2021-05-28 03:08:06 +0900
categories: [TIL, Linux]
tags: [linux, netstat, command]
---

## 1.

어떤 프로세스가 어떤 포트를 사용 중인지 확인하는 명령어

```
netstat -ntlp
```

<img width="678" alt="netstat_ntlp" src="https://user-images.githubusercontent.com/50176238/119875452-bab3a380-bf61-11eb-8727-976f72bade98.png">

## 2.

어떤 프로세스가 특정 포트를 사용 중인지 확인하는 명령어

```
netstat -ntlp|grep {port}
```

<img width="638" alt="netstat_ntlp_grep_8080" src="https://user-images.githubusercontent.com/50176238/119875163-6d373680-bf61-11eb-8d18-7ad5ad2edd37.png">

## Reference

- [[리눅스] 프로세스가 사용중인 PORT 확인. ( netstat -ntlp )](https://ycswarm.tistory.com/11)