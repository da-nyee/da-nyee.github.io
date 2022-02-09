---
title: '[Jenkins] 젠킨스에서 스케줄링하는 방법 (How to schedule in Jenkins) (feat. crontab)'
author: da-nyee
date: 2022-02-05 23:51:41 +0900
categories: [TIL, Jenkins]
tags: [jenkins, crontab]
---

최근에 Spring Batch를 사용하고 있는데, 이때 스케줄링을 접목할 필요가 생겼다.<br/>
[Spring Batch를 공부하며 참고했던 블로그](https://jojoldu.tistory.com/324)를 보면, 보통 Quartz를 이용해서 스케줄링을 한다고 설명한다.<br/>
하지만 이번에는 Jenkins를 써야 해서 이를 활용했다.<br/>

Jenkins의 아이템 - 구성 - 빌드 유발 - Build periodically를 통해 스케줄을 편하게 지정할 수 있다.<br/>
여기서 <b>crontab</b>을 사용해야 하는데, 문법을 잘 몰라서 검색해서 적용했다. (크론표현식과 매우 유사하다.)<br/>
앞으로 또 쓸 일이 있을 수도 있으니까! 잊지 않으려고 정리한다.<br/>

<br/>

## crontab

<b>minute / hour / day of month / month / day of week</b> 순서로 작성한다.<br/>

day of week에는 1~7을 적을 수 있다. (1은 월요일, 2는 화요일.. 7은 일요일이다.)<br/>

<br/>

## Examples

### 1.

20 02 * * 2<br/>

매주 화요일 새벽 2시 20분에 실행한다.<br/>

### 2.

20 02 2 * *<br/>

매월 2일 새벽 2시 20분에 실행한다.<br/>

### 3.

*/20 14-16 18 4 *<br/>

매년 4월 18일 오후 2시에서 4시까지 20분마다 실행한다.<br/>

<br/>

## References

- [jenkins periodically 시간, crontab 시간 설정 문법](https://marobiana.tistory.com/34)
- [[Jenkins] 빌드유발 Build periodically (Crontab)](https://onu0624.tistory.com/36)