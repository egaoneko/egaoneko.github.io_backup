---
layout: post
title: '[리눅스 정리 - 초보자를 위한 가이드] 8장. 쉘 스크립트'
date: 2015-05-24 21:00:08
categories: os
tags: [os, linux, guide]
published: true
fullview: false
comments: true
---

쉘 스크립트 부분에 대한 전반적인 내용은 유닉스 리눅스 사용에서 프로그래밍까지(창병모 저)라는 책을 참조하며 작성하였다.

>쉘 스크립트(shell script)는 쉘이나 명령 줄 인터프리터에서 돌아가도록 작성되었거나 한 운영 체제를 위해 쓰인 스크립트이다. 단순한 도메인 고유 언어로 여기기도 한다. 쉘 스크립트가 수행하는 일반 기능으로는 파일 이용, 프로그램 실행, 문자열 출력 등이 있다.[^1]

[^1]: 출처 : 위키백과 – 쉘 스크립트

* * *

**장점**

쉘 스크립트를 기록하는 것은 다른 프로그래밍 언어의 같은 코드로 쓰인 것보다 훨씬 더 빠른 경우가 많다. 다른 해석 언어에 비해, 쉘 스크립트는 컴파일 단계가 없기 때문에 스크립트는 디버깅을 하는 동안 빠르게 실행할 수 있다.

또한 Python과 같이 독립적으로 설치할 필요가 없기 때문에 쉘 자체가 프로그램 개발 툴로 사용할 수 있다.

* * *

**단점**

한 가지 단점으로는 실행되는 각 명령에 대한 잠재적으로 새로운 하부 프로세스의 수많은 필요에 따라 속도가 느려질 수 있다.

단순 sh 스크립트는 다양한 종류의 유닉스, 리눅스, BSD 운영 체제, therof 버전, 시스템 유틸리티와 잘 호환된다는 장점이 있지만 더 복잡한 쉘 스크립트는 쉘, 유틸리티, 다른 필수 요소 간의 약간의 차이가 많은 경우 실패할 가능성이 있다. 레리 월은 다음과 같은 유명한 말을 남겼다: "쉘 스크립트보다 쉘을 포팅하는 게 더 쉽다"

이와 비슷하게, 더 많은 복잡한 스크립트들은 쉘 스크립트 언어 자체의 제한 안에서 실행할 수 있다. 이러한 제한 때문에 다양한 쉘이 문제를 개선할 목적으로 고품질의 코드와 확장을 기록하기 힘들 수 있다.


## 01 쉘 레벨 및 변수

쉘에서 쉘을 재귀적으로 실행할 수 되며 이는 #echo $SHLVL이라는 명령어를 통해서 현재 쉘의 쉘 레벨을 확인할 수 있다.

<figure><img src="/images/linux-starter-guide/스크립트1.png" alt=""></figure>
<figure><img src="/images/linux-starter-guide/쉘레벨.jpg" alt=""></figure>


쉘의 변수는 환경변수와 지역변수 두 종류로 나눌 수 있는데 환경 변수는 값이 자식 프로세스에게 상속되며 지역변수는 그렇지 않다.

<figure><img src="/images/linux-starter-guide/스크립트2.png" alt=""></figure>


\#export [변수명]=[값]을 통해 변수를 선언하게 되면 쉘 레벨과 관계없이 변수 공유가 가능하다.

<figure><img src="/images/linux-starter-guide/스크립트3.png" alt=""></figure>


#### 쉘 스크립트의 변수는 다음과 같은 특징을 가진다.

* 타입이 존재하지 않음 (문자열로 취급)
* 대소문자 구별
* 미리 선언할 필요가 없음
* 변수 사용은 $[변수명]
* 정의는 [변수명]=[값]



* * *

### 리스트 변수 (list varialbe)

한 변수에 여러 개의 값 (문자열)을 저장할 수 있는 변수

      # 이름=(단어리스트)
            ex) # cities=(서울 부산 인천)

|  리스트 사용  |  의미  |
|:--:|:--:|
|  ${name[i]}  |  리스트 변수 name의 i번째 원소 (i=0, 1…)  |
|  ${name[*]}  |  “$1 $2 …” 리스트 변수 name의 모든 원소  |
|  ${name[@]}  |  “$1” “$2” … 리스트 변수 name의 모든 원소  |
|  ${#name[*]}  |  리스트 변수 name 내의 원소 개수  |
|  ${#name[@]}  |  리스트 변수 name 내의 원소 개수  |

* * *

### 표준 입력 읽기 (read 명령어)

표준입력에서 한 줄을 읽어서 단어들을 변수들에 순서대로 저장하며 남은 단어들은 마지막 변수에 모두 저장한다.

      # read 변수1, ...., 변수n
                ex) # read x y
                    Merry Christmas!

* * *

### 사전 정의 환경변수 (의미가 정해진 환경변수)

|  이름  |  의미  |
|:--:|:--:|
|  $USER  |  사용자 이름  |
|  $TERM  |  터미널 타입  |
|  $PATH  |  명령어를 검색할 디렉터리들의 리스트  |
|  $HOME  |  홈 디렉터리  |
|  $SHELL  |  로그인 쉘의 경로명  |
|  $MAIL  |  메일 박스의 경로명  |
|  $HOSTNAME  |  호스트 이름  |

* * *

### 사전 정의 지역변수

|  이름  |  의미  |
|:--:|:--:|
|  $$  |  쉘의 프로세스 번호  |
|  $0  |  쉘 스크립트 이름  |
|  $1 ~ $9  |  명령줄 인수  |
|  $*  |  모든 명령줄 인수 리스트  |
|  $#	명령줄  |  인수의 개수  |


## 02 산술 연산

	앞으로 나오는 연산이나 조건문, 반복문등, 앞선 변수 모두 띄어쓰기를 확실하게 구분하여야 오류가 발생하지 않는다.
	그리고 쉘 스크립트에서는 일반적으로 인자나 변수는 문자열 취급하기 때문에 다른 방식으로 연산을 한다.


* * *

### 본 쉘의 산술 연산

```bash
$ a=2+3
$ echo $a
$ a=`expr 2 + 3` (반드시 빈칸)
```

* * *

### 콘 쉘의 let 명령어를 이용한 산술 연산

```bash
$ let 변수=수식
$ let a=2*3
$ echo $a
6
```

* * *

### 콘 쉘의 let "수식"을 이용한 산술 수식

```bash
((수식)) = let " 수식 "

if (( $# != 1 ))
then
fi
```

* * *

### 변수 타입 선언 (declare)

```bash
$ declare -i a (a는 정수형 변수)
$ a=12
$ a=a+1 (let 필요 없음)
$ echo $a
```

|  이름  |  의미  |
|:--:|:--:|
|  declare -r 변수  |  읽기 전용 변수로 선언  |
|  declare -i변수  |  정수형 변수로 선언  |
|  declare -a 변수  |  배열 변수로 선언  |
|  declare -f  |  스크립트 안에정의된 모든 함수들을 보여준다.  |
|  declare -f 함수  |  해당 함수 내용을 보여준다.  |
|  declare -x 변수  |  환경변수로 export  |


## 03 비교 연산

### 산술 비교 연산

|  산술 비교 연산자  |  의미  |
|:--:|:--:|
|  정수1 -eq정수2  | 두 정수가 같으면 참 아니면 거짓 |
|  정수1 -ne 정수2  | 두 정수가 다르면 참 아니면 거짓 |
|  정수1 -gt 정수2  | 정수1이 정수2보다 크면 참 아니면 거짓 |
|  정수1 -ge 정수2  | 정수1이 정수2보다 크거나 같으면 참 아니면 거짓 |
|  정수1 -lt 정수2  | 정수1이 정수2보다 작으면 참 아니면 거짓 |
|  정수1 -le 정수2  | 정수1이 정수2보다 작거나 같으면 참 아니면 거짓 |

* * *

### 문자열 비교 연산

|  문자열 비교 연산자  |  의미  |
|:--:|:--:|
|  문자열1 == 문자열2  | 두 문자열이 같으면 참 아니면 거짓 |
|  문자열1  != 문자열2  | 두 문자열이 다르면 참 아니면 거짓 |
|  -n 문자열  | 문자열이 null이 아니면 참 |
|  -z 문자열  | 문자열이 null이면 참 |

* * *

### 파일 관련 연산

|  파일 관련 연산자  |  의미  |
|:--:|:--:|
|  -a 파일  | 해당 파일이 존재하면 참 |
|  -e 파일  | 해당 파일이 존재하면 참 |
|  -r 파일  | 사용자가 해당 파일을 읽을 수 있으면 참 |
|  -w 파일  | 사용자가 해당 파일을 쓸 수 있으면 참 |
|  -x 파일  | 사용자가 해당 파일을 실행할 수 있으면 참 |
|  -o 파일  | 사용자가 해당 파일의 소유자이면 참 |
|  -z 파일  | 해당 파일의 크기가 0이면 참 |
|  -f 파일  | 해당 파일이 일반 파일이면 참 |
|  -d 파일  | 해당 파일이 디렉터리이면 참 |


## 04 Bash 제어구조


### 조건문

                    if 조건식
                    then
                        명령어리스트
                    else
                        명령어리스트
                    fi

* * *

### 중첩 조건문

                    if 조건식
                    then
                        명령어리스트
                    elfi
                    then
                        명령어리스트
                    else
                        명령어리스트
                    fi

* * *

### 스위치

                    case 단어 in
                        패턴1) 명령어리스트;;
                        패턴2) 명령어리스트;;
                        ......
                        *) 명령어리스트;;
                    esac

* * *

### 반복문 (for) : 리스트의 각 값에 대해서 명령어들을 반복

                    for 변수 in 단어리스트 (모든 명령줄 인수 처리 : $*)
                    do
                        명령어리스트
                    done

* * *

### 반복문 (while) : 조건에 따라 명령어들을 반복 실행

                    while 조건식
                    do
                        명령어리스트
                    done

* * *

### 반복문 (until) : 조건에 따라 명령어들을 반복 실행

                    until 조건식 (조건식을 while문의 정반대로 넣어야 함)
                    do
                        명령어리스트
                    done

* * *

### 함수

                    함수이름()
                    {
                        명령어리스트
                    }


## 05 쉘 스크립트 사용 예

```bash
#! /bin/bash

FCnt=0
LFlag=0
PFlag=0
IFlag=0
FFlag=0
trash=~/.trash

error ()
{
cat << END
echo 휴지통 관리 프로그램 사용법 :
echo   trash -p : "휴지통 비우기"
echo   trash -i : "휴지통 비우기(확인)"
echo   trash -l : "휴지통 확인"
echo   trash 파일* : "휴지통에 버리기"
END
exit 1
}

for list in $*
do
  case $list in
    "-p")
        PFlag=1
        ;;
    "-i")
    IFlag=1
    ;;
    "-l")
        LFlag=1
        ;;
    "-*")
        echo $list 는 잘못된 옵션
    error
        ;;
    *)
        FFlag=1
        fileList[$FCnt]=$list
        let FCnt=FCnt+1
        ;;
    esac
done

let total=$LFlag+$PFlag+$FFlag+$IFlag
if (( total != 1 ))
then
   error
fi

if [ ! -d $trash ]
then
    'mkdir' $trash
fi

if (( LFlag == 1 ))
then
   'ls' -lgF $trash
   exit 0
fi

if (( PFlag == 1 ))
then
   'rm' $trash/*
   exit 0
fi

if (( IFlag == 1 ))
then
   'rm' -i $trash/*
   exit 0
fi


if ((FFlag == 1 ))
then
   'mv' ${fileList[*]} $trash
   exit 0
fi
exit 0
```

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

