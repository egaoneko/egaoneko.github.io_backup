---
layout: post
title: '[Angular] iframe src is unsafe value'
date: 2018-01-30 00:20:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

```xml
<iframe width="100%" height="300" [src]="site.url"></iframe>
```

Angular에서 상단과 같이 바로 `iframe`을 사용했더니 아래와 같은 오류가 발생하였다.

```
Error: unsafe value used in a resource URL context
at DomSanitizationServiceImpl.sanitize...
```

이를 해결하기 위해서 아래와 같이 파이프를 정의해서 해결하였다.

```typescript
import { Pipe, PipeTransform } from '@angular/core';
import { DomSanitizer} from '@angular/platform-browser';

@Pipe({ name: 'safe' })
export class SafePipe implements PipeTransform {
  constructor(private sanitizer: DomSanitizer) {}
  transform(url) {
    return this.sanitizer.bypassSecurityTrustResourceUrl(url);
  }
} 
```

```xml
<iframe width="100%" height="300" [src]="site.url | safe"></iframe>
```

* 참고
  * [Trusting safe values - Angular](https://angular.io/guide/security#bypass-security-apis)
  * [How to set iframe src in Angular 2 without causing `unsafe value` exception? - stack overflow](https://stackoverflow.com/questions/38037760/how-to-set-iframe-src-in-angular-2-without-causing-unsafe-value-exception)