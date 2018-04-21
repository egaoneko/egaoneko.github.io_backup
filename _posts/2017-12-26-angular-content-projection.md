---
layout: post
title: '[Angular] Content projection'
date: 2017-12-26 23:20:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

둘러쌓고 있는 부모 컴포넌트 내부에 있는 자식 컴포넌트를 원하는 부모의 템플릿 위치에 자식 컴포넌트를 넣고 싶어서 AngularJS에서 `Transclusion`이라고 부르던 것을 찾아보았다. Angular에서는 `Content projection`로 바뀌어 불리는 것을 몰라 한참 찾았다.

```html
<div style="border: 1px solid black;">
  <div style="background-color: gray">{{title}}</div>
  <ng-transclude></ng-transclude>
</div>
```

예를 들어 AngularJS에서 위와 같이 작성한 코드가 있다면,

```html
<div style="border: 1px solid black;">
  <div style="background-color: gray">{{title}}</div>
  <ng-content></ng-content>
</div>
```

Angular에서는 위와 같이 변경해주면 될 것이다.

`<ng-transclude></ng-transclude>`이 `<ng-content></ng-content>`로 변경되었다.

자세한 추가적인 내용들은 하단의 참조 링크들을 확인하면 좋을 것 같다.

아직 Angular를 제대로 본적이 없어서 앞서 말한 이유에 의해 사용할 때, `Content projection`이 사용하는 것이 더 좋은 선택인지 `ngTemplateOutlet`을 사용하는 것이 더 좋은 선택인지 좀 더 알아봐야 할 것 같다.

* 참고
  * [Content projection - Angular](https://angular.io/guide/lifecycle-hooks#content-projection)
  * [ngTransclude - AngularJS](https://docs.angularjs.org/api/ng/directive/ngTransclude)
  * [부모 컴포넌트에서 자식 요소로의 접근 - PoiemaWeb](http://poiemaweb.com/angular-component-accessing-child#21-콘텐트-프로젝션content-projection)
  * [ng-content: The hidden docs](https://medium.com/claritydesignsystem/ng-content-the-hidden-docs-96a29d70d11b)
  * [From transclusion to content projection](https://developer.telerik.com/topics/web-development/transclusion-content-projection/)
  * [Projection](https://angular-2-training-book.rangle.io/handout/components/projection.html)
  * [NgTemplateOutlet - Angular](https://angular.io/api/common/NgTemplateOutlet)