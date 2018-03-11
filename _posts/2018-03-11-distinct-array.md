---
layout: post
title: '배열에서 중복 값 제거하기'
date: 2018-03-11 23:00:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

배열에서 중복 값을 제거할 때 종종 `Set`이나 `Object`를 써서 처리했었는데, 얼마전 재밌는 코드를 봤다. 

```javascript
const arr = ["1", "1", "2", "3", "3", "1"];
const unique = arr.filter((value, idx, arr) => arr.indexOf(value) === idx);
// ["1", "2", "3"]
```

해당 코드를 상단과 같은 코드였는데, 처음 `(value, idx, arr) => arr.indexOf(value) === idx` 이 부분을 봤을때는 무슨 용도인가 한참 생각을 했다. callback의 세번째 인자로 넘오는 원본 배열인 `arr`을 사용해 별도의 처리 없이 중복 값을 제거하는 부분이 신선했다. 
