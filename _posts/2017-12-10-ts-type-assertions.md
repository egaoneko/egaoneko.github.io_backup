---
layout: post
title: 'Type assertions'
date: 2017-12-10 23:30:00
categories: typescript
tags: [typescript, til]
published: true
fullview: false
comments: true
---

```javascript
interface Test {
    test: string;
}
const a: Test = { test: 'Test' };
const b: Test = { test: 'Test' } as Test;
const c: Test = <Test>{ test: 'Test' };
```

`a`와 같이 단순히 직접 작성한 `Object`를 할당하는 것보다 `b`나 `c`와 같이 직접 작성한 값에 타입을 할당하는게 좋다. `c` 같은 경우는 JSX에서 문제가 될 수도 있으니, 상황에 따라 판단해서 써야할 것 같다.

```javascript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

```javascript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

TypeScript 문서에서 예제가 잘 나와있다. 위와 같이 `any`에서 `string`으로 할당하면 IDE의 자동완성 등 도움을 받을 수 있다.

* 참고
  * [Type assertions](https://www.typescriptlang.org/docs/handbook/basic-types.html#type-assertions)
