---
layout: post
title: '[Angular] Angular directive 만들기'
date: 2017-12-23 23:00:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

Angulr에서 문서에 따르면 Directive는 아래와 같이 작성한다.

```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  constructor(private el: ElementRef) { }

  @Input() defaultColor: string;

  @Input('appHighlight') highlightColor: string;

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || this.defaultColor || 'red');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

그리고 아래와 같이 사용한다.

```html
<p [appHighlight]="color">Highlight me!</p>

<p [appHighlight]="color" defaultColor="violet">
  Highlight me too!
</p>
```

여기서 해맸던 부분은 파라미터를 여러개 받는 방법과 파라미터에 값을 전달하는 방법이다.

우선 파라미터를 여러개 받는 방법은 다음과 같다.

```typescript
@Input('directiveName') directiveName: string;
@Input() param1: string;
@Input('param2') param2: string;
```

`@Input` 데코레이터와 함께 작성해야하며, 경우에 따라 명시적으로 이름을 줄 수 있다.

다음은 파라미터에 값을 전달해주는 부분 아래와 같다.

* `directiveName = "function"` : `"function"`이라는 문자열이 전달
* `directiveName = "{{function}}"` : `function`의 코드가 문자열로 전달
* `[directiveName] = "function"`  : `function` 자체가 전달

기존에는 문자열을 사용해서 첫번째와 같이 사용하여도 문제가 없었는데 `function`을 전달하니 문제가 생기었다. 세번째와 같은 방버으로 사용하니 원하는 값이 제대로 전달되었다.

* 참고
  * [Attribute Directives - Angular](https://angular.io/guide/attribute-directives)
  * [How to pass multiple parameter to @Directives in Angular with TypeScript? - stack overflow](https://stackoverflow.com/questions/38843532/how-to-pass-multiple-parameter-to-directives-in-angular-with-typescript)