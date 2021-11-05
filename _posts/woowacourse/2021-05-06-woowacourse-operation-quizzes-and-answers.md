---
title: '[우아한테크코스] 운영 퀴즈 및 정답 (Operation Quizzes and Answers)'
author: da-nyee
date: 2021-05-06 01:51:21 +0900
categories: [EDUCATION, Woowacourse]
tags: [woowacourse, operation, quizzes, answers]
---

## 1.

### Quiz
CPU 사용률 100%는 장애를 의미하지 않아요. 그럼에도 파악하는 이유는 무엇일까요?

### Answer

- 컴퓨터가 CPU 가용용량보다 더 많은 일을 하려고 시도하는 상태이다.
    - 보통 괜찮지만, 현재 실행되는 프로그램들의 속도가 약간 느려질 수 있다.
    - 만약, 오랫동안 100%를 유지하면 프로그램들의 속도가 더 느려질 수 있다.
- 컴퓨터의 속도를 빠르게 만들기 위해 어떤 프로그램이 CPU를 오래 점유하고 있는지 확인해야 한다.
- 또, CPU가 포화 상태로 넘어가기 직전 == 장애가 발생할 수 있기 직전의 상태이다.
    - 비정상적으로 오래 CPU 자원을 점유하는 프로세스를 미리 파악해야 한다.
    - 따라서, 이러한 프로세스에 대해 어떠한 조취를 취하기 위해 파악할 필요가 있다.

## 2.

### Quiz

메모리가 고갈되면 어떤 상황이 발생할까요?

### Answer

- 컴퓨터는 SWAP 메모리 == 디스크의 일부분을 사용하기 시작한다.
    - RAM에 있던 일부 프로세스를 언로드하고, SWAP에 해당 프로세를 새로 할당한다.
- 만약, 모든 프로세스가 작동 중이었다면 프로그램들의 속도가 약간 느려진다.
    - RAM보다 SWAP의 처리 속도가 느리기 때문이다.
- SWAP 역시도 다 차게 되면 더 심각한 문제가 발생한다.
    - 예를 들어, 1) 프로그램 업데이트가 거부될 수 있고 2) 프로그램이 의도치 않게 닫힐 수 있고 3) 프로그램 응답시간이 느려질 수 있고 4) 시스템이 불안정해질 수 있다.
- 따라서, 메모리 고갈은 메모리 누수가 일어나지 않는 이상 발생하면 안 된다.

## 3.

### Quiz

디스크가 꽉 차면 어떤 현상이 발생할까요? 그리고 어떤 파일들부터 지우면 좋을까요?

### Answer

> 디스크가 꽉 차면 어떤 현상이 발생할까?

- 에러 메시지(보통 "디스크 공간 적음") 에러 메시지가 나타난다.
- 컴퓨터가 느리게 작동한다.
    - 디스크에서 원하는 데이터를 찾는 데에 더 오랜 시간이 걸린다.
- 시스템 충돌이 발생한다.
    - 컴퓨터는 디스크 내의 가상 메모리 공간이 필요하다.
    - SWAP은 RAM이 비워지는 동안 데이터를 저장하기 위해 필요하다.
    - 만약, 이 공간이 충분하지 않다면 해당 공간은 오버플로우 된다.
    - 그리고 메모리 집중 연산이 컴퓨터를 동결 또는 충돌되게 만든다.
- 소프트웨어 업데이트를 할 수 없다.
    - 소프트웨어 업데이트 or 설치 or 재설치를 할 때, 이런 일을 할 만한 충분한 공간이 없으면 불가능하다.
- 데이터 붕괴가 발생한다.
    - 예를 들어, 디스크에 충분한 공간이 없어 시스템 충돌이 발생한다고 가정한다.
    - 이때 몇 개의 파일들이 열려있는 상태라면 해당 파일 데이터는 누락될 수도 있다.

> 어떤 파일들부터 지우면 좋을까?

- 윈도우 기준
- Hibernation 파일 ≒ 잠자기 모드 를 삭제한다. 👉 Command Prompt에서 삭제할 수 있다.
- Temp 폴더를 정리한다. 👉 윈도우가 처음에 한번만 사용 == 더 이상 사용하지 않는 파일들이 있다.
- 휴지통을 비운다. 👉 휴지통도 메모리를 차지한다. 따라서 영구 삭제를 한다.
- Windows.old 폴더를 삭제한다. 👉 윈도우 업데이트 이전 버전을 복사해두므로 불필요한 공간을 차지한다.
- Downloaded Program Files를 삭제한다. 👉 요즘은 불필요한 ActiveX 등을 가지고 있다.
- LiveKernelReport를 삭제한다. 👉 덤프 파일들 == 윈도우가 이미 가지고 있는 정보 로그들의 홈이다.

## 4.

### Quiz

Reverse Proxy를 별도로 구성함으로써 얻는 이점을 설명해주세요.

### Answer

- 예를 들어, 클라이언트가 웹 서비스에 데이터를 요청하면 Reverse Proxy는 해당 요청을 받아 내부 서버에서 데이터를 얻은 후에 이 데이터를 클라언트에 전달한다.
    - 내부 서버가 직접 서비스를 제공해도 된다.
    - 그러나, 이렇게 구성하는 이유 중 하나는 보안 때문이다.
- 보통 기업의 네트워크 환경은 DMZ라 하는 내부 네트워크 - 외부 네트워크 사이에 위치하는 구간이 존재한다.
    - 이 구간에는 메일 서버, 웹 서버, FTP 서버 등 외부 서비스를 제공하는 서버가 위치한다.
- Java로 서비스를 구현해서 WAS를 DMZ에 두고 서비스를 해도 된다.
    - 그렇지만, WAS는 보통 DB 서버와 연결된다. 그래서 WAS가 최전방에 있으면 WAS가 해킹당할 경우 DB 서버까지 해킹당하는 심각한 문제가 발생한다.
- 따라서, 바깥에 Reverse Proxy 서버를 두고 내부망에 실제 서비스 서버를 위치시킨다.
    - 바깥에 있는 Reverse Proxy 서버만 내부에 있는 실제 서비스 서버와 통신해서 결과를 클라이언트에게 제공하는 방식으로 서비스한다.
- 특히 Linux 환경에서는 Reverse Proxy로 Apache 웹 서버를 사용했을 때 SELinux를 켜두면, SELinux의 기본 정책에 따라 웹 서버는 톰캣의 8080과 8089 포트로만 접근이 가능하다.
    - 그래서 Apache 웹 서버가 해킹당해도 웹 서버 권한으로는 내부망 연결을 할 수 없다.
    - 톰캣의 8080 포트로 접근해도 사용자 권한보다 적은 권한을 가지고 있어 어차피 별 거 못한다.
- 또, Reverse Proxy를 클러스터로 구성하면 1) 가용성을 높일 수 있고 2) 사용자가 증가하는 상황에 맞춰서 웹 서버나 WAS를 유연하게 늘릴 수 있는 장점이 있다.
    - 웹 서버가 처리할 로직 < WAS가 처리할 로직
    - 따라서, 웹 서버를 하나로 두고 WAS를 여러개로 둬서 `로드 밸런싱` 을 하면 된다.

## References

- [Is it normal for my computer to be using this much CPU or memory?](https://help.gnome.org/users/gnome-system-monitor/stable/cpu-mem-normal.html.en)
- [What happens to a PC on full RAM?](https://www.quora.com/What-happens-to-a-PC-on-full-RAM)
- [What Will Happen If Your Hard Drive Is Full?](http://datanumen.com/blogs/will-happen-hard-drive-full/)
- [Delete These Windows Files and Folders to Free Up Disk Space](https://www.makeuseof.com/tag/delete-windows-files-folders/)
- [포워드 프록시(forward proxy) 리버스 프록시(reverse proxy) 의 차이](https://www.lesstif.com/system-admin/forward-proxy-reverse-proxy-21430345.html)