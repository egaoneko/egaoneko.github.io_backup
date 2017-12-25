---
layout: post
title: '엘리먼트에서 공백 처리'
date: 2017-12-25 18:00:00
categories: frontend
tags: [frontend, til]
published: true
fullview: false
comments: true
---

```javascript
let str = "Hello\nWorld!";
```

```xml
<div>{{str}}</div>
```

위와 같이 공백을 포함한 문자열이 엘리먼트 내에서 어떻게 처리되여야 정해야할 때가 있다. 경우에 따라서는 `\n`을 보존해야하는 경우도 있고 병합해야하는 경우도 있을 것이다. 이를 위해 CSS에 `white-space`라는 속성이 있댜. 공백에 대한 동작을 정리하면 아래와 같다.

|  | New lines | Spaces and tabs | Text wrapping |
| :-: | :-: | :-: | :-: |
| normal | Collapse | Collapse | Wrap |
| nowrap | Collapse | Collapse | No wrap |
| pre | Preserve | Preserve | No wrap |
| pre-wrap | Preserve | Preserve | Wrap |
| pre-line | Preserve | Collapse | Wrap |

공백 처리를 `white-space` 속성을 통해 손쉽게 하려다 `<br>`로 대체할 필요성이 있을까 싶어 물어본적이 있다. 그떄 들은 답변은 만약 CSS를 불러오지 못했을 때를 위해선 `<br>`로 표현해줘야하지 않을까라는 이야기를 들었고 이에 동의해 해당 방식으로 작성하였다.

* 참고
  * [white-space - MDN](https://developer.mozilla.org/ko/docs/Web/CSS/white-space)