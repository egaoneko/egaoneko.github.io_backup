---
layout: post
title: 'background image fallback 처리'
date: 2017-12-25 02:00:00
categories: frontend
tags: [image, css, til]
published: true
fullview: false
comments: true
---

`img` 엘리먼트를 사용하면 이미지를 불러오다 실패할 경우 `onerror`를 정의해 대체 이미지를 보여주거나 추가 작업을 할 수 있다.

이번에 관련된 작업을 하면서 `img` 엘리먼트는 앞서 말한 것처럼 fallback 처리를 하였는데, 반면 `background`에 대해 fallback 처리를 어떻게 하나 난감해 찾아보니 아래와 같은 방식으로 fallback 처리를 하였다.

```css
background: url('myimage.gif'), url('fallback.gif');
```

만약 `myimage.gif`를 불러오지 못했다면 `fallback.gif`를 볼 수 있다. 하지만 이 방법을 사용하면 `myimage.gif`를 불러왔다 하더라도 `fallback.gif`를 다운받는다.(이미지를 복수로 할당하는게 fallback 용도를 위한게 아니다.)

```css
background: image('myimage.gif', 'fallback.gif');
```

CSS3를 사용할 수 있다면 위와 같은 방식을 사용할 수 있다. 관련된 내용은 [image() notation](https://www.w3.org/TR/css3-images/#image-notation)에서 확인할 수 있다.

* 참고
  * [onerror event using background: url()](https://stackoverflow.com/questions/22287474/onerror-event-using-background-url)