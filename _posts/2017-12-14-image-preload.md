---
layout: post
title: 'Image preload'
date: 2017-12-14 23:50:00
categories: frontend
tags: [image, javascript, css, til]
published: true
fullview: false
comments: true
---

## 들어가며

슬라이드와 같은 방식으로 다음 이미지를 순차적으로 불러와 보여줄 때, 미리 이미지를 불러와 이미지 전환간에 깜박임없이 부드럽게 이미지를 전환하고자 하였다. 이를 위해 이미지를 미리 로드하는 두가지 방법을 기록하고자 한다.

## JavsScript

<figure><img src="/images/frontend/image-preload-with-js.gif" alt=""></figure>

<figure><img src="/images/frontend/image-preload-with-js-network.png" alt=""></figure>

```javascript
function preloadImages(images) {
  images.forEach(function(src) {
    setTimeout(() => {
      let img = new Image();
      img.src = src;
      img.onload = () => {
        img = null;
      };
    });
  });
}
preloadImages(imageSet2);
```

* [예제](https://egaoneko.github.io/playground/#/examples/image-preload-javascript)

이미지들을 순환하며 임시 이미지 앨리먼트에 넣어 미리 로드한 후에 해당 엘리먼트를 제거하는 방식으로 구현한다. JavsScript로 이미지들을 동적으로 미리 로드하고자 한다면 좋은 선택인 것 같다.

## CSS

<figure><img src="/images/frontend/image-preload-with-css.gif" alt=""></figure>

<figure><img src="/images/frontend/image-preload-with-css-network.png" alt=""></figure>

```html
<style>
  #preload-00 { background: url(https://dummyimage.com/600x400/52ACEEBE5D21) no-repeat -9999px -9999px; }
  #preload-01 { background: url(https://dummyimage.com/600x400/FD8F265C45CF) no-repeat -9999px -9999px; }
  #preload-02 { background: url(https://dummyimage.com/600x400/768D21AD6511) no-repeat -9999px -9999px; }
  #preload-03 { background: url(https://dummyimage.com/600x400/5A686067C23F) no-repeat -9999px -9999px; }
  #preload-04 { background: url(https://dummyimage.com/600x400/0E070CEAB80B) no-repeat -9999px -9999px; }
  #preload-05 { background: url(https://dummyimage.com/600x400/C13FC82B1BE7) no-repeat -9999px -9999px; }
  #preload-06 { background: url(https://dummyimage.com/600x400/B0663BC483CB) no-repeat -9999px -9999px; }
  #preload-07 { background: url(https://dummyimage.com/600x400/0244D9F1BA6F) no-repeat -9999px -9999px; }
  #preload-08 { background: url(https://dummyimage.com/600x400/3311445D45EF) no-repeat -9999px -9999px; }
  #preload-09 { background: url(https://dummyimage.com/600x400/075247DAAB1C) no-repeat -9999px -9999px; }
</style>
```

* [예제](https://egaoneko.github.io/playground/#/examples/image-preload-css)

CSS에 미리 로드할 이미지들을 등록해서 로드한다. 미리 로드해야할 이미지가 적고 정적으로 등록할 수 있다면, 추가적으로 JavaScript 코드를 작성하지 않고 사용할 수 있을 것 같다.

## AJAX

```javascript
window.onload = function() {
	setTimeout(function() {
		// XHR to request a JS and a CSS
		var xhr = new XMLHttpRequest();
		xhr.open('GET', 'http://domain.tld/preload.js');
		xhr.send('');
		xhr = new XMLHttpRequest();
		xhr.open('GET', 'http://domain.tld/preload.css');
		xhr.send('');
		// preload image
		new Image().src = "http://domain.tld/preload.png";
	}, 1000);
};
```

상단의 코드처럼 AJAX를 통해 이미지 뿐만 아니라 JavaScript나 CSS도 미리 로드를 할 수 있다.

## 참고

* [3 Ways to Preload Images with CSS, JavaScript, or Ajax](https://perishablepress.com/3-ways-preload-images-css-javascript-ajax/)
