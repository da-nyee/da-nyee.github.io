---
title: '[Operating System] 컴파일, 링크, 빌드 (Compile, Link, Build)'
author: da-nyee
date: 2020-10-01 05:40:52 +0900
categories: [TIL, Operating System]
tags: [operating system, compile, link, build]
---

## Introduction

컴퓨터과학(CS), 운영체제(OS) 기초를 다잡을겸 컴파일(Compile), 링크(Link), 빌드(Build)에 대해 알아보자.

## Compile

> 개발자가 작성한 source code를 `binary code(컴퓨터가 이해하는 기계어)로 변환`하는 과정이다.<br/>

해당 작업을 해주는 프로그램이 `Compiler`이다.<br/>

Java의 경우, JVM(Java Virtual Machine)에서 byte 코드 형태인 class 파일이 생성된다.

## Link

> 여러개로 분리된 Compile한 소스파일들 ➡ 최종 실행 가능한 파일로 만들기 위해 `필요한 부분을 찾아서 연결해주는 작업`이다.<br/>

프로젝트를 진행하다 보면,
- 소스파일이 여러개 생성되고
- A 소스파일에서 B 소스파일에 존재하는 함수(메소드)를 호출하는 경우가 있다.<br/>

이때, A와 B를 각각 Compile만 하면 A가 B에 존재하는 함수를 찾을 수 없어 호출할 수가 없다.<br/>
따라서 A와 B를 연결해주는 작업이 필요하다.<br/>

해당 작업을 `Link`라고 한다.<br/>

Link는 `정적 링크(Static Link)`와 `동적 링크(Dynamic Link)`가 있다.
- Static Link: Compile된 소스파일들을 연결해서 최종 실행 가능한 파일을 만드는 것이다.
- Dynamic Link: 프로그램 실행 도중, 프로그램 외부에 존재하는 코드를 찾아서 연결하는 것이다.<br/>

Java의 경우, JVM이 프로그램 실행 도중에 필요한 class를 찾아서 class path에 로드해준다.(Dynamic Link)

## Build

> 개발자가 작성한 source code를 `실행 가능한 소프트웨어 결과물로 만드는 일련의 과정`이다.<br/>

Build 단계 중 Compile이 포함되어 있다. 즉, `Build ⊃ Compile`이라 볼 수 있다.<br/>

`Build Tool`은 Build 과정을 도와주는 도구로, 일반적으로 다음과 같은 기능들을 제공한다.
- 전처리(Preprocessing)
- 컴파일(Compile)
- 패키징(Packaging)
- 테스팅(Testing)
- 배포(Distribution)<br/>

Java의 경우, Build Tool로는 `Ant, Maven, Gradle` 등이 있다.

## Conclusion

> Build == Compile + Link 등<br/>
> Run == Build + 실행 == (Compile + Link 등) + 실행<br/>

`Compile ➡ Link ➡ Build ➡ Run`의 순서로 이어진다.

## References

- [컴파일/빌드 란?](https://m.blog.naver.com/PostView.nhn?blogId=ssdyka&logNo=221494190411&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [컴파일과 빌드 차이점](https://freezboi.tistory.com/39)