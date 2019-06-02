---
layout: post
title: '[Angular] HttpClient 구독 후 해지가 필요할까?'
date: 2018-09-25 13:50:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

```ts
let sub: Subscription = this.http.get(url).subscribe(()=>{});
```

`HttpClient`를 사용하여 구독하고 난뒤에 해지가 필요한가 궁금했다.

`mousemove`와 같은 DOM 이벤트는 구독 후 필수로 해지해 주고 있었지만 `HttpClient`를 사용한 후에는 알아서 구독을 종료해 준다고 하여서 따로 `unsubscribe()`를 하지는 않았다.

한편으로는 눈으로 직접 본 것이 아니어서 늘 마음한 구석이 찝찝하여, Angular 내 해당 부분을 찾아보았다.

```ts
export class XHRConnection implements Connection {
  // ...
      const response = new Response(responseOptions);
      response.ok = isSuccess(status);
      if (response.ok) {
        responseObserver.next(response);
        // TODO(gdi2290): defer complete if array buffer until done
        responseObserver.complete();
        return;
      }
      responseObserver.error(response);
  // ...
}
```

[`xhr_backend.ts`](https://github.com/angular/angular/blob/master/packages/http/src/backends/xhr_backend.ts#L91)를 살펴보면, `response.ok`인 경우에 `next`를 호출 후 `compelete`를 호출하여 종료함을 확인할 수 있다.

* 참고
  * [`xhr_backend.ts`](https://github.com/angular/angular/blob/master/packages/http/src/backends/xhr_backend.ts#L91)