---
layout: post
title: 'TS에서 Enum 타입 검사하기'
date: 2017-12-23 22:40:00
categories: typescript
tags: [typescript, til]
published: true
fullview: false
comments: true
---

```typescript
enum TEST {
    A,
    B,
    C,
    D
}

let ENUM_TEST_ERROR: TEST = TEST;
```

Angulr에서 `enum`을 변수에 담아 `template`로 `enum`을 전달할 일이 있었다. `enum`을 담은 변수에서 타입을 검사하고 싶은데 바로 사용하면 다음과 같은 오류가 발생한다.

```
Type 'typeof TEST' is not assignable to type 'TEST'.
```

이럴 때 아래와 같이 작성하면 타입을 검사할 수 있다.

```typescript
enum TEST {
    A,
    B,
    C,
    D
}

let ENUM_TEST: typeof TEST = TEST;
```