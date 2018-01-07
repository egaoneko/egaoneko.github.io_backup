---
layout: post
title: 'offsetHeight 와 clientHeight'
date: 2017-12-25 02:40:00
categories: frontend
tags: [frontend, til]
published: true
fullview: false
comments: true
---

`offsetHeight`와 `clientHeight`의 차이점을 찾아보다 MSDN에서 관련된 이미지들을 보았다. `offsetHeight`와 `clientHeight` 뿐만 아니라 엘리먼트에 대한 치수와 위치에 대해 확인할 수 있다.

![Element Dimension and Location]({{ site.baseurl }}/assets/media/frontend/element-dimension-and-location1.png)

![Element Dimension and Location]({{ site.baseurl }}/assets/media/frontend/element-dimension-and-location2.png)

![Element Dimension and Location]({{ site.baseurl }}/assets/media/frontend/element-dimension-and-location3.png)

추가적으로 `offsetHeight`와 `clientHeight` 같이 엘리먼트의 위치를 가져와서 계산해야하는 경우에는 Reflow를 피하기 위해 캐시를 사용해야 한다.

## P.S.

![Element Dimension and Location]({{ site.baseurl }}/assets/media/frontend/element-dimension-and-location4.png)

<iframe width="100%" height="300" src="//jsfiddle.net/y8Y32/25/embedded/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>

* [jsfiddle](http://jsfiddle.net/y8Y32/25/)

추가적으로 좋은 예제가 있어서 첨부하였다.

## P.S.

```xml
<style>
 #elem-container{
   position: absolute;
   left:     100px;
   top:      200px;
   height:   100px;
 }
</style>

<div id="elem-container">dummy</div>
<div id="output"></div>  

<script>
  function getTheStyle(){
    var elem = document.getElementById("elem-container");
    var theCSSprop = window.getComputedStyle(elem,null).getPropertyValue("height");
    document.getElementById("output").innerHTML = theCSSprop;
   }
  getTheStyle();
</script>
```

* [Window.getComputedStyle() - MDN](https://developer.mozilla.org/ko/docs/Web/API/Window/getComputedStyle)

`getComputedStyle()` 메서드를 통해서 주어진 요소의 모든 CSS 속성값을 알 수 있다. 이때 가져오는 속성값들은 해당 요소에 대하여 기본적인 연산이 반영되어 적용된 이후 결과값들이다.

* 참고
  * [Measuring Element Dimension and Location with CSSOM in Windows Internet Explorer 9 - MSDN](https://msdn.microsoft.com/ko-kr/library/hh781509(v=vs.85).aspx)
  * [difference between offsetHeight and clientHeight - stack overflow](https://stackoverflow.com/questions/4106538/difference-between-offsetheight-and-clientheight)
  * [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)