---
title: '[Git] git reset --hard 되돌리기 (feat. git reflog)'
author: da-nyee
date: 2021-01-05 06:01:38 +0900
categories: [TIL, Git]
tags: [git, command]
---

## Introduction

git commit을 하다가 이전 commit으로 돌아가고 싶을 때, 주로 `git reset --hard HEAD~1`를 사용한다.<br/>
(이때, `--hard`는 다른 옵션들과 달리 파일 내용을 완전히 삭제시키기 때문에 주의해야 한다.)<br/>

git reset은 정말 필요할 때 사용하기 때문에 git reset 이전으로 되돌릴 일이 거의 없으나,<br/>
실수로 옵션을 잘못 주는 경우에는 `git reflog`로 commit log를 확인하여 git reset 이전으로 되돌린다.<br/>

이번 포스팅을 통해 commit을 git reflog로 확인하고, git reset --hard 이전으로 되돌리는 방법을 알아보자.<br/>

## git reset \<option\>

git reset에 줄 수 있는 옵션은 3가지가 있다. 차이점은 그림에 잘 나타나 있지만, 텍스트로도 짚고 넘어가자.<br/>

![git_reset_options](https://user-images.githubusercontent.com/50176238/103574220-e41c5600-4f12-11eb-9762-1b9503314d19.jpg)<br/>

### \-\-hard

`git log --oneline`으로 commit log를 확인한다.<br/>
```
$ git log --oneline
ab43d66 (HEAD -> master) docs: Update README.md
1fd1a15 docs: Update README.md
fb58071 docs: Update README.md
```

`git reset --hard HEAD~1`을 하고, 다시 commit log를 확인한다.<br/>
```
$ git reset --hard HEAD~1
HEAD is now at 1fd1a15 docs: Update README.md

$ git log --oneline
1fd1a15 (HEAD -> master) docs: Update README.md
fb58071 docs: Update README.md
```

`git status`로 working directory, stage의 상태를 확인한다.<br/>
```
$ git status
On branch master
nothing to commit, working tree clean
```

파일 내용이 완전히 삭제되어 `working directory, stage 어디에도 남아있지 않은 것`을 확인할 수 있다.<br/>

### \-\-mixed

`git log --oneline`으로 commit log를 확인한다.<br/>
```
$ git log --oneline
1fd1a15 (HEAD -> master) docs: Update README.md
fb58071 docs: Update README.md
```

`git reset --mixed HEAD~1`을 하고, 다시 commit log를 확인한다.<br/>
```
$ git reset --mixed HEAD~1
Unstaged changes after reset:
M       README.md

$ git log --oneline
fb58071 (HEAD -> master) docs: Update README.md
```

`git status`로 working directory, stage의 상태를 확인한다.<br/>
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

파일 내용이 유지되어 `working directory에 남아있는 것`을 확인할 수 있다.<br/>

### \-\-soft

`git log --oneline`으로 commit log를 확인한다.<br/>
```
$ git log --oneline
2d54abb (HEAD -> master) docs: Update README.md
fb58071 docs: Update README.md
```

`git reset --soft HEAD~1`을 하고, 다시 commit log를 확인한다.<br/>
```
$ git reset --soft HEAD~1

$ git log --oneline
fb58071 (HEAD -> master) docs: Update README.md
```

`git status`로 working directory, stage의 상태를 확인한다.<br/>
```
$ git status
On branch master        
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
```

파일 내용이 유지되어 `stage에 남아있는 것`을 확인할 수 있다.<br/>

## git reflog

🤔 이미 `--hard`로 파일 내용을 삭제했는데, 특정 commit으로 되돌아가야 한다면 어떻게 해야 할까?<br/>
😏 `git reflog`로 모든 commit log을 확인하고, `git reset`에 commit hash 값을 주어 되돌리면 된다!<br/>

`git reflog`로 모든 commit log를 확인한다.<br/>
```
$ git reflog
0bc79e7 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1       
8d9ea8f HEAD@{1}: commit: docs: Update README.md
0bc79e7 (HEAD -> master) HEAD@{2}: commit: docs: Update README.md
:...skipping...
```

`git reset --hard <특정 commit hash 값>`을 하고, 다시 commit log를 확인한다.<br/>
```
$ git reset --hard 8d9ea8f
HEAD is now at 8d9ea8f docs: Update README.md

$ git log --oneline
8d9ea8f (HEAD -> master) docs: Update README.md
0bc79e7 docs: Update README.md
fb58071 docs: Update README.md
```

특정 commit으로 git reset이 되돌려진, 즉 `commit이 복구된 모습`을 확인할 수 있다.<br/>

## References

- [What's the difference between git reset --mixed, --soft, and --hard?](https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard)
- [git reflog로 hard-reset되돌리기](https://junwoo45.github.io/2019-05-27-git_reflog/)