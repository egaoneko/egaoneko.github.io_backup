---
layout: post
title: 'TS에서 JSON import하기'
date: 2018-01-05 01:00:00
categories: typescript
tags: [typescript, til]
published: true
fullview: false
comments: true
---

```typescript
import * as data from './data.json'
```

상단과 같이 json에서 값을 읽어와야하는 경우가 있을때, TypeScript에서는 아래와 같은 오류가 발생한다.

```
Cannot find module 'data.json'
```

TypeScript v2+ 에서는 아래와 같은 코드를 TS 정의 파일인 `typings.d.ts`에 넣어주면 오류를 해결할 수 있다.

```typescript
declare module "*.json" {
    const value: any;
    export default value;
}
```

* 참고
  * [How to Import json into TypeScript](https://hackernoon.com/import-json-into-typescript-8d465beded79)