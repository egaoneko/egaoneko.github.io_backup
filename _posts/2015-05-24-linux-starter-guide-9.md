---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 9장. 프로세스'
date: 2015-05-24 21:00:09
categories: os
tags: [os, linux, guide]
published: true
fullview: false
comments: true
---

{% include toc.html %}

프로세스(process)는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램이다. 종종 스케줄링의 대상이 되는 작업(task)이라는 용어와 거의 같은 의미로 쓰인다. 여러 개의 프로세서를 사용하는 것을 멀티프로세싱이라고 하며 같은 시간에 여러 개의 프로그램을 띄우는 시분할 방식을 멀티태스킹이라고 한다. 프로세스 관리는 운영 체제의 중요한 부분으로 자리 잡혀 있다.[^1]

[^1]: 출처 : 위키백과 - 프로세스

즉, 간단히 말해 컴퓨터 안에 실행중인 프로그램을 프로세스라고 하며 실행중이지 않으면 프로그램이라 보면 된다.


## 01 프로세스의 실행방식


### Foreground

~~한 사용자가 하나의 단말기에서 2개 이상의 프로그램을 동시에 수행시킬 때, 그중 단말기를 점유하고 사용자에 의해 대화식으로 사용되는 프로그램. 사용자의 입장에서 보면 맨 앞에 있는 프로그램이므로 전면이라고 하고, 다른 프로그램은 그 프로그램의 뒤에서 보이지 않게 수행되므로 후면이라고 한다.~~[^2]

[^2]: 출처 : IT용어사전, 한국정보통신기술협회


### Background

~~우선순위(priority)가 높은 작업이 먼저 실시되고, 그 실시 도중 남는 시간이 있을 경우에 우선도가 낮은 작업이 실행되는 환경. 즉, 입출력 없이 CPU 계산만 많은 경우 프로그램의 우선순위가 후면으로 밀리게 된다. 일반적으로 배치 프로그램(batch program)은 이 환경에서 실행된다.~~[^3]

[^3]: 출처 : 컴퓨터인터넷IT용어대사전, 2011.1.20, 일진사


~~예를 들면 노래를 틀어놓고 인터넷을 한다고 하면 Foreground는 인터넷이고 Background는 음악 플레이어가 된다.~~

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/포그라운드.jpg)
![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/백그라운드.jpg)

~~좌측이 Foreground이며 sleep명령어가 실행되는 동안은 echo명령어를 수행하지 않았다. 반면에 우측의 Background는 sleep명령 뒤에도 echo명령을 수행하였으며 sleep명령후에는 스스로 종료되었다.~~

### Foreground & Background

페이스북의 김선영님께서 Foreground와 Background에 대해 잘못된 내용임을 알려주어 하단의 내용을 추가한다.

---

>특히 얼마전에도 이야기 했지만 포그라운드, 백그라운드는 정의는 잘못된 내용이 인터넷이나 잘못된 기초 책에 많이 돌아다닙니다.

>IEEE std 표준 정의에 의해 포그라운드, 백그라운드에는 session, controlling terminal의 관계가 정의되어야 합니다. 포그라운드는 세션내에 포그라운드 프로세스 그룹에서 controlling terminal을 소유한 프로세스를 의미하여, 백그라운드는 controlling terminal을 소유하지 못한 프로세스를 의미합니다. 전면에서 작동한다거나 후면에서 작동한다는 이야기는 사실과 많이 다르며 잘못된 내용이 인터넷과 기초책에 무분별하게 돌아다닌 결과입니다.

---

>포그라운드 프로세스란 세션내에서 제어터미널(controlling terminal)을 소유한 상태의 프로세스를 의미합니다. 만일 시험을 친다면 세션과 제어 터미널의 두 단어가 등장하지 않는다면 definition에서 틀린 것으로 간주할 수 있습니다. (이 정의는 IEEE std 1003.1 표준 문서에 등장합니다.) [1]

>백그라운드 프로세스란 세션내에서 제어터미널을 소유하지 못한 상태의 프로세스를 의미합니다. [2]

>[1] [IEEE std. 1003.1](http://www.opengroup.org)의 foreground process 참조

>[2] [IEEE std. 1003.1](http://www.opengroup.org)의 background process 참조

---

## 02 프로세스 명령어


      #ps [옵션] : 현 프로세스 조회 명령어

      옵션:
      	-a : 현재 실행 중인 모든 프로세스 출력
      	-ef : 현재 실행 중인 프로세스를 간단히 출력
      	-aux : 현재 실행 중인 프로세스를 자세히 출력
      	-u : 사용자와 프로세스 시간 출력
      	-x : 히든 프로세스 출력

* * *

      #kill [옵션] PID : 프로세스 종료 명령어

      옵션:
      	-9 : 프로세스 강제 종료 명령어

* * *

      #uname -p : CPU 정보를 알려줌

* * *

      #free [옵션] : 메모리와 가상 메모리의 상태를 보여주는 명령어

      옵션:
      	-b : 사용량을 바이트 단위로 출력
      	-m : 사용량을 메가바이트 단위로 출력
      	-s [숫자] : 입력한 숫자의 초단위로 출력

* * *

      #pstree : 부모, 자식 관계를 나타내는 트리를 출력

* * *

      #top [옵션] : 프로세스별 CPU와 메모리 점유율을 출력

      옵셥:
      	-d [숫자] : 입력한 숫자만큼의 시간마다 다시 출력

* * *

      #nice [-n 조정수치] [옵션] : 프로세스의 우선순위를 변경할 수 있는 NICE 값을 설정

* * *

      #renice [- 조정수치] : 이미 실행되 있는 프로세스의 우선순위를 변경

* * *

      /proc 디렉토리 : 현재 메모리에 존재하는 모든 작업들이 파일형태로 존재

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/proc.jpg)


## 03 데몬

일반적으로 프로그램은 설치 시 하드디스크에 설치되며 동그란 모양의 하드디스크에 깔려있는 프로그램을 읽어오기 위해서 수없이 회전하여 설치되어 있는 섹터를 검색 하고 검색한 부분을 읽어오게 된다. 하지만 하드디스크에서 읽어오는 것은 CPU와 RAM에 비해 상당히 느리다.

그래서 cpu와 하드디스크 사이에 RAM이라는 공간에 실행중인 프로세스를 복사하여 실행시키며 이렇게 하여 속도를 높여 사용하는데 이것을 보고 프로세스를 RAM에 상주시킨다고 한다.

이러한 프로세스는 총 3가지의 종류가 있는데 대화형, 배치형, 데몬형이다. 먼저 대화형은 우리가 흔히 쓰는 프로세스를 말한다. 배치형은 내가 명령을 입력장치로 넣지 않아도 알아서 실행되며 시작 프로그램을 예로 볼 수 있다.

마지막 데몬형은 실행되어도 화면에 출력이 안 되며 Backgound 형태로 돌아가며, 우리가 실행되고 있는지 아닌지를 잘 느끼지 못하는 프로그램이다. 윈도우에서는 서비스 리눅스에서는 데몬이라 불린다.[^4]

[^4]: 출처 : [용식이님의 블로그](http://blog.naver.com/bbaroo27/100183424345)


## 04 데몬의 종류

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/데몬.png)

### Standalone

항상 준비 되어있는 프로세스들을 의미하며 항상 실행 중에 있기 때문에 메모리에 상주 되어 있다. 따라서 응답 속도가 빠르지만 메모리에 부하를 준다. init.d, sendmail, apache, mysql, name server, nfs등이 있다.

### Xinetd


xinetd자체는 항상 준비되어있는 프로세스이며 Standalone 방식인 반면, xinetd의 내부는 xinetd방식이다. xinetd에 요청이 들어오면 xinetd가 자신에게 종속되어 있는 프로그램을 실행시키며 처리가 끝나면 다시 휴면상태로 되돌린다.

xinetd를 부모라고 생각하고, 그 안에 포함되어 있는 프로그램들을 자식이라고 생각한다면 요청 들어오면 부모가 깨워서 밖에 내보내는 것으로 볼 수 있다. 따라서 응답처리속도도 느리지만 요청이 들어오지 않을 경우 휴면상태이므로 메모리를 차지하지 않는다.

xinetd에는 블랙리스트 시스템이 들어 있으며 xinetd가 좋지 못한 요청이라고 간주하면 프로그램들을 밖으로 내보내지 않는다. xinetd.d,telnet,PoP3등이 존재한다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/xinetd.jpg)


## 05 서비스 관리

시스템이 부팅될 때 많은 서비스 데몬들이 실행되는데 그 중 불필요한 데몬도 있다. 이러한 불필요한 데몬들은 시스템 자원을 낭비하므로 필요 없는 서비스는 실행되지 않도록 해주기 위해 ntsysv나 chkconfig명령을 사용하여 설정 할 수 있다.

그리고 현재 실행중인 데몬을 제어하려면 service 명령어나 /etc/rc.d/init.d에 있는 실행스크립트를 이용하여 제어할 수 있다.



      #ntsysv : 커서로 이동하며 스페이스바를 누르면 선택/해제가 가능

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/ntsysv.jpg)

* * *

      #chkconfig [옵션] 데몬 [상태] : 시스템을 부팅할 때 런레벨에 따라 자동으로 실행되는 데몬들을 확인할 수 있고 특정 서비스를 추가, 수정할 수 있음

      옵션:
      	--level : 적용할 런레벨을 선택
      	--add : 데몬을 추가
      	--del : 데몬을 삭제
      	--list : 현재 데몬들의 목록을 출력

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/chkconfig.jpg)

* * *

      #service [데몬] [상태] : 현재 실행중인 데몬을 제어

      옵션:
      	start : 데몬을 실행
      	stop : 데몬을 중지
      	restart : 데몬을 재시작
      	status : 데몬의 상태

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/service.jpg)


## 06 좀비 프로세스와 퇴치 방법

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/좀비.png)

위와 같이 프로세스의 상태를 확인하다보면 좀비 상태인 프로세스들을 확인할 수 있다.

좀비 프로세스는 실행이 완료 되었음에도 불구하고, 소멸되지 않은 프로세스를 말하며 main 함수가 반환되면 소멸한다. 따라서 소멸되지 않았다는 것은 프로세스가 사용한 리소스가 메모리 공간에 여전히 존재한다는 의미이다.

이 좀비 프로세스의 생성은 자식 프로세스가 종료되면서 반환하는 상태 값이 부모 프로세스에게 전달되지 않으면 해당 프로세스는 소멸되지 않고 좀비가 되는 것이 원인이다.

이는 프로그램을 작성할 때 반환하는 상태 값을 받을 수 있도록 wait(), waitpid() 함수를 사용함으로써 해결 할 수 있다.


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

