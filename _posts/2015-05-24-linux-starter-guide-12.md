---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 추가. 퍼미션'
date: 2015-05-24 21:00:12
categories: OS
tags: [OS, Linux, guide]
published: true
fullview: false
comments: true
---

## 01 퍼미션

파일 혹은 디렉토리에 접근할 수 있는 권한을 퍼미션이라 하는데 이 권한을 통해 사용자는 파일이나 디렉토리에 대하여 다른 사용자의 접근을 제어할 수 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/1.jpg)

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/2.jpg)

상단의 그림과 같이 읽기, 쓰기, 실행에 권한을 사용자, 사용자와 같은 그룹 그리고 그 외의 사용자에게 부여할 수 있다. 또한 각 권한은 숫자로 표현할 수 있으며 읽기는 4, 쓰기는 2 그리고 실행은 1로 표현하고 합산한다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/3.jpg)

이 그림을 보면 루트(/) 디렉토리 밑의 디렉토리들이 각 각의 역할에 맞게 권한이 설정 되어있는 것을 볼 수 있다. 특히 bin과 같이 사용자가 이용은 해야 가능해야 하지만 변경하면 안 되는 디렉토리는 쓰기 권한이 제한되어 있는 것을 확인할 수 있다.


## 02 권한 변경 명령어

      #chmod [옵션] [모드] [파일명] : 파일의 접근권한을 변경한다.

      옵션:
      	-R --recursive : 하위 디렉토리에 있는 모든 디렉토리/파일을 변경
      	-v --verbose : 각 파일 정보를 상세히 출력

      	ex) chmod 754 test.txt
          	chmod u=rwx,g=rw,o=x test.txt


## 03 SetUID, SetGID, Sicky Bit

### SetUID : 해당 파일의 소유자의 실행권한이 없더라도 소유자의 실행권한으로 파일을 실행

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/4.jpg)

소유자의 퍼미션에 s가 붙어있으면 소유권이 없더라도 실행할 수 있으며, “chmod 4744 test.sh” 같은 식으로 허가권을 주게 되면 test.sh를 소유자가 아닌 경우에도 소유자의 권한으로 실행 할 수 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/5.jpg)

소유자의 실행권한으로 실행하였지만 프로세스 실행 UID가 root가 아닌 실행한 유저의 것을 확인할 수 있다.

* * *

### SetGID : SetUID와 유사하며, 그룹사용자로서의 실행권한을 얻을 수 있음 

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/6.jpg)

* * *

### sticky bit : 이 비트가 설정되면 파일을 만든 사용자만이 그 파일에 대한 권리를 가짐

{{ programming:os:linux:리눅스_정리:7.jpg }}![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/7.jpg)

대표적인 디렉토리로는 “/tmp” 디렉토리가 sticky bit로 설정되어 있는 것을 볼 수 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/8.jpg)

위의 그림은 root 계정으로 루트(/) 디렉토리 아래에 “test”라는 디렉토리를 만들고 sticky bit와 모든 사용자에게 읽기, 쓰기, 실행 권한을 주었다. 그리고는 smilecat이라는 계정으로 “xx”라는 파일을 만들고 이 파일 또한 모든 사용자에게 읽기, 쓰기, 실행 권한을 주었다. 그리고 test1이라는 계정으로 “rm명령어”를 이용하여 파일을 삭제하려고 하면 위 그림과 같이 허용되지 않음을 알 수 있다. 비록 공동으로 사용하는 디렉토리지만 다른 사용자가 내가 만든 파일을 함부로 삭제할 수 없는 것이다.


## 04 umask

리눅스에서 파일이나 디렉토리를 생성할 때, 초기에 주어지는 퍼미션을 설정할 때 사용하는 명령어다. 기본 설정은 root계정은 “0022”로 일반 사용자 계정은 “0002”로 되어있다.


### 기본 퍼미션

* 디렉토리 : 777
* 파일 : 666

* * *

### 명령어

      #umask [옵션] [umask 값]

      옵션:
      	-S : 현재 umask를 u,g,o와 r,w,x로 표현

* * *

### 연산 방법

* umask 값에 보수를 취한다.
* 보수를 취한 값과 기본 퍼미션을 AND 연산을 수행한다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/9.jpg)

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/10.jpg)


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

