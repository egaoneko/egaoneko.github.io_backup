---
layout: post
title: '[Java 이야기] Window Builder에 Drag&Drop'
date: 2014-02-24 16:06:29
categories: Java
tags: [Java, Window Builder]
published: true
---

![screenshot]({{ site.url }}/assets/media/window-builder7.jpg)

앞서서 Window Builder에 대하여 이야기했었는데 이번에도 또 Window Builder에 대한 이야기를 하고자 해요. 위 이미지는 기본으로 생성된 소스를 Window Builder로 볼 때 모습인데요. 아이콘을 하나 선택해서 오른쪽의 Frame에 가져다 놓으면서 GUI를 만들어 가는데요.

![screenshot]({{ site.url }}/assets/media/window-builder8.jpg)

위 그림과 같이 버튼을 클릭해서 마우스를 가져가게 되면 설정된 Layout 때문에 Visual Studio의 Drag&Drop이 아니라 GUI를 만드는데 어려움이 느껴지더라고요. 사실 아주 간단하게 클릭 한번으로 이 문제를 해결 할 수 있어요.

* * *

![screenshot]({{ site.url }}/assets/media/window-builder9.jpg)

위의 이미지와 같이 1번을 클릭하면 pane에 대한 속성을 2번에 있는 것처럼 확인하실 수 있는데요. 거기서 Layout의 아랫방향 화살표를 누르면 Layout에 대한 여러 속성들이 있는데 그중 우리가 사용할 속성은 GroupLayout이라는 속성이므로 해당 속성을 클릭하시면 되요.

![screenshot]({{ site.url }}/assets/media/window-builder10.jpg)

클릭하시면 위의 이미지와 같이 Drag&Drop으로 GUI를 꾸밀 수 있어요. System항목의 Marquee를 선택하면 여러 개체를 동시에 선택도 가능하고요. 한 가지 아쉬운 점이 이렇게 GroupLayout을 구성하면 각 개체끼리 위치가 연결되어서 가끔 뜻하지 않게 이동되는 경우도 있더라고요. 그럴 경우 되돌리기로 돌려주시면 되고요. 혹시라도 이 부분에 대해 해결책을 알고 계신 분은 댓글 좀 부탁드려요.

