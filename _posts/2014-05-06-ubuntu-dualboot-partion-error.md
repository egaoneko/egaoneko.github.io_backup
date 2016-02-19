---
layout: post
title: '[Ubuntu] 듀얼부팅 구성시 Ubuntu에서 Windows파티션을 인식하지 못할 때'
date: 2014-05-06 10:30:58
categories: Tips
tags: [OS, ubuntu]
published: true
---

![screenshot]({{ site.url }}/assets/media/ubuntu1.PNG)

Ubuntu의 매력에 요즘 푹빠졌네요. 혹시 간단하게 우분투 체험해보시고 싶으신분이 있다면 아래 링크에 접속해 보시면 저와 같이 그 매력에 빠지실거에요.

[Ubuntu Tour](http://www.ubuntu.com/tour/en/#)

Ubuntu를 듀얼부팅을 구성하려고 Windows에서 파티션까지 나눴는데 그 파티션을 찾지 못했을 때 당황했었던 기억이 나서 이번 포스팅을 준비했어요.

저는 찾아 돌아다닌 수고해비해 정말 간단한 설치와 명령어로 쉽게 해결할 수 있었는데요. 해결방법은 아래와 같아요.

{% highlight bash %}

sudo fdisk -l
sudo apt-get install gdisk
sudo fixparts /dev/sda
and then press the key 'w'

{% endhighlight %}
