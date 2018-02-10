---
layout: post
title: 'Transition with auto'
date: 2018-01-07 18:00:00
categories: frontend
tags: [css, transition, til]
published: true
fullview: false
comments: true
---



## 들어가며

원 요소의 `height`가 `auto`로 잡혀있고 특정 값으로 변경했다가 다시 `auto`로 `transition`을 통해서 요소의 크기를 조정했다. 이때 `auto`는 `transition`이 동작하지 않는 다는 사실을 몰라 많은 시간을 낭비해 기록해둔다.

## 원인

`height`의 `auto` 혹은 `none` 로 되어있을 때는 `transition`이 발생하지 않는다. 브라우저에서 이를 지원하려면 `transition`이 발생할 때, `auto`를 측정하기 위해 다른 요소들이 얼마나 이동했는지 확인하기 위해 `reflow`가 일어나 성능상 좋지 못하다고 한다.

## 해결

해결은 필요한 변경에 대한 값들을 미리 변수에 저장해두고 이를 바탕으로 고정 `height`값으로 조정하여 해결하였다.

## 추가

이를 해결해보기 위해서 몇가지 시도를 했었는데, `max-height`도 그중에 하나이다. `max-height`로 값을 제한하고 `auto`대신 큰 값을 주어 처리하는 방식이었다. 이 방식으로도 어느정도 동작을 하는데, 문제는 큰 갑을 주다보면 그 값에 영향을 받아서 마치 `delay`가 생기는 것처럼 보인다.

## 참고

* [Using CSS Transitions on Auto Dimensions - CSS-Tricks](https://css-tricks.com/using-css-transitions-auto-dimensions/)
* [CSS 트랜지션 사용하기 - MDN](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)