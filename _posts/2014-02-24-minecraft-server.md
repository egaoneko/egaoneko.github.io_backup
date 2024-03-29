---
layout: post
title: '[마인크래프트] 서버를 여는 세 가지의 방법'
date: 2014-02-24 17:56:54
categories: etc
tags: [minecraft]
published: true
fullview: false
comments: true
---

<figure><img src="/images/etc/minecraft1.jpg" alt=""></figure>

마인크래프트 특유의 게임내의 생존 방식, 시스템 등과 여러 개발자들에 의해 모드와 플러그인 등이 한동안 재미를 주었던 기억에 이 글을 쓰게 되네요. 개인적으로는 Java로 만들어졌으며 Eclipse를 통해서 이런저런 시도가 가능하다는 점에서 좀 더 관심이 갔었기도 하고요.

이번 포스트는 마인크래프트 서버에 대해 이야기하고자 하지만 서버에 대한 여는 법이나 설치법에 대한 것은 아니에요. 서버의 여는 세 가지 방법에 대한 소개와 간단히 난이도를 소개하는 방향으로 포스팅을 하려고 합니다. 사실 여는 법이나 설치법은 간단한 검색만으로도 쉽게 찾을 수 있고 저보다 저 자세히 설명들 해주셔서 여기서는 빼고자 해요.

## 1) 하마치를 통해서 여는 법 (난이도 하)

<figure><img src="/images/etc/minecraft2.jpg" alt=""></figure>

하마치를 통해서 여는 법은 앞으로 소개할 어느 방법보다 쉽게 열 수 있어요. 하마치를 통해서 가상 LAN과 유사한 네트워크를 만드는데 간단한 설치 한번으로 이용이 가능해요. 저도 한번 하마치 서버를 열어본 적이 있긴 한데 개인적으로 선호하는 방법은 아니네요. 일단 블로그나 카페를 통한 설치 파일은 믿을 수가 없고 공식 홈페이지 비슷한 곳이 있긴 하지만 여기가 공식 홈페이지가 맞는지 잘 모르겠더라고요. 그래도 조금 더 신용이 가는 후자 쪽을 선택해서 사용해봤는데 보안 부분이 불안한 것 빼고는 편하게 사용했어요.

요약하자면

* 보안이 약간 위험하지만 그래도 난 편한 것이 좋아!
* 도메인 서버, 노하마치 서버 어렵고 골치 아파!

위와 같은 분이시라면 하마치 서버를 추천할게요.

* 하마치 다운로드 : [https://secure.logmein.com/KR/products/hamachi/download.aspx ](https://secure.logmein.com/KR/products/hamachi/download.aspx)/ <http://hamachi.downloader.co.kr/Hamachi_home>
* 서버 여는 법 : [http://blog.naver.com/chtwon1263?Redirect=Log&logNo=150165710919](http://blog.naver.com/chtwon1263?Redirect=Log&logNo=150165710919)

하마치 다운로드는 공식 홈페이지에서 받는 것을 추천하는데 사실 어느 것이 공식 홈페이지 인지 잘 모르겠네요. 저는 두 번째 링크에서 받아서 사용했지만 첫 번째 링크가 공식 홈페이지라는 말이 있네요. 지금 확인해보니 첫 번째 링크가 좀 더 신용이 가기는 합니다. 홈페이지 내부를 좀 돌아다녀 보고 제품도 보고 연락처나 커뮤니티 메뉴 다국어 등 전반적으로 두 번째보다는 신용이 가네요. 두 번째는 다운로드와 설명만 있고 우측 상단에서 다른 나라 언어로 바꾸어 보았지만 가입과 로그인 부분만 해당 언어로 바뀌지 제품 설명에 대한 부분은 변화가 없네요.


## 2) 포트포워딩을 통해서 여는 법 (난이도 중)

<figure><img src="/images/ect/minecraft3.jpg" alt=""></figure>

이 방법은 하마치보다는 조금 더 손이 가는 작업이 필요한 방법입니다. 그렇다고 엄청 어렵지는 않는데 포트 포워딩이라든가 낯선 용어가 많이 나와 당황 하실 수 있으나 차근차근 하시면 못할 정도는 아니에요. 게다가 도메인과 연결에 연결해 쓰는 방법도 있어서 인터넷에 노마하치 서버라고 찾으면 이 방법에 대한 많은 글을 찾을 수 있어요. 보안 부분은 하마치처럼 설치해야하는 방법은 아니다보니 좀 더 나은 것 같네요.

요약하자면

* 하마치 설치는 불안해, 조금 어려워도 할 수 있어!
* 도메인을 사용하고 싶어!

위와 같은 분이시라면 포트포워딩을 추천할게요.

* 포트포워딩으로 하는 법 : <http://fogria.tistory.com/293>
* 도메인 연결 하는 법 : <http://blog.naver.com/dlflfh07/110182881744>


## 3) 가상 서버 호스팅(혹은 서버호스팅)을 통해서 여는 법 (난이도 상)

<figure><img src="/images/etc/minecraft4.jpg" alt=""></figure>

위의 두 방법 중 제일 어려운 편에 속하는 방법이네요. 저는 가상 서버 호스팅 서비스 중 리눅스 서버를 구매해 사용하고 있어 이 방법으로 서버를 열어봤네요. 가상 서버 호스팅 서비스 중 윈도우 서버도 있는데 이를 통해하면 리눅스보다 구현에 좀 더 쉬울 수 있어요. 사실 가상 서버 호스팅이나 서버 호스팅이나 구현 방법이 동일한데 가상 호스팅은 가격이 싸지만 성능이 딸리고 서버 호스팅은 성능 면에서 좋지만 가격이 비싸네요. 윈도우보다 리눅스 서버가 가격이 싸고 성능이 좋게 판매하고 있고요. 마인크래프트 서버를 만들기 위해 서버 호스팅을 하는 것은 개인적으로 봤을 때는 비싼 느낌이 있는데 가상 서버 호스팅으로 서버를 만들어봤는데 서버로 운영하기엔 성능이 너무 나쁘네요. 친구랑 단둘이 하실 것이라면 문제가 없지만 아니라면 포트포워딩 방법이 더 나은 것 같네요.

요약하자면

* 내 컴퓨터가 아니니까 보안 위험이 없다!
* 리눅스로 공부할 것도 있고 개인서버도 하나 가지고 싶은데 마크까지!
* 매달 조금씩 돈이 나가지만 즐기기 위해선 이정도 쯤이야!

위와 같은 분이시라면 가상 서버 호스팅(서버 호스팅)을 추천할게요.

* 가상 서버 호스팅　：<http://kkwaks.net/717>
* 서버 여는 법 : <http://inunmc.tistory.com/2>

## 추가된 내용

* 2015-05-24에 추가된 글입니다.

마인크래프트를 하면 머리가 아퍼서 못한지 오래됬네요. 그리고 티스토리에서 다른 블로그로 옮기고 티스토리를 방치해뒀는데 그곳에서 달린 댓글을 보고 추가글을 작성하게 됬네요.

서버 호스팅 사이트를 하나 추천 받았네요.

[https://netherbox.com](https://netherbox.com)라는 사이트인데 직접 운영해보지 않아서 자세한 것은 모르겠지만 마인크래프트를 전문적으로 서버 호스팅하는 것 같네요.

개인적으로 사용해본 [DigitalOcean](https://www.digitalocean.com)과 비슷한 램에 비슷한 가격이네요. 서버 호스팅이 익숙하시다면 여러 용도로 사용할 수 있는 DigitalOcean같은 서버 호스팅이 좋겠지만 어렵다면 netherbox도 한 방법이 아닐까 하네요.
