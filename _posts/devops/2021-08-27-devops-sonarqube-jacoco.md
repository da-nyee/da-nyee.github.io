---
title: '[DevOps] SonarQube + JaCoCo'
author: da-nyee
date: 2021-08-27 23:51:53 +0900
categories: [TIL, DevOps]
tags: [devops, sonarqube, jacoco]
---

## SonarQube?

- 정적 코드 분석 도구
- 정적 코드 분석으로 자동 리뷰를 수행하기 위한 지속적인 코드 품질 검사용 오픈소스 플랫폼
- 20개 이상의 프로그래밍 언어에서 버그, 코드 스멜, 보안 취약점을 발견하기 위한 목적으로 활용한다.

<br/>

## JaCoCo?

- Java Code Coverage
- Java 코드의 커버리지를 체크하는 라이브러리
- SonarQube와 연계하여 사용하는 경우가 많다.

<br/>

## 적용기

### 처음에는

- 여러 자료를 찾아보다 SonarQube 프로퍼티에 `sonar.java.coveragePlugin`이란 항목이 있는 걸 확인했다.
- 해당 프로퍼티에 `jacoco`만 작성해주면, 프로젝트 build.gradle에 JaCoCo를 따로 설정하지 않아도 SonarQube가 코드 커버리지도 산출한다고 생각했다.
- 쉽고 빠르게 정적 분석 레포트를 만들 수 있는 방법이 없을까 고민을 하다, 젠킨스 프리스타일을 이용해서 특정 브랜치(e.g. develop)을 기준으로 생성하는 방법을 발견했다.
- 그러나, 팀 회의를 통해 논의한 결과 develop에 코드가 이미 merge된 이후에 정적 분석을 하는 것은 큰 의미가 없다고 판단했다. 코드 품질을 먼저 검사하고, 버그 또는 취약점 등을 해결한 뒤 merge해야 한다는 의견에 동의했다.
- 최종적으로 Github 저장소에 PR이 보내지면, 1. 해당 PR에 대한 코드를 정적 분석해서 레포트를 생성하고 2. 해당 PR의 코멘트로 해당 결과를 남기는 방식으로 구성하기로 합의했다. 이때, 젠킨스 파이프라인을 이용하기로 결정했다.

<br/>

### 장장 8시간 30분간의 삽질

- 프리스타일에서는 젠킨스에 있는 JaCoCo 플러그인만 사용해도 JaCoCo를 사용할 수 있어 이 방법을 이용하려 했다.<br/>
위에서 언급했듯 build.gradle은 건드리지 않아도 된다고 생각했다.
    - 그런데, 알고보니 파이프라인에서는 해당 방식을 적용할 수 없었다.
- Github PR 코멘트도 SonarQube 프로퍼티로 제공되는 게 있어 이를 써보려고 했다.
    - 하지만, 해당 프로퍼티는 Github Enterprise를 활용하는 경우에 쓸 수 있는 프로퍼티였다.
- 앞선 잘못된 지식들을 가지고 초기 파이프라인을 만들었다.
    - Github 저장소에서 PR을 보낸 브랜치로 체크아웃
    - 빌드 및 테스트
    - SonarQube 분석
    - SonarQube Quality Gateway 실행
- 젠킨스의 빌드 로그를 살펴보면 정적 분석 레포트를 확인할 수 있었지만,<br/>
이때 1. 코드 커버리지가 나타나지 않는 문제와 2. PR에 코멘트가 남겨지지 않는 문제가 발생했다.
- 다른 자료들을 다시 확인하며 잘못된 부분들을 파악했고 수정했다.
    - 우선, build.gradle에 JaCoCo와 관련된 groovy 코드들을 추가했다.<br/>
    그리고 파이프라인에 JaCoCo 단계를 추가했다.
    - 다음으로, Github Open API를 활용해서 Github PR 코멘트를 남기는 기능을 직접 구현했다.<br/>
    API URL에 PR id를 지정하고, Request Body에 정적 분석 레포트 주소 등을 담아 요청하게 했다.
- 초기 파이프라인을 다음과 같이 변경했다.
    - Github 저장소에서 PR을 보낸 브랜치로 체크아웃
    - 빌드 및 테스트
    - JaCoCo 분석
    - SonarQube 분석
    - SonarQube Quality Gateway 실행
- 결과적으로 원하는대로 코드 커버리지까지 잘 잡히는 정적 분석 레포트가 생성됐고, Github PR 코멘트도 잘 남겨졌다.
- 여기서 한 가지 문제가 더 있었다.
    - 젠킨스에서 Github에 접근하기 위해 팀원 1명의 Github Access Token을 사용하고 있었는데,<br/>
    해당 팀원이 아닌 팀원이 PR을 날리면 PR에 `Can one of the admins verify this patch?`라는 내용의 코멘트가 달렸다.
- 해결 방법을 고민하며 또 다시 구글링을 했다.
    - 파이프라인 admin list에 백엔드 팀원들의 Github 계정명을 추가해서 해결했다.
- 추가적으로 특정 라벨, 즉 backend가 붙은 경우에만 파이프라인이 동작하도록 설정했다.

<br/>

## References

- [SonarQube 적용하기](https://velog.io/@lxxjn0/코드-분석-도구-적용기-3편-SonarQube-적용하기)
- [Gradle 프로젝트에 JaCoCo 설정하기](https://techblog.woowahan.com/2661/)