---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 5장. 표준 스트림'
date: 2015-05-24 21:00:05
categories: OS
tags: [OS, Linux, guide]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## 01 표준 스트림의 종류

리눅스에서의 기본 입출력 스트림(I/O stream)에는 표준 입력, 표준 출력, 표준 에러 가 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/표준스트림.png)

  * 표준 입력( 0 ) => 키보드
  * 표준 출력( 1 ) => 모니터
  * 표준 에러( 2 ) => 모니터

## 02 표준 스트림의 기호와 사용 예

|  기호  |  기능  |  사용 예  |
|:-----:|:----:|:--------:|
|  >  |  쓰기  |  #cat /etc/passwd > test.txt  |
|  <  |  읽기  |  #cat < test.txt > test.txt  |
|  <nowiki>>></nowiki>  |  추가  |  #cat < test.txt >> test.txt  |
|  2>  |  에러의 방향을 바꾼다.  |  #cat test.txt > a.txt 2> b.txt  |
|  1>&2  |  출력을 에러로 내보낸다.  |  #cat test.txt >&2 2> b.txt  |
|  2>&1  |  에러를 출력으로 내보낸다.  |  #cat test.txt >a.txt 2>&1  |
|  ex) find / -name ‘*.c’ > result.txt 2> error.txt |

	(">" 기호는 "1>" 와 같다. 즉 1이 생략되어 있다.)


## 03 파이프라인의 정의와 사용 예

>명령어 파이프라인(instruction pipeline)은 명령어를 읽어 순차적으로 실행하는 프로세서에 적용되는 기술로, 한 번에 하나의 명령어만 실행하는 것이 아니라 하나의 명령어가 실행되는 도중에 다른 명령어 실행을 시작하는 식으로 동시에 여러 개의 명령어를 실행하는 기법이다.
>
>하나의 명령어는 여러 개의 단계로 나눌 수 있는데, 이때 하나의 명령어를 처리할 때까지 다음 명령어가 처리되지 않고 기다린다면, 명령어의 특정 단계를 처리하는 동안 다른 단계를 처리하는 부분은 아무 작업도 하지 않게 된다. 이때 파이프라인을 사용하면 한 명령어의 특정 단계를 처리하는 동안 다른 부분에서는 다른 명령어의 다른 단계를 처리할 수가 있게 되므로 속도가 향상될 수 있다.[^1]

[^1]: 출처 : 위키백과 – 파이프 라인

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/표준스트림2.png)

파이프의 예를 보면 아래와 같다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/파이프.jpg)


## 04 명령어의 수행

리눅스에는 아래와 같은 조건 명령열이 있다.

* &&	: 명령어가 실패하면 수행을 멈추고 더 이상 진행하지 않음
* ||	: 명령어가 성공하면 수행하고 중지
* ;	: 실패와 성공에 관련 없이 수행


이를 C언어를 통해 풀어보고자 하면 아래와 같은 조건문이 있다고 하자.


	if ( 3<1 && 2<1 ) Vs. if ( 3<1 && 2>1 )

위의 조건문을 보게 되면 조건문 안의 조건 중 앞의 조건이 참이 되어야 뒤의 조건을 확인하게 된다. &&명령열도 이와 비슷하게 앞의 명령어가 실패하면 수행을 멈추고 앞의 명령어가 성공한다면 뒤의 명령어도 수행하게 된다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열1.jpg)
![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열2.jpg)

      if ( 3>1 || 2<1 ) Vs. if ( 3>1 || 2>1 )

앞에서와 반대로 위와 같은 조건문을 보게 되면 조건문 안의 조건 중 앞의 조건만 만족하게 되면 뒤의 조건을 확인하지 않아도 된다. ||명령열 또한 이와 비슷하게 앞의 명령어가 성공하면 수행을 중지하며 앞의 명령어가 실패하며 뒤의 명령어를 수행하게 된다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열3.jpg)
![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열4.jpg)


>**위의 명령열을 실제 사용하는 예는 다음과 같다.**
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열5.jpg)
>
>위와 같이 정상적으로 작성한 파일을 하나 준비했다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열6.jpg)
>
>정상적으로 작성한 파일은 컴파일을 성공적으로 수행하였고 따라서 명령열 뒤의 명령은 실행되지 않았다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열7.jpg)
>
>위와 같이 헤더파일에 의도적으로 손상을 주었다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/조건열8.jpg)
>
>손상을 입은 파일을 컴파일하게 되면 오류 메시지와 함께 명령어 수행에 실패하고 명령열 뒤의 명령인 echo 명령이 실행되는 것을 확인할 수 있다.


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

