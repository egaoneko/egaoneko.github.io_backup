---
layout: post
title: '[Angular] Callback과 EventEmitter'
date: 2017-12-25 02:05:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

```typescript
import { Directive, Input } from '@angular/core';

@Directive({
  selector: '[example]'
})
export class ExampleDirective {

  @Input('example') example: Function;
  ...
}
```

```xml
<div [example]="testFunction"></div>
```

상단과 같이 `function`을 받아서 특정 시점에 이벤트를 발생시키는 Directive를 작성하면서 무의식 중에 `example`에 Callback 메서드를 받아 해당 시점에 실행시켜주었다.

리뷰를 받으면서 Angular에서는 Callback보다는 `EventEmitter`가 어울리지 않냐는 이야기를 들었다. 당연 스럽게 `@Input` 데코레이터를 통해 받아야한다고만 생각했고, `EventEmitter`를 사용해야한다는 생각은 해보지를 못했다.

```typescript
import { Directive, Output } from '@angular/core';

@Directive({
  selector: '[example]'
})
export class ExampleDirective {

  @Output('example') example: EventEmitter<any> = new EventEmitter();
  ...
}
```

```xml
<div (example)="testFunction"></div>
```

`EventEmitter`를 사용하는 방식으로 상단과 같이 수정해보니, 걱정과 달리 잘 동작하였다. 가능하다면 `EventEmitter`를 사용하는 쪽이 옳아보인다.

* 참고
  * [EventEmitter - Angular](https://angular.io/api/core/EventEmitter#eventemitter)