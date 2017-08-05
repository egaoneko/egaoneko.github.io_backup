---
layout: post
title: 'Critical Rendering Path 최적화 도전기'
date: 2017-08-05 00:00:00
categories: Frontend
tags: [CRP, frontend, performance, optimization]
published: false
fullview: false
comments: true
---

최근 브라우저가 어떻게 작동하는지 궁금하던 차에 회사 직원분의 공유를 통해 [브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](http://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)라는 글을 접하게 되었다. 해당 글에서 흥미로운 주제인 **주요 렌더링 경로(Critical Rendering Path, 이하 CRP)**에 대해서 접하게 되어 이와 관련된 Udacity 강좌와 여러 글들에 대해 찾아보게 되었고, 이를 바탕으로 현재 재직하고 있는 회사의 웹페이지에 해당 개념을 적용해보는 도전기를 공유하기 위해 이 글을 작성하고자 한다.

본 글에서는 CRP에 대해 상세한 내용을 다루지 않을 것이기 때문에 상세한 내용이 궁금하다면 글 맨 하단의 참조 링크들을 확인하길 바란다. 특히 Google에서 제작한 Udacity의 [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)와 [Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)는 쉽게 설명해주어 좋은데, 심지어 번역도 되어있으니 처음 본다면 꼭 한번 보면 좋을 것 같다.

앞서 언급하였듯이 이 글은 재직하고 있는 회사의 웹페이지에 CRP 최적화를 시도해본 도전기를 공유하고자 작성하였으며, 측정 도구로는 Google에서 제작한 [Lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)를 사용하였다. Lighthouse를 사용하여 측정할 때, CRP에 대해서만 측장하기 위해 **Performacne**옵션만 설정하여 측정하였다.

## 주요 렌더링 경로(Critical Rederning Path)

위에서 언급하였듯이 본 글에서는 CRP에 대해서 간단히 언급만 하고 넘어가고자 한다.

### CRP 란

HTML, CSS 및 자바스크립트 바이트를 수신한 후 렌더링된 픽셀로 변환하기 위해 필요한 처리까지 그 사이에 포함된 단계, 즉, 브라우저가 서버에서 응답을 받아 하나의 화면을 그려내는 것을 주요 랜더링 경로(CRP)라고 한다.

[브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](http://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)에서 아래와 같이 해당 과정에 대해서 잘 요약해서 설명해주셨다.

![critical rendering path]({{ site.baseurl }}/assets/media/critical-rendering-path/critical-rendering-path.png)
**[브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](http://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)**

1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다.
1. HTML을 파싱한 결과로 DOM Tree(Document Object Model)를 만든다.
1. 파싱하는 중 CSS 파일 링크를 만나면 CSS 파일을 요청해서 받아온다.
1. CSS 파일을 읽어서 CSSOM(CSS Object Model)을 만든다.
1. DOM Tree와 CSSOM이 모두 만들어지면 이 둘을 사용해 Render Tree를 만든다.
1. Render Tree에 있는 각각의 노드들이 화면의 어디에 어떻게 위치할 지를 계산하는 Layout과정을 거쳐서,
1. 화면에 실제 픽셀을 Paint한다.

CRP를 최적화하는 작업은 위 단계에서 1단계~6단계를 수행할 때 걸린 총 시간을 최소화하는 프로세스이다.

#### HTML Parsing

* `basic_dom.html`

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
    <title>Critical Path</title>
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
</html>
```

* `style.css`

```css
body { font-size: 16px }
p { font-weight: bold }
span { color: red }
p span { display: none }
img { float: right }
```

* [Sample](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom.html)

![construction]({{ site.baseurl }}/assets/media/critical-rendering-path/construction.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

* 바이트 → 문자 → 토큰 → 노드 → 객체 모델

HTML과 CSS는 위와 같은 과정을 통해 DOM과 CSSOM으로 변환된다.

![full process]({{ site.baseurl }}/assets/media/critical-rendering-path/full-process.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

#### DOM(Document Object Model) Tree 생성

DOM은 HTML 문서의 객체 표현이고 외부를 향하는 자바스크립트와 같은 프로그래밍 언어와 HTML 문서의 연결 지점(인터페이스)이다. 자바스크립트를 이용해 DOM에 접근하는 경우가 많이 있지만, 그렇다고 해서 자바스크립트 언어의 한 부분은 아니며, 기타 언어들로 DOM에 접근할 수도 있다. 트리의 최상위 객체는 문서이다.

* [브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)
* [문서 객체 모델 (DOM) - MDN](https://developer.mozilla.org/ko/docs/Gecko_DOM_Reference)

![dom tree]({{ site.baseurl }}/assets/media/critical-rendering-path/dom-tree.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

HTML을 파싱하면 위와 같은 DOM Tree가 생성된다. DOM Tree는 문서 마크업의 속성 및 관계를 포함하지만 요소가 렌더링될 때 어떻게 표시될지에 대해서는 알려주지 않으며, 이것은 CSSOM의 책임입니다.

#### CSSOM(CSS Object Model) Tree

CSSOM은 자바스크립트 언어와 같은 프로그래밍 언어가 CSS를 조작 할 수있게 해주는 API 세트이며, 이를 통해 CSS양식을 동적으로 읽고 수정할 수 있다.

* [CSS Object Model - MDN](https://developer.mozilla.org/ko/docs/Web/API/CSS_Object_Model)

![cssom tree]({{ site.baseurl }}/assets/media/critical-rendering-path/cssom-tree.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

Style Sheet를 파싱하면 위와 같은 CSSOM Tree가 생성된다.

#### Render Tree

HTML 및 CSS 입력을 기반으로 빌드한 서로 독립적인 객체인 DOM 및 CSSOM 트리를 병합하여 브라우저가 화면에 픽셀을 렌더링하도록 Render Tree를 형성한다. 이때 Render Tree에는 페이지를 렌더링하는데 필요한 노드만 포함된다.

![render tree construction]({{ site.baseurl }}/assets/media/critical-rendering-path/render-tree-construction.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction/?hl=ko)**

모든 노드의 콘텐츠 및 스타일 정보를 모두 포함하는 Render Tree가 생성되었으므로 Layout단계로 진행할 수 있다.

#### Layout(Reflow)

브라우저는 Layout단계에서 앞서 생성한 표시할 노드와 해당 노드의 계산된 스타일을 포함하는 Render Tree를 사용하여, 루트에서 시작하여 트래버스하며 페이지에서 각 객체의 정확한 크기와 위치를 파악한다.

Layout 프로세스에서는 뷰포트 내에서 각 요소의 정확한 위치와 크기를 정확하게 캡처하는 **상자 모델**이 출력되며, 모든 상대적인 측정값은 화면에서 절대적인 픽셀로 변환된다.

* `nested.html`

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Critial Path: Hello world!</title>
  </head>
  <body>
    <div style="width: 50%">
      <div style="width: 50%">Hello world!</div>
    </div>
  </body>
</html>
```

![layout viewport]({{ site.baseurl }}/assets/media/critical-rendering-path/layout-viewport.png)
**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction/?hl=ko)**

* [Sample](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/nested.html)

위 예제에는 두 가지 중첩된 `div`가 포함되어 있다. 첫 번째(상위) `div`는 노드의 표시 크기를 뷰포트 너비의 `50%`로 설정하며, 두 번째 `div`(상위 `div`에 포함된)는 해당 너비를 상위 항목 너비의 `50%`(즉, 뷰포트 너비의 `25%`)로 설정하였다.

이때 각 `div`의 크기는 다음과 같다.

* `body` : `320px`(`width:100%`)
* 첫 번째 `div` : `160px`(`width:50%`)
* 두 번째 `div` : `80px`(`width:50%`)

#### Paint(Repaint)

이 단계에서 브라우저는 Render Tree의 각 노드들을 실제로 화면에 그리게된다.

Render Tree 생성, Layout 및 Paint 작업을 수행하는데 필요한 시간은 문서의 크기, 적용된 스타일 및 실행 중인 기기에 따라 달라진다. 즉, 문서가 클수록 브라우저가 수행해야 하는 작업도 더 많아지며, 스타일이 복잡할수록 페인팅에 걸리는 시간도 늘어난다.

예를 들면, 단색은 페인트하는데 시간과 작업이 적게 필요한 반면, 그림자 효과는 계산하고 렌더링하는데 시간과 작업이 더 필요하다.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZTnIxIA5KGw" frameborder="0" allowfullscreen></iframe>

위 영상은 Gecko에서 reflow를 거쳐서 화면에 paint되기까지를 보여준다.

> 앞에서 설명한 각 과정들은 많은 부분이 생략되어 있다. 특히 Layout과 Paint 부분은 많이 생략되어 있으므로, 해당 부분이 궁금하다면 [브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)를 참고하면 좋을 것 같다.