---
layout: post
title: 'Critical Rendering Path 최적화 도전기'
date: 2017-08-07 00:00:00
categories: frontend
tags: [crp, frontend, performance, optimization]
published: true
fullview: false
comments: true
---

{% include toc.html %}

최근 브라우저가 어떻게 작동하는지 궁금하던 차에 회사 직원분의 공유를 통해 [브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](http://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)라는 글을 접하게 되었다. 해당 글에서 흥미로운 주제인 **주요 렌더링 경로(Critical Rendering Path, 이하 CRP)**에 대해서 알게 되어 이와 관련된 Udacity 강좌와 여러 글들에 대해 찾아보게 되었고, 이를 바탕으로 현재 재직하고 있는 회사의 웹페이지에 해당 개념을 적용해보는 도전기를 공유하기 위해 이 글을 작성하고자 한다.

본 글에서는 CRP에 대해 상세한 내용을 다루지 않을 것이기 때문에 상세한 내용이 궁금하다면 글 맨 하단의 참조 링크들을 확인하길 바란다. 특히 Google에서 제작한 Udacity의 [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)와 [Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)는 쉽게 설명해주어 좋은데, 심지어 번역도 되어있으니 처음 본다면 꼭 한번 보면 좋을 것 같다.

앞서 언급하였듯이 이 글은 재직하고 있는 회사의 웹페이지에 CRP 최적화를 시도해본 도전기를 공유하고자 작성하였으며, 측정 도구로는 Google에서 제작한 [Lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)를 사용하였다. Lighthouse를 사용하여 측정할 때, CRP에 대해서만 측장하기 위해 **Performacne**옵션만 설정하여 측정하였다.

## 주요 렌더링 경로(Critical Rederning Path)

### CRP 란

HTML, CSS 및 JavaScript 바이트를 수신한 후 렌더링된 픽셀로 변환하기 위해 필요한 처리까지 그 사이에 포함된 단계, 즉, 브라우저가 서버에서 응답을 받아 하나의 화면을 그려내는 것을 주요 랜더링 경로(CRP)라고 한다.

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

![construction]({{ site.baseurl }}/assets/media/critical-rendering-path/construction.png)

**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

* 바이트 → 문자 → 토큰 → 노드 → 객체 모델

HTML과 CSS는 위와 같은 과정을 통해 DOM과 CSSOM으로 변환된다.

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

![full process]({{ site.baseurl }}/assets/media/critical-rendering-path/full-process.png)

**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

#### DOM(Document Object Model) Tree 생성

DOM은 HTML 문서의 객체 표현이고 외부를 향하는 JavaScript와 같은 프로그래밍 언어와 HTML 문서의 연결 지점(인터페이스)이다. JavaScript를 이용해 DOM에 접근하는 경우가 많이 있지만, 그렇다고 해서 JavaScript의 한 부분은 아니며, 기타 언어들로 DOM에 접근할 수도 있다. 트리의 최상위 객체는 문서이다.

* [브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)
* [문서 객체 모델 (DOM) - MDN](https://developer.mozilla.org/ko/docs/Gecko_DOM_Reference)

![dom tree]({{ site.baseurl }}/assets/media/critical-rendering-path/dom-tree.png)

**[Critical Rendering Path - Constructing the Object Model](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model/?hl=ko)**

HTML을 파싱하면 위와 같은 DOM Tree가 생성된다. DOM Tree는 문서 마크업의 속성 및 관계를 포함하지만 요소가 렌더링될 때 어떻게 표시될지에 대해서는 알려주지 않으며, 이것은 CSSOM의 책임입니다.

#### CSSOM(CSS Object Model) Tree

CSSOM은 JavaScript와 같은 프로그래밍 언어가 CSS를 조작 할 수 있게 해주는 API 세트이며, 이를 통해 CSS양식을 동적으로 읽고 수정할 수 있다.

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

#### 동작 과정 예

Webkit과 Gecko 브라우저에서의 동작 과정 예이다.

##### Webkit

![webkit]({{ site.baseurl }}/assets/media/critical-rendering-path/webkit.png)

**[브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)**

##### Gecko

![gecko]({{ site.baseurl }}/assets/media/critical-rendering-path/gecko.png)

**[브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)**

> 앞에서 설명한 각 과정들은 많은 부분이 생략되어 있다. 특히 Layout과 Paint 부분은 많이 생략되어 있으므로, 해당 부분이 궁금하다면 [브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)를 참고하면 좋을 것 같다.

### Rendor-Blocking

Render Tree를 생성하는데 DOM 및 CSSOM이 둘다 필요하기 때문에 HTML 및 CSS는 둘다 렌더링 차단 리소스이다. 또한, JavaScript를 사용하면 DOM 및 CSSOM을 쿼리하고 수정할 수 있기 때문에, JavaScript는 DOM 생성을 차단하고 페이지가 렌더링될 때 지연시킬 수도 있다.

HTML의 경우 DOM이 없으면 렌더링 할 것이 없기 때문에 렌더링 차단 이유가 명확하지만, CSS나 JavaScript는 요구 사항은 상황에 따라 이유가 다소 불명확할 수 있다.

아래는 렌더랑 차단 요소의 요약이다.

* HTML
    * 렌더링 차단 리소스
* CSS
    * 기본적으로는, 렌더링 차단 리소스
    * 미디어 유형과 미디어 쿼리를 통해 일부 CSS 리소스를 렌더링을 비차단 리소스로 표시가 가능 (최적화 요소)
    * 브라우저는 차단 동작이든 비차단 동작이든 관계없이 모든 CSS 리소스를 다운로드
* JavaScript
    * 명시적으로 비동기로 선언되지 않은 경우, DOM 생성을 차단
    * 자바스크립트 실행은 CSSOM을 차단

#### CSS : 차단 vs. 비차단 리소스

```xml
<link href="style.css" rel="stylesheet">
<link href="style.css"    rel="stylesheet" media="all">
<link href="portrait.css" rel="stylesheet" media="orientation:portrait">
<link href="print.css" rel="stylesheet" media="print">
<link href="other.css" rel="stylesheet" media="(min-width: 40em)">
```

#### JavaScript : 파서차단 vs. 비동기

```xml
<script>document.write("Hello, world");</script>
<script src="app.js"></script>
<script src="app.js" defer></script>
<script src="app.js" async></script>
```

![script sync and async]({{ site.baseurl }}/assets/media/critical-rendering-path/script-sync-async.png)

**[Speed up Google Maps(and everything else) with async & defer](https://medium.com/@nikjohn/speed-up-google-maps-and-everything-else-with-async-defer-7b9814efb2b)**

### CRP 측정

최적화를 하고자 하면 최적화하고자 하는 대상에 대해서 측정이 필요하다. 본 글에서는 아래의 몇가지 방법에 대해서 살표보고자 한다.

#### Chrome DevTools

[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/?hl=ko)는 Google Chrome에 내장되어있는 웹 저작 및 디버깅 도구이며, DevTools를 이용하여 사이트를 반복하고, 디버깅하고, 프로파일링할 수 있다. 자세한 도구의 사용법은 [타임라인 도구 사용법](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool?hl=ko)를 하기 바란다. DevTools를 사용하여 CRP뿐만 아니라 다양한 요소에 대해서 측정이 가능이 가능하지만, CRP를 측정면에서 본다면 다음 언급된 Lighthouse라는 도구가 더 유용하다.

![chrome devtools]({{ site.baseurl }}/assets/media/critical-rendering-path/chrome-devtools.png)

#### Lighthouse

[Lighthouse](https://developers.google.com/web/tools/lighthouse/?hl=ko)는 웹 앱 감사 도구이며 해당 페이지에 대해 일련의 테스트를 수행한 다음, 이 페이지의 결과를 통합된 보고서로 표시해준다. Lighthouse를 Chrome 확장 프로그램이나 NPM 모듈로서 실행할 수 있으며, 이는 Lighthouse와 지속적 통합 시스템을 통합하는데 유용하다.

![lighthouse]({{ site.baseurl }}/assets/media/critical-rendering-path/lighthouse.png)

특히, 아래와 같이 별도로 해당 페이지에 대한 CRP 측정 결과를 보여주므로 CRP 측정에 유용하다.

![lighthouse crp]({{ site.baseurl }}/assets/media/critical-rendering-path/lighthouse-crp.png)

#### Navigation Timing API

Navigation Timing API와 기타 여러 브라우저 이벤트를 조합해서 사용는 [Navigation Timing API](https://developer.mozilla.org/ko/docs/Navigation_timing) 접근방식에서는 [RUM(Real User Monitoring)](https://en.wikipedia.org/wiki/Real_user_monitoring) 지표를 캡처하며, 이 지표는 실제 사용자의 사이트 상호작용으로부터 캡처되며, 다양한 기기와 네트워크 조건에서 사용자가 경험하는 실제 CRP 성능을 정확하게 보여준다.

![dom navtiming]({{ site.baseurl }}/assets/media/critical-rendering-path/dom-navtiming.png)

```html
<html>
  <head>
    <title>Critical Path: Measure</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
    <script>
      function measureCRP() {
        var t = window.performance.timing,
          interactive = t.domInteractive - t.domLoading,
          dcl = t.domContentLoadedEventStart - t.domLoading,
          complete = t.domComplete - t.domLoading;
        var stats = document.createElement('p');
        stats.textContent = 'interactive: ' + interactive + 'ms, ' +
            'dcl: ' + dcl + 'ms, complete: ' + complete + 'ms';
        document.body.appendChild(stats);
      }
    </script>
  </head>
  <body onload="measureCRP()">
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
</html>
```

![device navtiming small]({{ site.baseurl }}/assets/media/critical-rendering-path/device-navtiming-small.png)

* [Sample](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html)

### CRP 분석

주요 렌더링 경로를 최적화하기 위해서는 서로 다른 리소스 간의 의존성 그래프를 파악해야 하며, 어떤 리소스가 **중요**한지 식별해야 하고, 이러한 리소스를 페이지에 포함할 방법에 대한 다양한 전략 중에서 선택해야 한다. 이때 문제를 해결할 수 있는 방법이 한 가지만 있는 것은 아니며, 각 페이지가 서로 다르기 때문에 자신만의 유사한 프로세스에 따라 최적의 전략을 찾아야 한다.

#### 리소스 워터폴(waterfall)

`domContentLoaded`(파란색)은 DOM이 준비되고 JavaScript의 실행을 차단하는 CSS가 없는 시점을 표시하며, Render Tree를 생성할 수 있다. `load`(빨간색)은 페이지에 필요한 모든 리소스가 다운로드되고 처리되는 시점을 표시(즉, 이미지에서 차단됨)하며, 브라우저 로딩 스피너가 회전을 멈춘다.

CSS와 파서 차단 JavaScript가 포함되어 있다면, Render Tree를 빌드하기 위해 DOM과 CSSOM이 모두 필요하다. 또한 파서 차단 JavaScript 파일이 포함되기 때문에, CSS 파일이 다운로드되어 파싱될 때까지 `domContentLoaded` 이벤트가 차단된다.

외부 스크립트의 경우 `async` 키워드를 추가하여 파서의 차단을 해제할 수 있으며, 이 경우에는 CSSOM 생성 또한 동시에 하기 때문에, `domContentLoaded` 이벤트는 HTML이 파싱된 후 바로 실행된다.

CSS와 JS를 모두 페이지 내에 인라인으로 추가하는 경우에는 HTML 페이지가 더 커지지만, 페이지 안에 필요한 모든 요소가 있기 때문에 브라우저가 외부 리소스를 가져올 때까지 기다릴 필요가 없다. 이 경우 외부 스크립트를 비동기로 부르는 것과 비슷한 `domContentLoaded` 시간을 가진다.

##### HTML waterfall

![waterfall dom]({{ site.baseurl }}/assets/media/critical-rendering-path/waterfall-dom.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

##### HTML, CSS and inline JS waterfall

![waterfall dom css js inline]({{ site.baseurl }}/assets/media/critical-rendering-path/waterfall-dom-css-js-inline.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

##### HTML, CSS and extenal and sync JS waterfall

![waterfall dom css js]({{ site.baseurl }}/assets/media/critical-rendering-path/waterfall-dom-css-js.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

##### HTML, CSS and extenal and async JS waterfall

![waterfall dom css js async]({{ site.baseurl }}/assets/media/critical-rendering-path/waterfall-dom-css-js-async.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

##### HTML, inline CSS and inline JS waterfall

![waterfall dom css inline js inline]({{ site.baseurl }}/assets/media/critical-rendering-path/waterfall-dom-css-inline-js-inline.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

#### 성능 패턴

##### HTML performance patterns

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Critical Path: No Style</title>
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
</html>
```

![analysis dom]({{ site.baseurl }}/assets/media/critical-rendering-path/analysis-dom.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

* 1개의 주요 리소스
* 1번 이상의 왕복(최소 주요 경로 길이)
* 5KB의 주요 바이트

##### HTML and CSS performance patterns

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
  </body>
</html>
```

![analysis dom css]({{ site.baseurl }}/assets/media/critical-rendering-path/analysis-dom-css.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

* 2개의 주요 리소스
* 2번 이상의 왕복(최소 주요 경로 길이)
* 9KB의 주요 바이트

##### HTML, CSS and extenal and sync JS performance patterns

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
    <script src="app.js"></script>
  </body>
</html>
```

![analysis dom css js]({{ site.baseurl }}/assets/media/critical-rendering-path/analysis-dom-css-js.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

* 3개의 주요 리소스
* 3번 이상의 왕복(최소 주요 경로 길이)
* 11KB의 주요 바이트

##### HTML, CSS and extenal and async JS performance patterns

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet">
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
    <script src="app.js" async></script>
  </body>
</html>
```

![analysis dom css js async]({{ site.baseurl }}/assets/media/critical-rendering-path/analysis-dom-css-js-async.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

* 2개의 주요 리소스
* 2번 이상의 왕복(최소 주요 경로 길이)
* 9KB의 주요 바이트

##### HTML, CSS with media query and extenal and async JS performance patterns

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <link href="style.css" rel="stylesheet" media="print">
  </head>
  <body>
    <p>Hello <span>web performance</span> students!</p>
    <div><img src="awesome-photo.jpg"></div>
    <script src="app.js" async></script>
  </body>
</html>
```

![analysis dom css nb js async]({{ site.baseurl }}/assets/media/critical-rendering-path/analysis-dom-css-nb-js-async.png)

**[Critical Rendering Path - Analyzing Critical Rendering Path Peformance](ttps://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp/?hl=ko)**

* 1개의 주요 리소스
* 1번 이상의 왕복(최소 주요 경로 길이)
* 5KB의 주요 바이트

### CRP 최적화

#### CRP 최적화 주요 변수

최초 렌더링 시 최대한 빠르게 렌더링하려면 아래 세 가지 변수를 최소화해야 한다.

* 주요 리소스의 수
    * 페이지의 초기 렌더링을 차단할 수 있는 리소스
    * 리소스가 적을수록 브라우저, CPU 및 기타 리소스의 작업이 감소한다.
* 주요 경로 길이
    * 주요 리소스와 해당 바이트 크기 간의 종속성 그래프를 나타내는 기능
    * 일부 리소스 다운로드는 이전 리소스가 처리된 후에만 시작될 수 있으며, 리소스가 클수록 다운로드하는데 걸리는 왕복 수가 증가한다.
* 주요 바이트의 수
    * 브라우저에서 다운로드해야 하는 주요 바이트 수
    * 주요 바이트 수가 적을수록 신속하게 콘텐츠를 처리하여 화면에 렌더링한다.
    * 리소스를 제거하거나 중요하지 않은 것으로 만들어 리소스 수를 줄이면, 바이트 수를 줄일 수 있다.
    * 리소스를 압축하고 최적화하여 전송 크기를 최소화할 수 있다.

#### CRP 최적화 단계

주요 렌더링 경로를 최적화하기 위한 일반적인 단계는 아래와 같다.

1. 주요 경로(리소스 수, 바이트 수, 길이)를 분석하고 파악
1. 주요 리소스를 제거하거나 이에 대한 다운로드를 연기하거나 비동기로 표시하는 등의 방법으로 주요 리소스 수를 최소화
1. 주요 바이트 수를 최적화하여 다운로드 시간(왕복 수)을 단축
1. 나머지 주요 리소스가 로드되는 순서를 최적화(주요 경로 길이를 단축하려면 가능한 한 빨리 모든 주요 자산을 다운로드)

#### CRP 최적화 주의 사항

주요 렌더링 경로를 최적화할 때 주의해야 할 사항은 다음과 같다.

* 렌더링 차단 JavaScript 및 CSS 제거
* JavaScript 사용 최적화
    * 비동기 JavaScript 리소스 선호
    * 동기식 서버 호출 금지
    * JavaScript 파싱 지연
    * 장기적으로 실행되는 JavaScript 피하기
* CSS 사용 최적화
    * CSS를 문서 헤드에 넣기
    * CSS 가져오기(`@import`) 피하기
    * 렌더링 차단 CSS를 인라인 처리

## CRP 최적화 이전의 회사 웹페이지 분석

### 최적화 전 Lighthouse를 사용한 측정

#### PC 측정 결과

* [Lighthouse 분석 링크]({{ site.baseurl }}/assets/pages/critical-rendering-path/ticketlink-pc-crp-origin)에서 상세한 분석 데이터를 볼 수 있다.

##### Performance 측정 결과

![ticketlink pc crp origin performance]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-origin-performance.png)

##### CRP 측정 결과

![ticketlink pc crp origin crp]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-origin-crp.png)

#### Mobile 측정 결과

* [Lighthouse 분석 링크]({{ site.baseurl }}/assets/pages/critical-rendering-path/ticketlink-mobile-crp-origin)에서 상세한 분석 데이터를 볼 수 있다.

##### Performance 측정 결과

![ticketlink mobile crp origin performance]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-origin-performance.png)

##### CRP 측정 결과

![ticketlink mobile crp origin crp]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-origin-crp.png)

### Lighthouse 결과 분석

|  | Critical resources | Critical path length | Critical bytes(KB) | First meaningful paint(ms) | First interactive(ms) | Consistently Interactive(ms) |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| PC | 23 | 23 | 140.5 | 3,090 | 4,510 | 측정 실패 |
| Mobile | 15 | 15 | 281.27 | 3,380 | 5,280 | 11,490 |

* Critical resources : 페이지의 초기 렌더링을 차단할 수 있는 리소스
* Critical path length : 주요 리소스와 해당 바이트 크기 간의 종속성 그래프를 나타내는 기능
* Critical bytes : 브라우저에서 다운로드해야 하는 주요 바이트 수
* First miningful paint : 페이지의 중요 부분이 렌더링 완료될떄
* First interactive : 필수 스크립트들이 로드가 되어, 사용자의 입력을 받을 수 있을때
* Consistently Interactive : 대부분의 네트워크 리소드들이 로드가 완료가 되었을때

### CRP 최적화 포인트 예측

* 주요 리소스가 많이 존재하며, 대부분 렌더링 차단 요소가 JavaScript이므로 차단 요소를 제거
* JavaScript minify와 uglify 적용해서 주요 바이트 최적화
* 렌더링 차단 CSS를 인라인 처리
* 비동기 JavaScript를 `link`태그보다 먼저 호출되도록 이동
* 그외 CRP 최적화는 아니지만, 이미지 최적화 적용

## CRP 최적화 이후의 회사 웹페이지 분석

### 예측 포인트를 통한 CRP 최적화 시도

* 렌더링 차단 JavaScript를 제거
    * 외부 JavaScript의 경우는 가능하면 `async`를 적용하고, 의존 관계가 물려있는 경우는 `defer`을 적용
    * 인라인 JavaScript의 경우는 가능하면 외부 JavaScript로 빼고자 시도하고, 할 수 없다면 `window`의 `load` 이벤트가 발생한 뒤에 수행되도록 수정
* ~~JavaScript minify와 uglify 적용해서 주요 바이트 최적화~~
    * 해당 사항을 적용하기에는 기존 코드의 수정이 많이 필요해서 제외
* ~~렌더링 차단 CSS를 인라인 처리~~
    * 인라인 CSS를 제거하여도 큰 차이가 없고 유지보수를 편의를 위해 복원
* 비동기 JavaScript를 `link`태그보다 먼저 호출되도록 이동
* 이미지 최적화 적용
    * 이미지 적용되는 부분에 맞도록 크기의 이미지를 사용할 수 있게 처리

### 최적화 후 Lighthouse를 사용한 재측정

#### PC 측정 결과

* [Lighthouse 분석 링크]({{ site.baseurl }}/assets/pages/critical-rendering-path/ticketlink-pc-crp-optimization)에서 상세한 분석 데이터를 볼 수 있다.

##### Performance 측정 결과

![ticketlink pc crp optimization performance]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-optimization-performance.png)

##### CRP 측정 결과

![ticketlink pc crp optimization crp]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-optimization-crp.png)

#### Mobile 측정 결과

* [Lighthouse 분석 링크]({{ site.baseurl }}/assets/pages/critical-rendering-path/ticketlink-mobile-crp-optimization)에서 상세한 분석 데이터를 볼 수 있다.

##### Performance 측정 결과

![ticketlink mobile crp optimization performance]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-optimization-performance.png)

##### CRP 측정 결과

![ticketlink mobile crp optimization crp]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-optimization-crp.png)

### 재측정한 Lighthouse 결과 분석

|  | Critical resources | Critical path length | Critical bytes(KB) | First meaningful paint(ms) | First interactive(ms) | Consistently Interactive(ms) |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| PC | 2 | 2 | 15.59 | 1,370 | 5,710 | 19,150 |
| Mobile | 1 | 1 | 24.8 | 1,200 | 5,810 | 5,810 |

## CRP 분석과 최적화 시도를 통해 얻은 결론

### CRP 최적화 전후 비교

|  | Critical resources | Critical path length | Critical bytes | First meaningful paint | First interactive | Consistently Interactive |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| PC | 91.3% 향상 | 91.3% 향상 | 88.9% 향상 | 55.7% 향상 | 26.6% 저하 | 측정불가 |
| Mobile | 93.3% 향상 | 93.3% 향상 | 91.2% 향상 | 64.5% 향상 | 10.0% 저하 | 49.4% 향상 |

First interactive에서 다소 성능이 저하되었지만, 나머지 요소들에서는 많은 성능들이 향상되었다. First interactive 저하의 원인은 렌더링 차단 요소인 JS들을 minify와 uglify 같이 JS 자체에 대한 최적화 없이 렌더링 차단만 해결하여 First meaningful paint 자체는 해결되었지만, 필수 스크립트들이 로드되는 부분 자체가 최적화 된 것이 아니기 때문에 First interactive에서 성능 저하가 일어난 것이 아닐까 추즉하고 있다. 이 부분에 대해서는 좀 더 찾아볼 예정이다.

### 최적화 전후 Snapshot 비교

#### PC(CRP 최적화 전)

![ticketlink pc crp origin snapshot]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-origin-snapshot.png)

#### PC(CRP 최적화 후)

![ticketlink pc crp optimization snapshot]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-pc-crp-optimization-snapshot.png)

#### Mobile(CRP 최적화 전)

![ticketlink mobile crp origin snapshot]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-origin-snapshot.png)

#### Mobile(CRP 최적화 후)

![ticketlink mobile crp optimization snapshot]({{ site.baseurl }}/assets/media/critical-rendering-path/ticketlink-mobile-crp-optimization-snapshot.png)

### 마치며

렌더링 차단 JavaScript를 제거하는 정도의 적은 노력으로 생각하던 것보다 노력 대비 성능 향상을 얻을 수 있는 것 같다. CRP 최적화는 다른 최적화들보다 개발할 때 조금만 더 신경을 써준다면 좋은 결과를 얻을 수 있을 것 같다.

또한 Lighthouse나 Chrome DevTools 등 다양한 측정 도구가 있고, 특히 Lighthouse의 경우 NPM도 지원하니 CI에 적용한다면 도움이 될 것 같다.

## 참고문헌

* [Website Performance Optimization - Udacity](https://www.udacity.com/course/website-performance-optimization--ud884)
* [Critical Rendering Path - Google Developers](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)
* [브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](http://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)
* [브라우저는 어떻게 동작하는가? - D2](http://d2.naver.com/helloworld/59361)
* [Ryan Seddon: So how does the browser actually render a website - JSConf EU 2015](https://youtu.be/SmE4OwHztCc)
* [Rendering in WebKit - Google Developers](https://youtu.be/RVnARGhhs9w)
* [How the Browser Pre-loader Makes Pages Load Faster](https://andydavies.me/blog/2013/10/22/how-the-browser-pre-loader-makes-pages-load-faster/)
* [script의 async와 defer 속성](https://blog.asamaru.net/2017/05/04/script-async-defer/)
* [Speed up Google Maps(and everything else) with async & defer](https://medium.com/@nikjohn/speed-up-google-maps-and-everything-else-with-async-defer-7b9814efb2b)
