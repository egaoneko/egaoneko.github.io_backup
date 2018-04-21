---
layout: post
title: 'Copy clipboard'
date: 2018-01-30 00:40:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

아래와 같이 코드를 작성하면 클립보드로 `input`에 있는 내용을 복사할 수 있다.

```html
<input id="input" type="text"/>
<button id="copy">Copy</button>
```

```javascript
function copy() {
  var copyText = document.querySelector("#input");
  copyText.select();
  document.execCommand("copy");
}

document.querySelector("#copy").addEventListener("click", copy);
```

만약 `input`을 수정하지 못하도록 하고 싶다면 `disabled`를 사용하면 `copyText.select()`가 동작하지 않아서 복사가 작동하지 않는다. 이때 `disabled`대신 `readonly`를 사용하면 해결할 수 있다.

```html
<!-- <input id="input" type="text" disabled="disabled"/> -->
<input id="input" type="text" readonly="readonly"/>
```

* 참고
  * [Document.execCommand() - MDN](https://developer.mozilla.org/ko/docs/Web/API/Document/execCommand)
  * [Interact with the clipboard - MDN](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Interact_with_the_clipboard)
  * [Allow Copy/Paste in a disabled input text box in Firefox browsers - stack overflow](https://stackoverflow.com/questions/8876928/allow-copy-paste-in-a-disabled-input-text-box-in-firefox-browsers)