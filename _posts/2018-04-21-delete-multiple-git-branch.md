---
layout: post
title: '복수의 Git Branch 삭제하기'
date: 2018-04-21 19:40:00
categories: git
tags: [git, til]
published: true
fullview: false
comments: true
---

```sh
$ git branch
feature/...
feature/...
release/...
release/...
hotfix/...
hotfix/...
```

Source Tree를 사용하건, Shell을 사용하건 관계없이 Git Branch들을 자주 정리해주지 않으면 금방 쌓이기 마련이다. Branch가 너무 많이 쌓이다보니 작업이 완료된 Brnach들과 작업하고 있는 Branch들이 구분되지 않아 불편하다. Source Tree를 통해서 일일이 지우기는 곤란하고 한번에 복수의 Branch를 지우고자 알아보니 아래와 같은 명령어를 사용하면 손쉽게 정규 표현식을 사용하여 필요한 Branch만 제거가 가능하다.

```sh
git branch -D `git branch | grep -E 'feature/*'`
```

## Reference

* [How to delete mutiple branches with one command in git?](https://git.tutorialhorizon.com/2016/07/20/how-to-delete-mutiple-branches-with-one-command-in-git/)