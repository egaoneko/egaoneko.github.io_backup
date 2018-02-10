---
layout: post
title: 'HTML special entity decode'
date: 2017-12-12 23:50:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

## 들어가며

```javascript
let html = '1 &amp; 2';
```

위와 같이 HTML 파싱에 문제가 없게 인코딩된 `&amp;`같은 문자들을 원래의 문자로 디코딩하는 방법을 알아보고자 한다.

## 정규표현식 사용

```javascript
let decodeEntities = (function() {
    let translateRegex = /&(nbsp|amp|quot|lt|gt);/g;
    let translate = {
            'nbsp': String.fromCharCode(160),
            'amp' : '&',
            'quot': '"',
            'lt'  : '<',
            'gt'  : '>'
        };
    let translator = function($0, $1) {
            return translate[$1];
        };

    return function(s) {
        return s.replace(translateRegex, translator);
    };
})();
```

정규표현식을 사용하여 변환해야할 문자들을 변경하는 방법이다. 변경해야되는 문자열이 더 있다면 목록을 추가해줘야 한다.

## DOM 사용

```javascript
let decodeEntities = function(encodedString) {
    var textArea = document.createElement('textarea');
    textArea.innerHTML = encodedString;
    return textArea.value;
}
```

DOM을 사용하는 방식이다. DOM에서 변경할 문자열을 넣고, 자동변환된 문자열을 받아서 사용한다.

## 결론

개인적으로는 DOM을 사용하는 방법이 짧고 간결해서 좋았지만, 사용이 잦고 DOM Element를 캐시하지 못하는 상황에서는 부담스러워서 정규표현식을 사용해서 처리했다.