---
layout: post
title: 'AngularJS를 최적화를 하던 중 실수'
date: 2017-03-19 21:30:00
categories: JavaScript
tags: [JavaScript, AngularJS]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## AngularJS를 최적화를 하던 중 실수

[Tips to Improve AngularJS Performance](https://medium.com/@cookatrice/tips-to-improve-angularjs-performance-410cf42de57f#.9yv6y03nd)라는 글을 보며, 기존에 AngularJS 사용하여 만든 프로그램을 최적화하는 작업을 수행하였다.

많이 양방향 바인딩({% raw %}`{{}}`{% endraw %})되어있던 것들을 단방향 바인딩({% raw %}`{{::}}`{% endraw %})으로 변경하였고, `ng-repeat`에는 `track by`를 설정하여 `Watchers` 설정을 피하고자 노력하였으며, `$scope.$evalAsync` 와 `$timeout`의 사용을 줄여 `Watchers` 실행을 줄이고자 노력하였다.

문제는 `Watchers` 설정을 줄이기 위해 수행한 행동들이 오히려 프로그램에서 버그가 되어버리는 상황이 발생하였다.

## 양방향 바인딩에서 단방향 바인딩으로 변경에서의 실수

양방향 바인딩에서 단방향 바인딩으로 변경할 때, 많이 바뀌지 않는 부분들을 단방향 바인딩으로 변경하고 이상이 없는지 확인을 하였다. 이때 팝업 같은 경우애는 이 팝업에 해당하는 객체가 변경되어면 단방향 바인딩으로 설정한 값들도 변경될 것이라고 판단하고 함께 단방향 바인딩으로 변경하였다.

{% raw %}
```xml
<div>
	<h1>{{::popup.title}}</h1>
    <p>{{::popup.content}}</p>
</div>

<script>
	...
	function ctroller() {
    	var ctrl = this;
        ctrl.popup = popupObj;
    }
    ...
</script>
```
{% endraw %}

테스트를 해보았을 때 잘 변경이되어서 넘어갔는데, 이것이 화근이되었다. 해당 페이지는 PC의 브라우저뿐아니라 모바일의 브라우저 및 웹뷰로도 보여지는 페이지였다. 해당 부분이 특정 모바일에서는 잘 작동하다가 특정 모바일에서는 작동하지 않는 현상이 발생하였고 해당 소스를 양방향 바인딩으로 변경하고서야 정상적으로 작동이 되었다.

## `ng-repeat`에는 `track by`를 설정에서의 실수

`ng-repeat`에 `track by`를 설정할때 무의식적으로 `track by $index`를 설정하였는데, 이것이 특정 상황에서는 잘 작동하여 인식하지 못하다 그렇지 못한 상황에서 오작동하고 있는 문제를 발견하였다.

```xml
<div ng-repeat="product in products track by $index">
	...
</div>
```

`ng-repeat`에서 `track by` 부분을 제거하면 정상작동을 하는 것을 확인하였고, 아래와 같이 해당 부분을 `id` 값을 통해 추적하도록 변경할 생각이다.

```xml

<div ng-repeat="product in products track by product.id">
	...
</div>
```
