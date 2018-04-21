---
layout: post
title: '[Angular] innerHTML'
date: 2017-12-13 21:00:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

```html
<div>{{text}}</div>
```

```javascript
let text = '1 &amp; 2';
```

상단과 같이 HTML special entity들이 있다면 원하는 출력 값인 `1 & 2`이 아닌 `1 &amp; 2`이 노출된다. 이를 해결하기 위해서 이전 글인 [HTML special entity decode]({% post_url 2017-12-12-html-specail-entity-decode %})에서 작성한 함수를 사용해 Pipe를 만들었다.

```html
<div [innerHTML]="text"></div>
```

```javascript
let text = '1 &amp; 2';
```

알고보니 Angular에서 `[innerHTML]` 속성을 지원하고 있어서 이를 사용하면 별도의 Pipe 사용없이 해당 이슈를 해결할 수 있었다.

* 참고
  * [Property binding or interpolation?](https://angular.io/guide/template-syntax#property-binding-or-interpolation)
