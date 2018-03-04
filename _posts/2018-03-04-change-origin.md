---
layout: post
title: '출처 변경'
date: 2018-03-04 18:00:00
categories: frontend
tags: [frontend, til]
published: true
fullview: false
comments: true
---

```javascript
// window popup open
window.open('test.html');

// popup
window.opener.document.querySelector('#test');
```

최근 새로운 팝업을 열고 해당 팝업에서 부모창을 컨트롤해야하는 일이 있어서 위와 같이 `window.opener`를 통해 부모의 윈도우를 접근하였다.

```
SecurityError: Blocked a frame with origin "http://example.com" from accessing a cross-origin frame
```

그런데 동일 도메인 상에서 팝업을 열었는데도 `opener.document`를 접근하니 동일 출처 정책을 위반하였다는 에러가 발생하였다. 예를 들자면 `test.example.com/index.html`에서 팝업을 열었는데, `test.example.com/test.html`에서 부모의 `document`를 접근하지 못하는 상태였다.

```javascript
// parent
document.domain = 'example.com';

// child
document.domain = 'example.com';
```

알고보니 상단과 같이 스크립트를 통해 현재 도메인이나 현재 도메인의 상위 도메인으로 변경으로 페이지 출처를 변경할 수 있다는 사실을 알았다. 부모창내 코드 어디에선가 `document.domain`를 변경한 부분이 있었고, 자식창에서 똑같이 페이지 출처를 설정해줌으로써 해결할 수 있었다.


* 참고
  * [Same-origin policy - MDN](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)
  * [HTTP 접근 제어 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS)
  * [window.opener에 권한 문제로 접근할 수 없는 경우 해결방법](http://noritersand.tistory.com/281)