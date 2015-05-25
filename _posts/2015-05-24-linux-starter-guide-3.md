---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 3장. 디렉토리'
date: 2015-05-24 21:00:03
categories: OS
tags: [OS, Linux, guide]
published: true
---

## 01 디렉토리 구조

리눅스의 디렉토리는 트리 형태의 구조를 보이며 최상위 디렉토리는 "/" (Root File System) 이다. tree <경로> -L <보고자 하는 단계> 이라는 명령어를 통해 디렉토리의 트리 구조를 확인 할 수 있다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/디렉토리.jpg)


## 02 주요 디렉토리

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/디렉토리2.png)

|  이름  |  설명  |
|:-----:|:-----:|
|  /bin  | 리눅스 기본 명령어 |
|  /sbin  | ifconfig, e2fsck, ethtool, halt 등과 같이 주로 시스템 관리자들이 사용하는 시스템관리자용 명령어를 저장하고 있는 디렉토리. |
|  /etc  | 환경설정 파일 |
|  /user  | 명령어 , 시스템 프로그램 , 라이브러리 루틴 |
|  /boot  | 리눅스 커널 이미지 부트로더 |
|  /dev  | 디바이스 파일 , 용령없는 파일로 구성된 가상 디렉토리로 존재함. |
|  /home  | 사용자 홈 디렉토리 . 일반 사용자 계정을 만들 때 기본설정에 의해 홈 디렉토리를 생성하는 디렉토리. |
|  /root  | 관리자 홈 디렉토리(보안상 따로 분류되어 있음) |
|  /lib  | 시스템 라이브러리가 위치하는 디렉토리 |
|  /mnt  | 플로피 디스크 , 씨디롬 등의 장치를 인식시키기 위한 디렉토리 |
|  /media  | /mnt 대체 훼손된 파일 장소 |
|  /lost+found  | 훼손된 파일 장소 |
|  /proc  | 일명 "가상파일시스템" 이라고 하는 곳으로 현재 메모리에 존재하는 모든 작업들이 파일형태로 존재하는 곳이다. 디스크상에 실제 존재하는 것이 아니라 메모리상에 존재하기 때문에 가상파일시스템이라고 부른다. 실제 운용상태를 정확하게 파악할 수 있는 중요한 정보를 제공하며 여기에 존재하는 파일들 가운데 현재 실행중인 커널(kernel)의 옵션 값을 즉시 변경할 수 있는 파라미터파일들이 있기 때문에 시스템 운용에 있어 매우 중요한 의미를 가진다. |
|  /tmp  | 임시 파일 |
|  /var  | 시스템 관련 스풀, 메일 등이 위치한 디렉토리 |


## 03 리눅스에서의 파티션

리눅스에서의 파티션에 대해 들어가기에 앞서서 파티션에 대해 간단히 보고 가고자 한다.

> 일반적으로 파티션(partition)은 무언가를 여러 부분으로 나누는 것을 뜻한다. 파티션은 다음의 뜻으로도 쓰인다.[^1]
>
>  * 일괄 처리 : 수많은 프로세서에 걸쳐 하나의 일괄 작업을 분할하는 것
>  * 디스크 파티션 : PC 시스템의 디스크를 구성하는 작업
>  * 논리 파티션 : 물리 메모리를 보호 영역으로 나누는 작업
>  * 파티션 (가구) : 사무실 또는 전시장 등의 특정 공간과 다른 공간의 사이를 나누어 주는 벽

[^1]: 출처 : 위키백과 - 파티션

위의 파티션 중에 우리가 앞으로 보고자하는 파티션은 디스크 파티션이므로 그에 대한 정의 또한 보면 아래와 같다.

>디스크 파티션(disk partition) 작업은 하드 디스크 드라이브의 기억 공간을 "파티션"이라 알려진 별도의 데이터 영역으로 분할하는 것을 말한다. 파티션 편집기 프로그램을 사용하여 이러한 파티션을 만들고 없애고 수정할 수 있다. 한 번 디스크가 여러 개의 파티션으로 나뉘면 다른 카테고리의 디렉터리와 파일들은 다른 파티션에 저장된다. 파티션이 많아질수록 제어권이 더 많아지지만 너무 많으면 다루기 쉽지 않게 된다. 공간 관리, 접근 허가, 디렉터리 검색이 추가되는 방식은 파티션 상에 설치된 파일 시스템의 종류에 달려 있다. 파티션 크기는 파티션에 설치된 파일 시스템에 따라 그 지원 능력이 달라지므로 파티션 크기를 조심스럽게 살피는 것이 필요하다.[^2]

[^2]: 출처 : 위키백과 – 디스크 파티션

리눅스에서의 파티션은 윈도우에서의 파티션과 아래와 같이 차이를 보인다.

* 윈도우

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/파티션_윈도우.jpg)

* 리눅스

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/파티션_리눅스1.jpg)

윈도우에서 파티션을 나누면 위의 그림과 같이 크기에 따라 파티션이 나뉘게 된다. 하지만 리눅스에서 파티션은 기본파티션과 확장파티션이 있으며 이에 대한 것을 앞으로 설명할 것이다.

앞으로 설명할 내용은 다음 주소의 사이트를 참조하여 작성한 것이다.

>[정보보안전문가를 위한 첫걸음](http://cafe.naver.com/windowsmcp/551)
>
>[정보 보안 전문가 공식 커뮤니티](http://cafe.naver.com/withsamo/60)
>
>[네이버 지식in](http://kin.naver.com/qna/detail.nhn?d1id=1&dirId=10302&docId=77038816&qb=66as64iF7Iqk7YyM7Yuw7IWY&enc=utf8&ion=kin&rank=1&search_sort=0&spq=0&pid=RwSrVlpySEdsstQSxWssssssssl-305985&sid=UwVrjgpyVooAAHVK4@g)
>
>[장상진님의 블로그](http://blog.naver.com/comtoman?Redirect=Log&logNo=150004556923)

리눅스에서 파티션 종류는 기본파티션(Primary)와 확장파티션(Extended) 두 종류가 있다. 하나의 하드디스크는 최대 기본 파티션 3개와 확장 파티션 1개를 만들 수 있다. 확장 파티션을 생성 하지 않을 경우 기본 파티션을 최대 4개까지 만들 수 있다. 확장 파티션은 또다시 논리 파티션으로 나눌 수도 있으며, 논리 파티션은 최대12개까지 만들 수 있습니다. 이 최대 개수에 대한 설명은 아래와 같다.[^3]

[^3]: 논리 파티션의 개수에 대한 부분은 앞에 다시 설명할 것이다.

|  파티션 종류  |  파티션 최대 개수  |
|:----------:|:--------------:|
|  primary  |  4  |
|  extended  |  1  |
|  logic  |  12  |

설명에 앞서 MBR방식으로 초기화된 HDD를 그냥 HDD라고 표기하고자 한다. MBR방식으로 초기화된 HDD에는 4개의 파티션을 만들 수 있는데 이는 512 바이트의 Master Boot Record라는 공간에 파티션정보를 담을 수 있는 공간이 4개 까지 밖에 없기 때문이다. 이 4개에 포함되는 것이 primary와 extended이다. 그런데 extended는 1개를 초과해서 만들 수 없다. 따라서 최대 파티션의 개수는 primary 4개 혹은 primary 3개 + extended 1개 이다.

logical drive는 드라이브로서 extended partition 안에 담겨지게 되며 extended partition 은 logical drive를 담아두는 역할만 하며 자체로는 쓰이지 않는다. logical drive에 대한 파티션정보는 MBR이 아닌 extended 파티션 안에 담겨지게 된다.[^4]

[^4]: 정확히는 logical drive가 다음 logical drive에 대한 정보를 가지고 있다고 한다.

logical drive는 디스크 하나 당 SCSI 하드디스크는 15개, IDE 하드디스크는 63개의 파티션을 가질 수 있지만 하나의 물리적인 디스크 안에 12개 이상 파티션을 만드는 것은 좋지 않은 것으로 알려져 있다.

어떤 운영체제들은 논리파티션에 운영체제를 설치할 수 없는데 MS Windows의 경우도 반드시 Primary 파티션에 OS를 설치해야 하므로 설치 시 이 점을 고려해야 한다.


파티션은 실린더를 기준으로 나누어지게 되며 파티션을 나누면 아래 그림처럼 선택한 용량만큼 실린더 범위를 할당 받게 된다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/image007-comtoman.gif)

여기서 실린더라 함은 하드 디스크의 구분하는데 쓰이는데 하드디스크의 디스크 부분은 트랙(Track), 섹터(Sector), 실린더(Cylinder)로 구분되어 있다.

>**트랙** : 자기 디스크 등의 회전하는 기억 매체나 자기 테이프 상의 테이프를 물리적으로 기록하는 부분. 자기 디스크의 경우, 「트랙」은 원반상에 동심원상으로 되어 있다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/dthumb.phinf.naver.net.jpg)
>
>**섹터**	: 자기 디스크, 디스크 팩 등의 동심원상에 정보가 기록되어 있는 각 트랙(track)을 똑같은 길이로 분할했을 때 그 일부분이며, 이 섹터마다 붙여진 어드레스를 섹터 어드레스(sector address)라 하고, 섹터를 고정 길이 레코드(fixed length record)로 취급한다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/dthumb.phinf.naver.net2.jpg)
>
>**실린더** : 하드 디스크 팩 내의 모든 디스크 상의 동일한 트랙이 수직으로 쌓여 있는 것으로 실린더 형태를 하고 있어서 이런 이름을 붙였다.
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/dthumb.phinf.naver.net3.jpg)
>
>![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/chs-comtoman.gif)
>
>출처[^5]

[^5]: 출처 : 컴퓨터인터넷IT용어대사전, 2011.1.20, 일진사

위에서 언급한 MBR에 대해서 언급하면 운영체제가 돌아가기까지의 과정을 보아야 한다. 먼저 컴퓨터에 전원을 넣게 되면, ROM BIOS에 들어 있는 작은 프로그램(Bootstrap Loader)이 하드디스크의 첫 번째 트랙, 첫 번째 섹터에 해당하는 MBR(Master Boot Record)에 있는 프로그램을 실행 시킨다. MBR에는 그 프로그램 외에도 파티션 테이블이 들어 있는데, 다음과 같은 정보를 포함하고 있습니다.

  * 파티션의 시작과 끝을 나타내는 주소 (파티션의 크기와 위치를 알 수 있다.)
  * 부팅 가능한 파티션인지의 Flag
  * 파티션 타입 (2자릿수의 ID로 표현, 파티션이 어떤 용도로 사용될 것인지를 나타내나, 표준이 아니고, 파일시스템마다 사용하는 값이 다를 수 있다.)

MBR안의 작은 프로그램은 이 정보를 바탕으로 부팅 가능한 운영체제의 부트 섹터로 제어권을 넘기게 되고, 제어권을 넘겨받은 부트섹터는 자신의 운영체제를 메모리 안으로 끌어들이기 위한 부트 프로그램을 실행시킨다.

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/image006-comtoman.gif)


## 04 장치와 파티션에 대한 표현

#### 하드 디스크 장치에 대한 표현

	하드 디스크 추가 시 알파벳순으로 증가


>**EIDE HardDisk : /dev/hd**
>         ( /dev/hda(첫번째) , /dev/hdb(두번째), /dev/hdc(세번째), /dev/hdd(네번째) )

>**SATA HDD : /dev/sd**
>         ( /dev/sda(첫번째) , /dev/sdb(두번째), /dev/sdc(세번째), /dev/sdd(네번째) )

>**SCSI HDD : /dev/sd**
>         ( /dev/sda(첫번째) , /dev/sdb(두번째), /dev/sdc(세번째), /dev/sdd(네번째) )

#### 파티션의 표현

	/dev/sda(첫번째 HardDisk)의 기본 파티션의 경우

  * 기본 파티션 : 순서대로 1-4번까지 지정
    * 기본 파티션 1 : /dev/sda1
    * 기본 파티션 2 : /dev/sda2
    * 기본 파티션 3 : /dev/sda3
    * 기본 파티션 4 : /dev/sda4
  * 확장파티션의 논리 파티션 : 반드시 5번부터 번호가 부여[^6]
    * 논리파티션 : /dev/sda5 , /dev/sda5 ~ /dev/sda11, /dev/sda12

[^6]: 기본파티션이 2개이고 확장파티션 1개이더라도 5번부터 부여된다.

#### 리눅스 파티션 트리
>|---primary 파티션 /dev/sda1
>
>|---primary 파티션 /dev/sda2
>
>|---primary 파티션 /dev/sda3
>
>|---extend 파티션 /dev/sda4
>
>>|---logical 파티션 /dev/sda5
>>
>>|---logical 파티션 /dev/sda6
>>
>>~
>>
>>|---logical 파티션 /dev/sda16

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/파티션_리눅스2.jpg)

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/파티션_리눅스3.jpg)


## 05 마운트

>마운트(mount)는 컴퓨터 과학에서 저장 장치에 접근할 수 있는 경로를 디렉터리 구조에 편입시키는 작업을 말한다. 좁은 의미로는 유닉스 계열의 운영체제에서의 mount 명령어 또는 그 명령어를 사용하는 것을 말한다. mount 명령어를 사용하면 저장 장치의 접근 경로를 원하는 위치에 생성할 수 있다. 마운트를 이용하면 분산 파일시스템으로 확장하기가 용이하다.[^7]

[^7]: 출처 :　위키백과 - 마운트

![screenshot]({{ site.baseurl }}/assets/media/linux-starter-guide/마운트.jpg)


      * 네이티브 파티션(/)와 스왑 파티션(swap)은 반드시 생성 하여야 한다.

      * 스왑 파티션은 물리적인 메모리의 2배를 설정 한다. 단 2GB 이상의 물리적 메모리의 경우에는 물리적 메모리와 동일하게 설정하거나 그 이하여도 상관없다.


## 06 파티션 분할의 이점


* **파일시스템 점검 시간을 줄여 부팅 속도 증가** : fsck 유틸리티에 의하여 파일시스템 점검을 할 수 있는데 하나의 단일 파티션보다는 분할되어 있는 파티션 중 점검해야하는 파티션만을 선택하여 점검이 가능하므로 부팅 시간이 단축 될 수 있다.

* **파티션에 문제 발생 시 다른 파티션 자료 보호** : 단일 파티션의 경우에는 네이티브 파티션(/)이 손상될 경우 모든 데이터가 파괴되거나 하나의 응용프로그램 등의 무제로 치명적 손상이 발생할 경우 모든 데이터는 파괴될 수 있다. 만일 각 파티션이 분할되어 있다면 [/] 파티션이 손상 되더라도 나머지 파티션에 존재하는 데이터들은 안전하게 복구할 수 있는 이점이 있다.

* **자료 백업의 용이** : 단일 파티션의 경우에는 데이터를 백업하기 위해서는 다른 하드 디스크나 다른 곳으로 백업을 해야 한다. 하지만 파티션이 분할되어 있을 경우에는 다른 파티션으로 자료를 백업할 수 있다.

* **특정 파티션 읽기 전용 마운트 가능** : 특정 파티션을 읽기 전용으로 마운트 하여 자료를 안전하게 보호할 수 있다.


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

