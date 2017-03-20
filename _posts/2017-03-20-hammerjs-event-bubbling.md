---
layout: post
title: 'Hammer.js Event Bubbling'
date: 2017-03-20 23:00:00
categories: JavaScript
tags: [JavaScript, Hammer.js, bubbling]
published: true
fullview: false
comments: true
---

## Hammer.js Event Bubbling

[Hammer.js](http://hammerjs.github.io/)의 Event Bubbling을 알아보기 위해서 아래의 코드를 작성하였다. 아래의 코드는 [HTML 이벤트 버블링(Event Bubbling) 에 대해서](http://blog.javarouka.me/2011/12/html-event-bubbling.html)라는 글에서의 예제를 참고하여 작성하였다.

<iframe style="width: 100%; height: 600px" src="http://embed.plnkr.co/MDOET5fqUzIMvaF33uLb" frameborder="0" allowfullscren="allowfullscren"></iframe>


```xml
<!DOCTYPE html>
<html>

  <head>
    <script data-require="hammer.js@*" data-semver="2.0.4" src="//cdn.jsdelivr.net/hammerjs/2.0.4/hammer.js"></script>
    <script src="propagtion.js"></script>
    <script src="script.js"></script>
    <style>
  body { background-color: #FFF; }
  div { margin: 10px; padding: 10px; background-color: red; }
  div div { background-color: yellow; } 
  div div div { background-color: blue; }
  textarea { width: 100%; height: 200px; margin: 0 auto; }
 </style>
  </head>

  <body>
    <div id="depth1">
      <div id="depth2">
        <div id="depth3"></div>
      </div>
    </div>
    <textarea></textarea>
    
    <div id="depth4">
      <div id="depth5">
        <div id="depth6"></div>
      </div>
    </div>
    <textarea></textarea>
    
    <div id="depth7">
      <div id="depth8">
        <div id="depth9"></div>
      </div>
    </div>
    <textarea></textarea>
  </body>

</html>
```

```javascript
window.onload = function(e) {

    // VanillaJS
    var logger = document.getElementsByTagName("textarea")[0];
    function log(newtext) {
        logger.value += newtext + "\n";
        logger.scrollTop = logger.scrollHeight; 
    }
  
    var eventPhaseMap = {
      0: 'NONE',
      1: 'CAPTURE',
      2: 'TARGET',
      3: 'BUBBLE'
    };
  
    var divs = document.getElementsByTagName("div");
    for(var i=0; i < 3; i++) {
        (function(){
            var div = divs[i];
            div.onclick = function(e) {   
                //e.stopPropagation();
                log(eventPhaseMap[e.eventPhase] + " : "+ div.id);                
            };
        })();
    }
    
    // Hammer.js
    var loggerHammer = document.getElementsByTagName("textarea")[1];
    function logHammer(newtext) {
        loggerHammer.value += newtext + "\n";
        loggerHammer.scrollTop = loggerHammer.scrollHeight; 
    }
    
    for(var i=3; i < 6; i++) {
        (function(){
            var div = divs[i];
            
            var mc = new Hammer(div);

            // listen to events...
            mc.on("tap", function(e) {
              logHammer(eventPhaseMap[e.srcEvent.eventPhase] + " : "+ div.id);
              //e.stopPropagation();
              //e.gesture.stopPropagation();
            });
        })();
    }
    
    // Hammer.js with propagtion.js
    var loggerHammerWithPropagation = document.getElementsByTagName("textarea")[2];
    function logHammerWithPropagtion(newtext) {
        loggerHammerWithPropagation.value += newtext + "\n";
        loggerHammerWithPropagation.scrollTop = loggerHammerWithPropagation.scrollHeight; 
    }
    
    for(var i=6; i < 9; i++) {
        (function(){
            var div = divs[i];
            
            var mc = propagating(new Hammer(div));

            // listen to events...
            mc.on("tap", function(e) {
              logHammerWithPropagtion(eventPhaseMap[e.srcEvent.eventPhase] + " : "+ div.id);
              e.stopPropagation();
              //e.gesture.stopPropagation();
            });
        })();
    }
};
```

우선 9개의 `div`를 준비하였다. 3개의 `div`에는 VanillaJS로 `onclick` 이벤트를 바인드하였으며, 그 다음 3개의 `div`에는 Hammer.js로 `tab` 이벤트를 바인드 하였다.

VanillaJS로 바인딩한 `div`를 밖에서부터 클릭해 들어오면 `textarea`에 아래같은 내용이 입력된다.

```
TARGET : depth1
TARGET : depth2
BUBBLE : depth1
TARGET : depth3
BUBBLE : depth2
BUBBLE : depth1
```

클릭한 `Target`부터 시작해 버블링이 일어남을 확인할 수 있으며, 주석을 걸어둔 `stopPropagtion`으로 버블링을 막을 수도 있다.

반면에, Hammer.js로 바인딩한 `div`로 밖에서부터 클릭해 들어오면 `textarea`에 아래같은 내용이 입력된다.

```
BUBBLE : depth4
BUBBLE : depth4
BUBBLE : depth5
BUBBLE : depth4
BUBBLE : depth5
BUBBLE : depth6
```

최상위 `div`부터 클릭한 `Target`까지 버블링이 일어남을 확인할 수 있다. 특히 역순으로 이벤트가 버블링되는 VanillaJS와 다르게 최상위부터 순서대로 버블링이 일어남을 확인할 수 있다. 덕분에 주석을 걸어둔 `stopPropagation`으로는 버블리을 막을 수 없었다. `stopPropagation`을 함수의 최상단에서 호출하면 `stopPropagation` 아래의 코드들은 작동하지 않음을 확인할 수 있었다.

마지막 3개의 `div`에는 Hammer.js에 [propagation.js](https://github.com/josdejong/propagating-hammerjs)라는 라이브러리를 함께 적용하였다. 앞선 두번의 경우와 같이, 밖에서부터 클릭해 들어오면 `textarea`에 아래같은 내용이 입력된다.

```
BUBBLE : depth7
BUBBLE : depth8
BUBBLE : depth9
```

propagation.js를 함께 사용하고 `stopPropagtion`을 적용하여 위와 같이 `stopPropagation`이 잘 작동함을 확인할 수 있었다.