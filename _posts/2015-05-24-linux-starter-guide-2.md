---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 2장. 명령어'
date: 2015-05-24 21:00:02
categories: OS
tags: [OS, Linux, guide]
published: true
fullview: false
comments: true
---

## 01 명령어란?

> 명령어는 쉘을 통하여 자원을 제어 할 수 있는 언어 체계를 뜻한다.[^1]
>
>  * 전산학에서 명령어(command)는 셸 환경 등에서 프로그램 실행을 위해 일반 사용자가 입력하는 언어를 뜻한다.
>  * 전산학에서 명령어(instruction)는 컴퓨터 아키텍처 내부에서 중앙 처리 장치의 단일 동작(연산)을 뜻한다.
>  * 프로그래밍에서 명령어는 명령문(statement) 또는 함수(function)을 뜻한다.

[^1]: 출처 : 위키백과 - 명령어

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/명령어.jpg)

명령어는 위의 그림에서와 같이 사용자 영역에서 해당 명령어를 입력하면 그림의 번호순으로 해당 명령어가 있는지 확인한다. 해당 명령어가 있으면 해당 명령어를 수행하고 해당 명령어가 없으면 사용자에게 명령어를 찾을 수 없음을 알린다.


## 02 자주 사용하는 기본 명령어

리눅스에는 많은 명령어가 있고 처음부터 모든 명령어를 다 알기는 힘들다. 아래 명령어는 필자가 실습을 하며 많이 사용하고 기본적으로 알아두면 좋다고 생각한 명령어들을 모아보았다. 아래의 명령어들의 설명이나 옵션은 검색이나 man 명령어를 통해서 확인 할 수 있다.

|  |  |  |  |
|:--:|:--:|:--:|:--:|
|  ls  |  whoami  |  pwd  |  cp  |
|  rm  |  rmdir  |  mkdir  |  mv  |
|  cd  |  chmod  |  du  |  df  |
|  passwd  |  echo  |  vi  |  man  |
|  touch  |  cat  |  head  |  tail  |
|  more  |  less  |  clear  |  rpm  |
|  yum  |  tar  |  gzip  |  gunzip  |
|  find  |  which  |  whereis  |  finger  |
|  who  |  ps  |  kill  |  pstree  |
|  shutdown  |  init  |  grep  |  wc  |
|  nslookup  |  w  |  last  |  write  |
|  wall  |  in  |  |  |


## 03 네트워크 정보 확인 명령어

리눅스에서 IP의 확인과 네트워크가 접속되었는지 확인 등 네트워크 정보를 확인할 때 쓰는 명령어는 아래와 같다.

| 명령 | 설명 |
|:---:|:---:|
| ifconfig | 컴퓨터의 IP주소 정보, 네트워크 인터페이스 구성 |
| ping | 네트워킹 상태를 확인 (생존유무) |
| route –n | 라우터 연결 정보 |
| traceroute	| 목적지에 도착하기 위한 방문 순서 |
| arp | IP 주소를 하드웨어 주소로 사상 검사와 관리 |
| netstat | 네트워크 상태 알아보기 |

네트워크에 연결하기 위해 설정해야 하는 파일들의 이름은 아래와 같다.

#### IP 주소, Network 주소 Broadcast 주소 : /etc/sysconfig/network-script/ifcfg-eth[숫자]

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/명령어1.png)

#### Gateway 주소 : /etc/sysconfig/network

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/명령어2.png)

#### DNS 주소 : /etc/resolv.conf

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/명령어3.png)

#### 네트워크 재시작 : /etc/init.d/network restart

* * *

## 관련글

### 초보자를 위한 가이드

* [[리눅스 정리 - 초보자를 위한 가이드] 들어가며,]({% post_url 2015-05-24-linux-starter-guide-0 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 1장. 실습환경 구축]({% post_url 2015-05-24-linux-starter-guide-1 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 2장. 명령어]({% post_url 2015-05-24-linux-starter-guide-2 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 3장. 디렉토리]({% post_url 2015-05-24-linux-starter-guide-3 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 4장. 파일 시스템]({% post_url 2015-05-24-linux-starter-guide-4 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 5장. 표준 스트림]({% post_url 2015-05-24-linux-starter-guide-5 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 6장. Vi]({% post_url 2015-05-24-linux-starter-guide-6 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 7장. 사용자 관리]({% post_url 2015-05-24-linux-starter-guide-7 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 8장. 쉘 스크립트]({% post_url 2015-05-24-linux-starter-guide-8 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 9장. 프로세스]({% post_url 2015-05-24-linux-starter-guide-9 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 10장. 소프트웨어 설치]({% post_url 2015-05-24-linux-starter-guide-10 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 마치며,]({% post_url 2015-05-24-linux-starter-guide-11 %})
* [[리눅스 정리 - 초보자를 위한 가이드] 추가. 퍼미션]({% post_url 2015-05-24-linux-starter-guide-12 %})

