---
title: '[Git] 깃 커밋 메시지 컨벤션 (Git Commit Message Convention)'
author: da-nyee
date: 2020-08-31 02:55:21 +0900
categories: [TIL, Git]
tags: [git, github, convention]
---

## Introduction

Git의 개념을 이해하고 사용한지 1년 정도 됐다.

작년 여름에 집중적으로 공부했는데, 그땐 공책에 필기로 정리하고 블로그에 따로 글을 쓸 생각은 안했다.<br/>
기술 블로그를 시작하면서 복습할 겸 Git의 개념, 명령어 등을 시간날 때마다 포스팅할 생각이다.

나는 Git, Github에 익숙해진 이후, 1일 1커밋을 지키고자 노력하고 있다.<br/>
1일 1커밋 초반에는 어떤식으로 커밋 메시지를 작성하는 게 좋을지 고민이 많았다.<br/>
그러던 도중 우연히 Git 커밋 메시지 가이드 라인을 보고 해당 관습을 따르게 되었다.<br/>

내가 참고한 이 관습에는 이름이 있다. 바로 `Udacity Style`이다.<br/>
이번 포스팅에서는 이 `Udacity Style`이 무엇인지 다뤄보고자 한다.

## Udacity Style?

`Udacity Style`의 커밋 메시지 구조는 다음과 같이 생겼다.
```
type: Subject

Body

Footer
```

Subject, Body, Footer에 들어가는 내용을 하나하나 살펴보자.

### Title

커밋 메시지 Title은 `docs: Update README.md`처럼 작성한다.<br/>
`docs`는 type, `Update README.md`는 Subject이다.

### Subject

Subject는 크게 4가지의 특징을 가진다.
- 길이는 50자 이하로 작성한다.
- 동사원형(ex. Add, Update, Modify)로 시작한다.
- 첫 글자는 대문자이다.
- 끝에는 마침표를 붙이지 않는다.

### Type

Type은 크게 7가지가 있다.
- feat: 새로운 기능 추가
- fix: 버그 수정
- docs: 문서 수정
- style: 코드 포맷 변경, 세미콜론 누락, 코드 변경 없음
- refactor: 프로덕션 코드 리팩터링
- test: 테스트 추가, 테스트 코드 리팩터링, 프로덕션 코드 변경 없음
- chore: 빌드 테스크 업데이트, 패키지 매니저 환경설정, 프로덕션 코드 변경 없음

### Body

커밋 메시지는 대부분 Title만 읽어도 내용을 파악할 수 있다.<br/>
따라서, Body는 선택사항으로 커밋 메시지에 부가적인 설명이 필요할 때 작성한다.

Body는 크게 3가지의 특징을 가진다.
- 길이는 72자 이하로 작성한다.
- Title과 Body 사이에는 한 줄의 공백을 추가한다.
- 커밋 메시지의 `What`과 `Why`에 대해 작성한다. `How`는 고려하지 않는다.

### Footer

Footer 역시 선택사항이다.<br/>
해결한 이슈 커밋의 ID, 해결하기 위해 참고한 커밋의 ID 등을 작성한다.

### Example
```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```

## Reference

- [Udacity Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)