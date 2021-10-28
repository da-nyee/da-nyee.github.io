---
title: '[Git] Tagging으로 버전 관리하기 (Tagging for Version Control)'
author: da-nyee
date: 2021-10-28 18:46:31 +0900
categories: [TIL, Git]
tags: [git, tagging, version control]
---

## 목록 조회

```
% git tag
v1.0.0
v1.1.0
v1.2.0
v2.0.0
v2.1.0
```

<br/>

## 개별 조회

```
% git show {tag_name}
```

```
// 예시
% git show v2.0.0
tag v2.0.0
Tagger: Daeun Lee <leede0418@likelion.org>
Date:   Wed Oct 27 17:39:28 2021 +0900

pick-git v2.0.0 배포

commit ccf5598c74e735561394cd62725254f068895cff (tag: v2.0.0)
Author: LEE DAEUN <leede0418@gmail.com>
Date:   Wed Oct 27 17:37:09 2021 +0900

    [v2.0.0] pick-git 배포 (#715)

    ...
```

<br/>

## 추가

### Without a message

```
% git tag {tag_name}
```

```
// 예시
% git tag v2.1.1
```

### With a message

```
% git tag -a {tag_name} -m "{tag_message}"
```

```
// 예시
% git tag -a v2.1.1 -m "pick-git v2.1.1 배포"
```

<br/>

## 삭제

### Local

```
% git tag -d {tag_name}
```

```
// 예시
% git tag -d v2.1.1
'v2.1.1' 태그 삭제함 (과거 387f1309)
```

### Remote

```
% git push origin :{tag_name}
```

<br/>

## 원격 저장소 업로드

### Each tag

```
% git push origin {tag_name}
```

### All tags

```
% git push origin --tags
```

<br/>

## References

- [2.6 Git Basics - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- [[Git]Tag 추가, 변경 및 삭제하기](http://minsone.github.io/git/git-addtion-and-modified-delete-tag)