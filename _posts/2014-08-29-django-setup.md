---
layout: post
title: '[Django] Django 설치'
date: 2014-08-29 13:14:49
categories: python
tags: [python, django]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## Django 설치에 앞서

이 문서에서 다루고자 하는 Django설치는 직접 Django 아카이브를 다운받아 설치하고자 하는 방법이 아닌 pip를 사용하는 방법을 다루고자 한다. 이에 대한 내용은 [Django 공식홈페이지](https://www.djangoproject.com/download/)에 잘나와있다. 이 문서를 작성하는 시점(2014년 8월 14일)의 최근 공식 버전은 1.6.5.이며 이를 설치하였다.

* * *

## 윈도우에서 pip 설치

### 윈도우 환경변수 세팅

윈도우 환경변수 세팅 방법 : 제어판\시스템 및 보안\시스템 이동 → 고급 시스템 설정 → 시스템 변수의 path클릭 → 편집 → 확인

> path에 추가 C:\Python2x(사용하는 버전); C:\Python2x\Scripts;


### get-pip.py 다운로드(다른 이름으로 링크 저장)

> [get-pip.py](https://bootstrap.pypa.io/get-pip.py)


### easy_install & pip 설치

콘솔창을 실행하고 get-pip.py를 저장한 곳으로 이동하여 아래와 같은 순서로 입력한다.

{% highlight bash %}
python get-pip.py
easy_install pip
{% endhighlight %}

### pip 설치 확인

> 콘솔창을 실행하고 pip를 입력한다.


## pip를 사용하여 Django 설치

pip를 설치하였다면 정말 간단하게 Django를 설치할 수 있다.

### 최근 공식 버전 설치

{% highlight bash %}
pip install Django==1.6.5
{% endhighlight %}

### 최근 개발 버전 설치

{% highlight bash %}
pip install https://www.djangoproject.com/download/1.7c2/tarball/
{% endhighlight %}
