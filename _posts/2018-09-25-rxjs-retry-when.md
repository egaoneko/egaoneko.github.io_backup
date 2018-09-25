---
layout: post
title: '[RxJS] retryWhen with delay and take'
date: 2018-09-25 13:40:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

RxJS에서 API 호출 실패 시 300초 간격으로 3회를 재시도하는 로직을 작성하고 싶어서 찾아보니, 아래와 같이 작성하면 원하는 대로 작동하였다.

```javascript
retryWhen((errors: any) => {
  return concat(
    errors.pipe(
      delay(300),
      take(3),
    ),
    throwError()
  );
}),
```

하지만 상단의 코드와 같이 작성하였더니, 마지막에 발생한 에러에 대한 정보를 받고자 했지만 받을 방법이 없었다.

그래서 아래와 같이 `tap`을 사용하여 에러가 발생할 때 마다 `error`에 대한 정보를 저장하고 마지막에 그 객체를 전달하는 방식으로 처리하였다.

```javascript
retryWhen((errors: any) => {
  const error = {};
  return concat(
    errors.pipe(
      delay(300),
      tap((err: any) => Object.assign(error, err)),
      take(3),
    ),
    throwError(error)
  );
}),
```

* 참고
  * [Rxjs Retry with Delay function - stack overflow](https://stackoverflow.com/questions/44979131/rxjs-retry-with-delay-function)