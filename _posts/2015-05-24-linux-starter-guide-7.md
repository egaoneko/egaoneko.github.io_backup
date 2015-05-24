---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 7장. 사용자 관리'
date: 2015-05-24 21:00:07
categories: OS
tags: [OS, Linux, guide]
published: true
---

사용자 관리를 통해 여러 유저에게 다양한 접근 권한을 부여할 수 있다. 유저는 고유 ID 1개와 기본그룹 ID 1개 그리고 보조그룹 ID 여러 개로 구성된다. UID, GID는 숫자로 구성되고 식별자 역할을 수행한다.


## 01 파일

* * *

### /etc/passwd	: 사용자 정보가 담겨있는 파일

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/사용자관리1.png)

ID:password:UID:GID:Comment:home directory:shell

* * *

### /etc/shadow	: 사용자 비밀번호가 담겨있는 파일

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/사용자관리2.png)

ID:패스워드:암호생성일자:암호변경가능최소시간:초기암호변경 없이 사용기간:만료 지정시 만료 경고 일 수
(!! : 일반사용자들, * : 시스템 사용자들(데몬 돌리기 위한))

* * *

### /etc/group	: 사용자 그룹 정보가 담겨 있는 파일

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/사용자관리3.png)

Group name:password(일반적으로 쓰이지 않음):GID:Group List


## 02 명령어

### 사용자 관리

      #useradd [옵션] 사용자 ID : 사용자 추가

      옵션:
          -u : UID
          -g : GID
          -c : comment



      #userdel [옵션] 사용자 ID : 사용자 삭제

      옵션:
          -r : 홈 디렉토리까지 삭제



      #usermod [옵션] 사용자 ID : 사용자 설정 변경

      옵션:
      	-u : UID
      	-g : GID
      	-c : comment

* * *

### 그룹 관리


      #groupadd [옵션] 그룹 ID : 그룹추가

      옵션:
          -g : GID


      #groupdel [옵션] 그룹 ID : 그룹 삭제


      #groupmod [옵션] 그룹 ID : 그룹 설정 변경


* * *

### 비밀번호 변경


      #passwd [옵션] 사용자 ID : 사용자의 비밀번호 변경 (SETUID 설정이 걸려 있음)

      옵션:
      	-d : 패스워드가 null로 지정
      	-l : 패스워드 잠금 (로그인 금지)
      	-u : 패스워드 잠금 해제 (로그인 활성)
      	--stdin : 비밀번호 재입력 없이 변경 가능 (일괄적으로 처리 시 사용)
                사용 예) #echo ‘123##’ | passwd —stdin abc

* * *

### 사용자 전환

      #su [옵션] 사용자 ID : 사용자 전환 (사용자의 정보만 변경)

      옵션:
      	- : 사용자의 환경변수까지 변경



## 03 SKELL 파일

SKELL 파일은 사용자가 생성될 때 함께 만들어지는 파일이다. 이 파일은 /etc/skel 디렉토리에 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/skel1.png)
![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/skel2.png)

/etc/skel 디렉토리에 다음과 같이 파일이 있는 것이 보이며 그 파일들이 abc라는 계정의 홈 디렉토리에 생성됨을 알 수 있다.


## 04 환경 설정 파일

### BASH 쉘 환경설정 파일

      ~/.bash_profile : 로그인시 설정 내용을 읽어 들임
      ~/.bashrc : 쉘 실행 시 내용을 읽어 들임
      ~/.bash_logout : 로그아웃 시 읽어 들임

* * *

### 본 쉘 환경설정 파일

      ~/.profie

* * *

### C 쉘 환경설정 파일

      ~/.login
      ~/.cshrc

* * *

### BASH 쉘 환경설정 파일을 읽어 들이는 순서

      /etc/profile -> ~/.bash_profile -> ~/.bashrc -> /etc/bashrc


* * *

### PATH (환경 변수)

* PATH 변수 확인 : #echo $PATH
* 모든 사용자 PATH 설정 : /etc/profile 편집
* 특정 사용자 PATH 설정 : ~/.bash_profile
* 현재 로그인 상태에서만 사용되도록 설정 : #PATH=$PATH: 추가할 디렉토리 설정
* alias 설정 : ~/.bashrc 편집
* 사용자별 프롬프트 변경 : 절대경로 표시 : /etc/bashrc 편집

>**PS1="\u |\w| \$"**
>
>
>| 제어문자 | 설명 |
>|:----:|:-----:|
>|  \t  |  현재 시간(시:분:초 형식)  |
>|  \d  |  오늘 날짜, 요일(요일 월 일 형식)  |
>|  \n  |  new line  |
>|  \s  |  현재 셸 이름, $0의 값  |
>|  \w  |  현재 작업 디렉토리의 이름 (전체경로까지)  |
>|  \W  |  현재 작업 디렉토리의 이름 (현재 위치만)  |
>|  \u  |  사용자 이름  |
>|  \h  |  호스트 이름  |
>|  \#  |  셸이 기동한 후로부터의 명령의 번호  |
>|  \!  |  실행하는 명령의 history 번호  |
>|  \$  |  프롬프트 모양 uid가 0, root이면 #, 그렇지 않으면 $  |
>|  \nnn  |  ASCII 코드 nnn에 해당하는 문자  |
>|  \\  |  백슬래시  |
>|  \[  |  제어문자라 쓸 수 없는 문자를 사용하기 위해서는 [로 시작  |
>|  \]  |  제어문자의 끝  |

* 로그아웃시의 실행 : ~/.bash_logout 편집

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

