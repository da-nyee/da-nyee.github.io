---
title: '[IntelliJ] 콘솔 한글 깨짐 (Console Encoding Issue)'
author: da-nyee
date: 2020-12-02 04:20:22 +0900
categories: [TIL, IntelliJ]
tags: [intellij, console, encoding, issue]
---

## Introduction

`IntelliJ`에서 프로그램을 실행시켜 `콘솔을 확인했을 때 한글이 깨져 출력되는 경우`가 종종 있다.<br/>

예를 들면,<br/>

![console_encoding_errors](https://user-images.githubusercontent.com/50176238/100780113-ad06e080-344c-11eb-9a69-746731f9f572.png)

위의 사진처럼 알 수 없는 문자들이 나타난다.<br/>

최근에도 `콘솔 한글 깨짐 현상`을 겪어 구글링을 통해 문제를 해결했다.<br/>
이런 문제는 어떻게 해결해야 하는지 기록해두는 게 좋을 것 같아 포스팅을 한다.

## Method 1

첫번째는 `Settings`에서 `File Encodings`를 `UTF-8`로 변경하는 방법이다.<br/>
(참고로, `Settings`는 상단바 `File-Settings` 혹은 단축키 `Ctrl+Alt+S`로 열 수 있다.)<br/>

![intellij_settings](https://user-images.githubusercontent.com/50176238/100782389-ad54ab00-344f-11eb-9775-00c3f193b906.PNG)

이 화면에서 `Global Encodings`, `Project Encoding`, `Properties Files`를 `UTF-8`로 설정한다.

## Method 2

두번째는 `VM Options`를 편집하는 방법이다.<br/>
(참고로, `VM Options`는 상단바 `Help-Edit Custom VM Options`로 열 수 있다.)<br/>

![intellij_vmoptions](https://user-images.githubusercontent.com/50176238/100783582-50f28b00-3451-11eb-809c-807c872642b3.PNG)

이 파일에서 맨 아래에 `-Dfile.encoding=UTF-8`, `-Dconsole.encoding=UTF-8`을 추가한다.<br/>
위의 옵션들을 추가했다면, `IntelliJ`를 재시작한다.

## Method 3

세번째는 `Gradle`를 `rebuild`하는 방법이다.<br/>
(참고로, `Gradle`은 `IntelliJ` 화면 맨 오른편에 위치해 있다.)<br/>

![intellij_gradle](https://user-images.githubusercontent.com/50176238/100784234-3a98ff00-3452-11eb-89df-5d614766e4f1.PNG)

이 부분에서 `build-clean`을 클릭하고, `build clean`이 끝나면 `build-build`를 선택한다.<br/>

여기까지 적용하면 `콘솔 한글 인코딩 문제가 해결된다!` 🙌

## Reference

- [Intellij 한글 깨짐 & unmappable character for encoding x-windows-949](https://beemiel.tistory.com/4)