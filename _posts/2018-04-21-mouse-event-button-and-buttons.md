---
layout: post
title: 'Mouse event button & buttons'
date: 2018-04-21 19:00:00
categories: frontend
tags: [frontend, til]
published: true
fullview: false
comments: true
---

```javascript
document.body('mousedown', (e)=>console.log(e.button, e.buttons));
document.body('mousemove', (e)=>console.log(e.button, e.buttons));
document.body('mouseup', (e)=>console.log(e.button, e.buttons));
```

마우스 이벤트에서 이벤트를 일으킨 마우스의 버튼들을 알고자한다면, 위와 같이 마우스 이벤트의 `button`와 `buttons` 두 가지 속성을 사용할 수 있다.(`which`도 있지만 표준이 아니므로 사용하지 않는 것이 좋을 것 같다.) MDN에서는 두 속성에 대해서 아래와 같이 설명하고 있다.

## [`MouseEvent.button`](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/button)

The button number that was pressed when the mouse event was fired.

>`0`: Main button pressed, usually the left button or the un-initialized state
>
>`1`: Auxiliary button pressed, usually the wheel button or the middle button (if present)
>
>`2`: Secondary button pressed, usually the right button
>
>`3`: Fourth button, typically the Browser Back button
>
>`4`: Fifth button, typically the Browser Forward button

`MouseEvent.button` 속성은 하나 또는 여러 개의 단추를 누르거나 놓음으로써 발생하는 마우스 이벤트의 올바른 값만을 보장한다. `mousedown`와 `mouseup`에 대해서는 정상적으로 동작하지만 `mousemove`에서는 누르고 있는 마우스 버튼과 관계없이 `0`을 반환한다.

* [`MouseEvent.buttons`](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/buttons)

The buttons being pressed when the mouse event was fired. Each button that can be pressed is represented by a given number (see below). If more than one button is pressed, the value of the buttons is combined to produce a new number.

>`0`: No button or un-initialized
>
>`1`: Left button
>
>`2`: Right button
>
>`4`: Wheel button or middle button
>
>`8`: 4th button (typically the "Browser Back" button)
>
>`16`: 5th button (typically the "Browser Forward" button)

`MouseEvent.buttons` 속성은 모든 종류의 마우스 이벤트 중에 눌려진 버튼들의 상태를 나타낸다. `buttons`라는 이름에 걸맞게 복수의 마우스 버튼들을 누르고 있으면, 상단의 값들의 합으로 구분할 수 있다. 예를 들어 마우스 왼쪽 버튼(`1`)과 마우스 오른쪽 버튼(`2`)를 함께 누르면 `3`을 값으로 가지고 있음을 확인할 수 있다. 버튼 값들이 2진수의 각 자리값에 해당하므로 눌러진 버튼을 구분하여 사용할 수 있다.

## Reference

* [MouseEvent - MDN](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent)
* [MouseEvent.button](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/button)
* [MouseEvent.buttons](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/buttons)
* [MouseEvent.which - MDN](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/which)