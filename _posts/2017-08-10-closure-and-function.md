---
layout: post
title: '클로저와 함수'
date: 2017-08-10 23:50:00
categories: javascript
tags: [javascript]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## TL;DR

* 클로저(Closuer)는 객체를 통해서 접근하는 것이 아니라, 함수(Function)를 통해서 접근한다.

## 들어가기 앞서

틈틈히 읽고 있는 [Secrets of the JavaScript Ninja, Second Edition](https://www.manning.com/books/secrets-of-the-javascript-ninja-second-edition)에서 클로저와 스코프에 대한 챕터에서 재미있는 내용을 보아서 이 글을 작성한다. 이 글에서는 클로저에 대한 설명은 하지 않을 것이다.

## 클로저를 가진 객체

```javascript
function Counter() {
    var count = 0;

    this.getCount = function () {
        return count;
    }
    this.increaseCount = function() {
        count += 1;
    }
}
var counter = new Counter();
```

위와 같이 클로저를 가진 객체가 있다.

![counter]({{ site.baseurl }}/assets/media/closuer-and-function/counter.png)

`counter` 인스턴스의 스코프에 대해 보면 위와 같다. `Counter` 생성자를 실행하면 새로운 렉시컬 환경이 생성되고, 지역변수들은 이 렉시컬 환경에 생성된다.

## 함수를 통해 접근되는 클로저

```javascript
function Counter() {
    var count = 0;

    this.getCount = function () {
        return count;
    }
    this.increaseCount = function() {
        count += 1;
    }
}
var counter = new Counter();
counter.getCount(); // 0

var fakeCounter = {};
fakeCounter.increaseCount = counter.increaseCount;
fakeCounter.increaseCount();
counter.getCount(); // 1
```

만약 위와 같이 새로운 `fakeCounter` 객체가 `counter`의 함수를 받아서 실행하면 클로저는 어떤 것을 바라보게 되는 걸까? `fakeCounter`에 새로 추가된 `increaseCount`를 실행하고 `counter`의 `getCount`을 실행하면 `count`값이 하나 증가된 것을 볼 수 있다.

 ![fakeCounter]({{ site.baseurl }}/assets/media/closuer-and-function/fake-counter.png)

 `counter` 인스턴스의 스코프에 추가된 `fakeCounter`에 대해 보면 위와 같다. 앞서 본 그림에서 보이듯이 클로저는 객체를 통해 접근하는 것이 아니라, 함수를 통해서 접근한다. 따라서 `fakeCounter`의 `increaseCount`를 통해 클로저를 접근하여 실행하면 기존 스코프에 있는 `count`값이 증가하게 되고, `counter`의 `getCount`를 통해 클로저에 접근해서 확인하면 그 값이 증가되있는 것을 볼 수 있다.