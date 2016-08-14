---
layout: post
title: '[Git&COMODO Firewall] Git과 COMODO Firewall을 사용할 때 마우스 우클릭 로딩 문제'
date: 2014-10-14 19:42:46
categories: Etc
tags: [comodo firewall, git]
published: true
fullview: false
comments: true
---

![screenshot]({{ site.url }}/assets/media/git.png)

Git을 설치하면 상단과 같이 "마우스 우클릭"했을 때의 메뉴에 기능들이 추가됨을 확인할 수 있습니다.

문제는 COMODO Firewall을 사용할 때 로딩이 발생합니다. 저같은 경우에는 windows 8.1 embedded를 설치한 노트북에서 발생했고 로딩은 5~6초정도 걸렸어요.

이 문제를 해결하기위해 stackoverflow에 해당 질문을 올렸지만 답변이 없어서 발생한 해당 노트북을 다시 windows7으로 설치했었네요.

오늘 다른 물어볼게 있어 stackoverflow에 접속해보니 해당 문제에 대한 해결책이 올라와있어 이를 올리고자 합니다.

> I found the setting responsible for this problem in the Behaviour Blocker of the HIPS module of Comodo Internet Security:
>
> * Rightclick the Comodo icon on the notification area and choose: "HIPS" > "Settings"
> * On the left pane, choose "Behaviour Blocker"
> * On the right pane, on the "Advanced" block, click "Exceptions" (or "Exclusions", I'm translating from a Brazilian Portuguese install)
> * Rightclick the background of the list that pops up and choose: "Add" > "Files"
> * Navigate to the Git install dir (usually "c:\program files (x86)\git\") and open the "bin" directory
> * select the "git.exe" executable and click "Open", then "OK" and then "OK" again.
>
> If this didn't work for you, try adding the "git.exe" and "sh.exe" to this list and the anti-virus exclusions.

[원본 주소](http://stackoverflow.com/questions/22077556/git-bash-is-slow-on- startup-and-commands)
