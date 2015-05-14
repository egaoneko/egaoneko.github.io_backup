---
layout: post
title: '[오류] avrdude: stk500_getsync(): not in sync: resp=0x00'
date: 2014-03-09 13:08:10
categories: Arduino
tags: [아두이노, 오류]
published: true
---

![screenshot]({{ site.baseurl }}/assets/media/arduino-error-stk5001.PNG)

아두이노를 구매하고 처음 시작할 때, stk500_getstnc()라는 오류가 하루 동안 저를 괴롭혔는데요. 카페를 다 뒤져도 해결책이 나오지를 않고, stack overflow에서도 찾지 못했거든요. 우연히 이것 저것 만지다가 해결하게 되어 혹시 같은 어려움을 격는 분들이 있을까 올립니다. 참고로, 저는 Mega 2560보드로 구매했어요.

![screenshot]({{ site.baseurl }}/assets/media/arduino-error-stk5002.PNG)

첫번째로 도구에 보드 메뉴에 현재 사용하는 보드와 같은 보드로 설정되어있는지 확인해주세요. 저는 이때 Mega2560이 아닌 Uno로 잡혀있어서 제 보드에 맞는 Mega2560으로 설정했어요.

![screenshot]({{ site.baseurl }}/assets/media/arduino-error-stk5003.png)

두번째로 프로그래머 부분인데요. 이 부분에서 AVR ISP로 잡혀있었는데 이걸 AVRISP mk2로 바꾸었어요. 사실 이 부분이 아두이노에서 어떤 역활을 하는지 잘 모르겠지만 바꿔보니 해당 오류와 파생되었던 오류들도 해결되었네요.



