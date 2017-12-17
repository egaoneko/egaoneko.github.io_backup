---
layout: post
title: '[Angular] Binding targets'
date: 2017-12-17 19:00:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

```xml
<img data-src="{{heroImageUrl}}">
<button aria-label="{{help}}">help</button>
```

Angular template를 사용할 때, 엘리먼트에 속성을 설정하고자 `[]` 없이 평소 HTML 작성하듯이 작성하니 작동하지 않았다.

```xml
<img [attr.data-src]="heroImageUrl">
<button [attr.aria-label]="help">help</button>
<div [class.special]="isSpecial">Special</div>
<button [style.color]="isSpecial ? 'red' : 'green'">
```

이에 대해 찾아보니 엘리먼트에 속성, 클래스 혹은 스타일을 설정하고자 할때, 상단과 같이 `[]` 사용하여 해당 값을 전달해 줘야 정상적으로 작동하였다.

* 참고
  * [Binding targets](https://angular.io/guide/template-syntax#binding-targets)
