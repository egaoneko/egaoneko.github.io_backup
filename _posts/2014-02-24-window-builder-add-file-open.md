---
layout: post
title: '[Java 이야기] 추가한 파일 Window Builder로 열기'
date: 2014-02-24 11:43:24
categories: Java
tags: [Java, Window Builder]
published: true
fullview: false
comments: true
---

![screenshot]({{ site.url }}/assets/media/window-builder1.jpg)

Eclipse로 Java GUI를 만들어야 할 때 Visual Studio 시리즈처럼 쉽게 GUI를 만들게 도와주는 툴이 없을까 찾아다녔던 적이 있었어요. 그 끝에 찾은 방법이 Eclipse에 Window Builder을 설치하여 사용하는 방법이었는데 이 Window Builder를 이용하면 Visual Studio만큼 편하게 GUI를 만들 수 있어요.

![screenshot]({{ site.url }}/assets/media/window-builder2.jpg)

![screenshot]({{ site.url }}/assets/media/window-builder3.jpg)

이 포스트에서 보고자하는 것은 추가한 파일을 Window Builder로 여는 것에 대한 것이에요. 위와 같이 다른 곳에서 만들어 파일로 추가하거나 가져오기로 넣은 파일이 Window Builder로 보기위한 버튼이 보이지 않는데요.

저는 처음에 이걸 해결하기 위해서 프로젝트를 만들고, 다시 동일한 이름의 파일을 만들어 거기에 기존 소스를 붙여 넣는 등 번거롭고 필요 없는 방법을 사용했어요. 프로젝트 규모가 커지면 커질수록 이 방법은 사용하기가 힘들고 낭비되는 방법이라 여기저기 찾아다닌 끝에 쉽고 간단한 방법을 찾았어요.

![screenshot]({{ site.url }}/assets/media/window-builder4.jpg)

우선 이렇게 분명 GUI로 만들어졌는데 Window Builder로 Design버튼이 없는 파일의 .java소스 파일을 오른쪽으로 클릭해주세요.

![screenshot]({{ site.url }}/assets/media/window-builder5.jpg)

오른쪽으로 클릭하면 나오는 메뉴 중 연결 프로그램에 마우스를 두면 위의 그림과 같이 추가 메뉴를 볼 수 있는데요. 잘 보시면 현재 문서가 Java 편집기로 열려있는 것을 알 수 있는데 여기서 Window Builder Editor를 클릭해주시면 되요.

![screenshot]({{ site.url }}/assets/media/window-builder6.jpg)

클릭하고나면 이렇게 Source버튼과 Design버튼이 나타나며 Window Builder을 사용할 수 있어요. 이런 간단한 방법만 알아두면 쉽게 프로젝트를 수행할 수 있고 같이 프로젝트 하는 사람들의 파일을 받아도 부담 없이 열어볼 수 있어요.



