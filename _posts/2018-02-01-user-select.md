---
layout: post
title: '드래그(선택) 방지'
date: 2018-02-01 11:30:00
categories: frontend
tags: [frontend, til]
published: true
fullview: false
comments: true
---

사용자의 드래그(선택)을 방지하기 위해서 몇가지 방법을 찾아보았고 이를 정리한다.

## JS

```javascript
document.ondragstart = function() { // 드래그 방지
  return false;
};
document.onselectstart = function() { // 선택 방지
  return false;
};
```

## CSS

```css
-webkit-touch-callout: none;
-webkit-user-select: none;
-khtml-user-select: none;
-moz-user-select: none;
-ms-user-select: none;
user-select: none;
```

* 참고
  * [GlobalEventHandlers.onselectstart - MDN](https://developer.mozilla.org/ko/docs/Web/API/GlobalEventHandlers/onselectstart)
  * [GlobalEventHandlers.ondragstart - MDN](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/ondragstart)
  * [user-select - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/user-select)
  * [user-select - CSS-Tricks](https://css-tricks.com/almanac/properties/u/user-select/)