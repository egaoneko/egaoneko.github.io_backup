---
layout: post
title: 'AngularJS 와 opener'
date: 2017-03-19 21:30:00
categories: javascript
tags: [javascript, angularjs]
published: true
fullview: false
comments: true
---

## AngularJS 와 opener

```javascript
var service = opener.window.angular.element(opener.window.document.body).injector().get('someAngularService');

var data = service.getData();

observer.add(updateData);
function undateData() {
	data = service.getData();
}
```

새창에서 연 `angular`에서 이전 창의 `angular`의 `data`를 컨트롤해야 하는 경우에 위와 같이 `window.opener`를 사용할 수 가 있다.

```javascript
app.config(['$compileProvider', function ($compileProvider) {
  // disable debug info
  $compileProvider.debugInfoEnabled(false);
}]);
```

> Call this method to enable/disable various debug runtime information in the compiler such as adding binding information and a reference to the current scope on to DOM elements.

단, `$compileProvider.debugInfoEnabled`를 `false`로 설정하면 작동하지 않는다고 한다.

## 참고

* [stack overflow](http://stackoverflow.com/questions/17007939/accessing-parent-window-angular-scope-from-child-window)
* [$compileProvider.debugInfoEnabled](https://docs.angularjs.org/api/ng/provider/$compileProvider#debugInfoEnabled)
* [DISABLING DEBUG INFO IN ANGULAR 1.3](https://blog.thoughtram.io/angularjs/2014/12/22/exploring-angular-1.3-disabling-debug-info.html)
