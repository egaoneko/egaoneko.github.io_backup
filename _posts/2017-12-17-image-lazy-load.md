---
layout: post
title: 'Image lazy load'
date: 2017-12-17 18:00:00
categories: frontend
tags: [image, javascript, til]
published: true
fullview: false
comments: true
---



## 들어가며

많은 이미지가 있고 페이지에 보이지도 않는 이미지를 미리 받아둘 필요가 없는 경우가 생겼다. 이를 위해 이미지를 나중에 로드하는 방법을 찾아보았고 그 방법을 기록하고자 한다.

## Vanilla

<figure><img src="/images/frontend/lazy-load-image-with-vanilla.gif" alt=""></figure>

```javascript
var offsetWidth = 200;
var offsetHeight = 100;
var scrollBox = document.getElementById('scroll-box');

...

lazyLoadImageSet = Array.from(document.querySelectorAll('.image-block > img'));

scrollBox.addEventListener('scroll', function(){
  loadImages(lazyLoadImageSet);
});

...

function loadImages(images) {
  var scrollRect = {
    top: scrollBox.scrollTop - offsetHeight,
    left: scrollBox.scrollLeft - offsetWidth,
    right: scrollBox.scrollLeft + scrollBox.offsetWidth + offsetWidth,
    bottom: scrollBox.scrollTop + scrollBox.offsetHeight + offsetHeight
  };

  images.slice().forEach(function(image) {
    var imageRect = {
      top: image.offsetTop,
      left: image.offsetLeft,
      right: image.offsetLeft + image.clientWidth,
      bottom: image.offsetTop + image.clientHeight
    };

    if (!collisionCheck(scrollRect, imageRect)) {
      return;
    }

    if (image.src) {
      return;
    }

    loadImage(image);
    images.splice(images.indexOf(image), 1);
  });
}

function collisionCheck(scrollRect, imageRect) {
  return(
    imageRect.top > scrollRect.top &&
    imageRect.left > scrollRect.left &&
    imageRect.right < scrollRect.right &&
    imageRect.bottom < scrollRect.bottom
  );
}

function loadImage(image) {
  image.src = image.getAttribute('data-src');
}
```

* [예제](https://egaoneko.github.io/playground/#/examples/image-lazy-load-vanilla)

Vanilla라고 이름을 붙이긴 했지만 이 글에서 이야기하고자 하는 방법들 모두 Vanilla를 쓰기 때문에, 다음에 이야기할 방법과 달리 새로운 API를 사용하지 않고 작성하였기에 Vanilla를 붙였음을 먼저 언급한다.

이 방법에서는 `scroll`을 추적하여 lazy load할 이미지들 중 보이는 이미지들의 `src`에 주소를 할당하여 이미지를 불러오도록 처리하였다. 이 방법에서 이미지들이 보이는지 여부를 확인하기 위하여 `collisionCheck`라는 별도의 메서드를 만들어 충돌을 확인하였다.

## IntersectionObserver

<figure><img src="/images/frontend/image-lazy-load-with-intersection-observer.gif" alt=""></figure>

```javascript
var scrollBox = document.getElementById('scroll-box');
var imageBox = document.getElementById('image-box');

...

var io = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
      if (!entry.isIntersecting) {
          return;
      }

      var target = entry.target;

      loadImage(target);
      observer.unobserve(target);
  });
});

...

lazyLoadImageSet.forEach(el => {
  io.observe(el);
});

...

function loadImage(image) {
  image.src = image.getAttribute('data-src');
}
```

* [예제](https://egaoneko.github.io/playground/#/examples/image-lazy-load-intersection-observer)

두 번째 방법은 [IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)를 사용해서 구현히였다. `IntersectionObserver`는 대상 엘리먼트가 루트 엘리먼트에 교차하기 시작하거나 끝났을 때(보이거나 보이지 않을 때) 비동기로 이벤트를 발생시킨다. 이를 이용해 이미지 엘리먼트들을 관찰하고 있다가 이미지 엘리먼트가 보여서 이벤트가 발생했을 때, 이미지 주소를 할당하고 이미지 엘리먼트에 대한 관찰을 해제하는 방식으로 구현하였다.

이 방법은 앞서 설명한 방법에서의 `getBoundingClientRect`를 사용으로 인한 리플로우 현상으로 인한 성능 문제 및 `iframe`에서의 사용 등의 문제를 해결할 수 있고, 충돌 처리등의 추가적인 코드 작성이 없다.

일부 브라우저 및 버전에 따라 `IntersectionObserver`가 제공되지 않을 수 있으나, [Intersection Observer polyfill](https://github.com/w3c/IntersectionObserver)를 제공하고 있으니 사용하여보면 좋을 것 같다.

* P.S.

`IntersectionObserver`는 엘리먼트가 교차할 때 발생하기 때문에 계속 보이고 있다고 해서 지속적으로 이벤트가 발생하지는 않아 문제가 생길 수 있다. 예를 들면 `IntersectionObserver` 무한 스크롤을 구현하였는데, 만약 감지를 걸어둔 부분이 계속 보인다면 뜻하지 않게 동작할 수 있다.

## 참고

* [Lazy Loading Images - CSS-TRICS](https://css-tricks.com/snippets/javascript/lazy-loading-images/)
* [IntersectionObserver - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
* [IntersectionObserver를 이용한 이미지 동적 로딩 기능 개선](http://tech.lezhin.com/2017/07/13/intersectionobserver-overview)