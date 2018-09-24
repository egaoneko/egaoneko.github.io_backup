---
layout: post
title: '팝업 차단 감지'
date: 2018-09-24 21:15:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

개발중인 사이트에서 새창을 띄우는데 새창을 오픈 중에 팝업이 차단되어 뜨지 못하는 경우가 있었다. 문제는 팝업이 차단되어 노출되지 않는 것보다, 사용자가 스스로가 차단하지 않고 브라우저가 기본적으로 팝업을 차단하는 경우에 팝업이 열리지 않아 오류로 인해 팝업이 뜨지 않는 것으로 보이는 부분이었다.

이를 해결하기 위해 stackoverflow를 찾아보니, 여러 방법중에 아래의 방법이 가장 적합하다고 생각하여 사용하였다.

```javascript
function hasPopupBlocker(poppedWindow) {
    var result = false;

    try {
        if (typeof poppedWindow == 'undefined') {
            // Safari with popup blocker... leaves the popup window handle undefined
            result = true;
        }
        else if (poppedWindow && poppedWindow.closed) {
            // This happens if the user opens and closes the client window...
            // Confusing because the handle is still available, but it's in a "closed" state.
            // We're not saying that the window is not being blocked, we're just saying
            // that the window has been closed before the test could be run.
            result = false;
        }
        else if (poppedWindow && poppedWindow.test) {
            // This is the actual test. The client window should be fine.
            result = false;
        }
        else {
            // Else we'll assume the window is not OK
            result = true;
        }

    } catch (err) {
        //if (console) {
        //    console.warn("Could not access popup window", err);
        //}
    }
    return result;
}
```

* 참고
  * [Copy imageData by value in JavaScript - stack overflow](https://stackoverflow.com/questions/5642383/copy-imagedata-by-value-in-javascript?rq=1)
  * [Detect blocked popup in Chrome - stackoverflow](https://stackoverflow.com/questions/668286/detect-blocked-popup-in-chrome/1089792#1089792)