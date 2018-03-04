---
layout: post
title: 'Canvas ImageData 복사'
date: 2018-03-04 20:00:00
categories: graphics
tags: [graphics, canvas, til]
published: true
fullview: false
comments: true
---

```javascript
function copyImageData(ctx, src) {
  var dst = ctx.createImageData(src.width, src.height);
  dst.data.set(src.data);
  return dst;
}
```

Canvas에서 `getImageData`로 가져온 `ImageData`를 복사하고 싶다면 상단과 같이 복사할 수 있다. 새로운 Canvas에 `ImageData`를 `putImageData`로 넣어서 사용하기도 했었지만 `ImageData`민을 복사하여 조작하고자 하여 사용하였다.

* 참고
  * [Copy imageData by value in JavaScript - stack overflow](https://stackoverflow.com/questions/5642383/copy-imagedata-by-value-in-javascript?rq=1)
  * [latest specification](https://www.w3.org/TR/2dcontext/#dom-imagedata-data)
  * [Pixel manipulation with canvas](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)
  * [Faster Canvas Pixel Manipulation with Typed Arrays](https://hacks.mozilla.org/2011/12/faster-canvas-pixel-manipulation-with-typed-arrays/)