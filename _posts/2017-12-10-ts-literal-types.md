---
layout: post
title: 'Literal types'
date: 2017-12-10 23:00:00
categories: typescript
tags: [typescript, til]
published: true
fullview: false
comments: true
---

```javascript
let state: 'start' | 'move' | 'stop';
state = 'move';
state = 'turn'; // Type error

let status: 100 | 200 | 300 | 400 | 500;
status = 100;
status = 700; // Type error
```

TypeScript에서 타입을 위와 같이 특정한 값을 명시적으로 나타낼 수 있다.

```javascript
enum Status {
    Informational_response = 100,
    Success = 200,
    Redirection = 300,
    Client_error = 400,
    Server_error = 500
}

let statu: Status;
statu = Status.Informational_response;
```

물론 이와 같이 `enum`을 사용허는 것이 특정한 값을 명시적으로 나타내는 것 보다 가독성이나 유지보수 측면에서 더 좋겠지만, 특정한 곳에만 쓰이는데 별도의 파일을 만들어서 정의하기 번거로울 떄는 좋은 수단이 될 것 같다.

* 참고
  * [String Literal Types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#string-literal-types)
  * [Numeric Literal Types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#numeric-literal-types)
