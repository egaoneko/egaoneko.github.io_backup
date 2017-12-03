---
layout: post
title: 'debounce 와 throttle'
date: 2017-03-19 20:30:00
categories: JavaScript
tags: [JavaScript, debounce, throttle]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## 들어가기 앞서

본 포스트에서는 [Lodash](hhttps://lodash.com/)를 사용하였다.

## debounce 와 throttle

얼마전 [자바스크립트 코딩 면접에서 알고 있어야 할 3가지 질문](https://joshua1988.github.io/web_dev/javascript-interview-3questions/)라는 글을 읽고 세번째 질문에서 언급된 `디바운싱 (Debouncing)`을 보고 상당히 놀랐었다.

왜냐하면 이 글을 보기전에는 `debounce`과 `throttle`라는 것을 전혀 알지 못하고 있었지만 개발을 하면서 `throttle`과 동일한 작동을 하는 코드를 작성했었기 때문이다. 웹 페이지내에 내부 데이터를 새로고침해주는 버튼이 있었는데, 서버의 부하를 줄이기 위해서 이 버튼을 자주 누를 수 없도록 1초의 딜레이를 주는 기능이었다. 이 글을 읽고는 현재 해당 기능을 구현한 로직은 제거하고 `underscore`의 `throttle`을 사용하도록 수정하였다.

### debounce

```javascript
_.debounce(func, [wait=0], [options={}])
```

> Creates a debounced function that delays invoking `func` until after `wait` milliseconds have elapsed since the last time the debounced function was invoked. [debounce - lodash](https://lodash.com/docs/4.17.4#debounce)

먼저 `debounce`는 마지막 호출 이후 일정 시간이 지난 후에 등록된 함수를 실행한다.

CSS-TRICKS의 [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)에서 `debounce`를 엘레베이터에 비교하여 적절하게 잘 설명되어 있다. 엘레베이터의 문이 닫히기 시작할 때 누군가 타기를 시도한다면, 엘레베이터는 다른 층으로 이동하지 않고 문을 다시 열 것이다. 엘레베이터에 누군가 계속 타기를 시도한다면 엘레베이터는 계속 지연되다가 마지막으로 타기를 시도한 사람이 탄 후 일정 시간이 지난 후에 다른 층으로 이동할 것이다.

### throttle

```javascript
_.throttle(func, [wait=0], [options={}])
```

> Creates a throttled function that only invokes `func` at most once per every `wait` milliseconds. [throttle - lodash](https://lodash.com/docs/4.17.4#throttle)

반면 `throttle`은 일정 시간마다 최대 한 번만 등록된 함수를 호출한다.

스크롤은 많은 이벤트를 보내게 되는데(특히 trackpad), 무한 스크롤에 `onscroll` 이벤트를 바인딩한 경우 Ajax 요청을 많이 보내게 된다. 이때 `throttle`를 사용한다면 과다한 요청을 방지할 수 있다.

## 결론

CSS-TRICKS의 [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)에서 내린 결론을 보자면,

* `debounce` : 갑작스런 이벤트를 하나의 이벤트로 그룹화 할 때,
* `throttle` : 일정 시간마다 실행의 흐름을 일정하게 보장할 때,

사용하면 될 것 같다.

## 참고

* [Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)
* [[lodash/underscore] throttle과 debounce](https://hyunseob.github.io/2016/04/24/throttle-and-debounce/)
* [debounce source - lodash](https://github.com/lodash/lodash/blob/4.8.0-npm/debounce.js)
* [throttle source - lodash](https://github.com/lodash/lodash/blob/4.8.0-npm/throttle.js)
