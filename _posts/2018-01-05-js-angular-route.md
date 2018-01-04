---
layout: post
title: '[Angular] 동일 컴포넌트로 라우트 이동'
date: 2018-01-05 01:10:00
categories: javascript
tags: [angular, til]
published: true
fullview: false
comments: true
---

![Angular navigate same component]({{ site.baseurl }}/assets/media/angular/angular-navigate-same-component.png)

```
# From
https://egaoneko.github.io/mandal-art/mandal-art/1

# To
https://egaoneko.github.io/mandal-art/mandal-art/2
```


위 이미지와 같이 컴포넌트를 불러온 상태에서 동일 컴포넌트로 라우트를 이동할 때, `OnInit`메서드가 작동하지 않는다. 이는 라우트가 동일 컴포넌트로 이동할 때 이를 재사용하기 때문인데, 자세한 내용은 아래 링크를 확인하길 바란다.

>In this example, you retrieve the route parameter map from an Observable. That implies that the route parameter map can change during the lifetime of this component.
>
>They might. By default, the router re-uses a component instance when it re-navigates to the same component type without visiting a different component first. The route parameters could change each time.
>
>...
>
>Unfortunately, ngOnInit is only called once per component instantiation. You need a way to detect when the route parameters change from within the same instance. The observable paramMap property handles that beautifully.
>
>[Observable paramMap and component reuse - Angular](https://angular.io/guide/router#observable-parammap-and-component-reuse)

```typescript
constructor(route:ActivatedRoute) {
  route.params.subscribe(val => {
    // put the code from `ngOnInit` here
  });
}
```

상단과 같은 방법으로 이를 우회할 수 있다.

* 참고
  * [Observable paramMap and component reuse - Angular](https://angular.io/guide/router#observable-parammap-and-component-reuse)
  * [Router Navigate does not call ngOnInit when same page - stack overflow](https://stackoverflow.com/questions/41678356/router-navigate-does-not-call-ngoninit-when-same-page)
  * [Angular2 router 2.0.0 not reloading components when same url loaded with different parameters? - stack overflow](https://stackoverflow.com/questions/39533291/angular2-router-2-0-0-not-reloading-components-when-same-url-loaded-with-differe/39533351#39533351)