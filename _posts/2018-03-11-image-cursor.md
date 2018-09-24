---
layout: post
title: 'CSS에서 이미지 마우스 커서 사용하기'
date: 2018-03-11 16:00:00
categories: css
tags: [css, til]
published: true
fullview: false
comments: true
---

기본으로 구현된 커서가 아닌 커스텀된 커서가 필요하여 알아보다보니 [Changing the Cursor with CSS for Better User Experience (or Fun)](https://css-tricks.com/using-css-cursors/) 이라는 글을 보게되어 몇가지 방법을 시도해보았다.

제일 먼저 `Element`를 JavaScript로 이동하여 구현하기 손쉬어 포인터처럼 구현하는 것을 시도해봤는데 해당 `Element`와 바탕이 되는 `Element`의 이벤트 처리가 껄끄러워 제외하였다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<svg viewBox="0 0 12 12"
     version="1.1"
     xmlns:svg="http://www.w3.org/2000/svg"
     xmlns="http://www.w3.org/2000/svg"
     width="12"
     height="12">
  <circle cx="6" cy="6" r="4" stroke="#000" stroke-opacity="0.75" stroke-width="2" fill="#fff" fill-opacity="0.5"/>
</svg>
```

```css
.element-png {
  text-align: center;
  color: #ffffff;
  padding: 30px;
  width: 300px;
  height: 300px;
  background: red;
  border-radius: 400px;
  cursor: url(./cursor.png) 6 6, auto;  
}

.element-svg {
  text-align: center;
  color: #ffffff;
  padding: 30px;
  width: 300px;
  height: 300px;
  background: red;
  border-radius: 400px;
  cursor: url(./cursor.svg) 6 6, auto;  
}

.element-svg-base64 {
  text-align: center;
  color: #ffffff;
  padding: 30px;
  width: 300px;
  height: 300px;
  background: red;
  border-radius: 400px;
  cursor: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+PHN2ZyB2aWV3Qm94PSIwIDAgMTIgMTIiICAgICB2ZXJzaW9uPSIxLjEiICAgICB4bWxuczpzdmc9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiAgICAgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiAgICAgd2lkdGg9IjEyIiAgICAgaGVpZ2h0PSIxMiI+ICA8Y2lyY2xlIGN4PSI2IiBjeT0iNiIgcj0iNCIgc3Ryb2tlPSIjMDAwIiBzdHJva2Utb3BhY2l0eT0iMC43NSIgc3Ryb2tlLXdpZHRoPSIyIiBmaWxsPSIjZmZmIiBmaWxsLW9wYWNpdHk9IjAuNSIvPjwvc3ZnPg==) 6 6, auto;  
}
```

그외 직접 이미지가 필요한 커서의 경우 상단과 같은 `SVG`를 만들어서 `svg`, `base64`, `png`를 만들어서 사용하였다.

<figure><img src="/images/css/image-cursor.gif" alt=""></figure>

세 방법 모두 잘 작동하여서 현 프로젝트에서 관리에 용이한 쪽으로 선택하여 사용하려고 한다. `png`을 사용하는 방법의 경우 디자이너가 관리를 하게 되어, 이미지 변경에 대해 직접 신경쓰지 않아도 될 것 같다. 하지만 이 커서가 스타일이 변경되어야 되는 경우에, 단지 이 부분만 수정하면 될 것이 아니고 해당 부분은 개발자가 수정해야 하기 때문에, `svg`를 사용하는 쪽이 커서의 수정이 용이해서 선호한다.