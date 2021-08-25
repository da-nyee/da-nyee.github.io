---
title: '[Git] Git Branch 전략 (Git Branch Strategy)'
author: da-nyee
date: 2021-08-25 19:18:19 +0900
categories: [TIL, Git]
tags: [git, branch, strategy]
---

## Base Git Flow

![base_git_flow](https://user-images.githubusercontent.com/50176238/130752917-dfad0b75-6f25-40e7-9eb9-5b0b5272dbb1.png)

- main, hotfix, release, develop, feature - 총 5개의 브랜치를 사용한다.
- 처음에 main을 기준으로 develop을 생성한다.
- 각 기능마다 develop을 기준으로 feature를 생성해서 개발을 진행한다. 개발을 완료하면 develop에 merge한다.
- 배포를 준비할 때는 release를 활용하여 develop -> release로 merge한다.
- 만약 release에 bug가 있을 경우 hotfix를 따로 이용하지 않고, release에서 바로 해결한다.
- 배포 준비를 완료하면 release -> main으로 merge하고, release -> develop으로 develop에도 동일하게 반영한다.
- 만약 main에 bug가 있을 경우 hotfix를 사용하여 해결한다. 그리고 hotfix -> main, hotfix -> develop으로 해당 commit을 반영한다.
- 배포를 버전별로 관리하기 위해 main에 tag를 붙인다.

<br/>

## Pick-Git Git Flow

![pick_git_flow1](https://user-images.githubusercontent.com/50176238/130754093-c4c24908-97e2-4116-80ff-36a93df29a73.png)
![pick_git_flow2](https://user-images.githubusercontent.com/50176238/130754185-083daefb-4f52-44f3-9f60-438838637bd9.png)
![pick_git_flow3](https://user-images.githubusercontent.com/50176238/130754223-0d414657-b93b-4c97-9fbf-aa40eb51b7c1.png)

- main, hotfix, develop, feature - 총 4개의 브랜치를 사용한다.
    - 브랜치 전략을 최대한 간단하게 갖고가자는 생각으로 release는 활용하지 않기로 결정했다.
- 처음에 main을 기준으로 develop을 생성한다.
- 각 기능마다 develop을 기준으로 feature를 생성해서 개발을 진행한다.
    - 개발하던 중 develop에 새로운 commit이 추가되면, feature의 base 해시값이 변경된 것이므로 develop의 가장 최신 commit으로 rebase한다.
    - 개발을 완료하면 feature -> develop으로 PR을 생성한다.
    - 이때, 젠킨스를 통해 해당 PR에 대한 Build + Test 및 SonarQube 검증을 진행한다.
    - SonarQube 검증을 통과하고, 팀원들 중 최소 1명 이상(FE 팀원 수를 고려하여 결정했다)이 PR Approve를 하면 merge를 위한 최소 요건이 충족된다.
    - 최소 요건을 충족하면 develop에 squash-merge한다. 이는 여러 개의 commit을 하나의 commit으로 압축하여 커밋 그래프를 단순하게 가져가기 위한 전략이다.
- 배포를 할 때는 develop -> main으로 PR을 생성한다.
    - 이때는 SonarQube 검증은 따로 하지 않지만, 팀원 모두가 PR Approve를 해야 merge할 수 있다.
    - 요건을 충족하면 main에 squash-merge한다. 그리고 develop에 해당 commit을 반영하기 위해 pull(fetch + merge)한다.
- 만약 main에 버그가 있을 경우 hotfix를 사용하여 해결한다. 그리고 hotfix -> main, hotfix -> develop으로 해당 commit을 반영한다.
- 배포를 버전별로 관리하기 위해 main에 tag를 붙인다.

<br/>

## Reference

- [우린 Git-flow를 사용하고 있어요](https://techblog.woowahan.com/2553/)