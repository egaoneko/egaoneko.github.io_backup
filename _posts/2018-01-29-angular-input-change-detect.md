---
layout: post
title: '[Angular] @Input 변화 감지'
date: 2018-01-29 23:30:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

`ngOnChanges`를 통해 컴포넌트나 디렉티브의 `input`프로퍼티에 대해 감지할 수 있다. 아래는 Angular 문서에 있는 `ngOnChanges`에 대한 예제이다.

```html
<on-changes [hero]="hero" [power]="power"></on-changes>
```

```typescript
@Input() hero: Hero;
@Input() power: string;

ngOnChanges(changes: SimpleChanges) {
  for (let propName in changes) {
    let chng = changes[propName];
    let cur  = JSON.stringify(chng.currentValue);
    let prev = JSON.stringify(chng.previousValue);
    this.changeLog.push(`${propName}: currentValue = ${cur}, previousValue = ${prev}`);
  }
}
```

* 참고
  * [OnChanges() - Angular](https://angular.io/guide/lifecycle-hooks#onchanges)