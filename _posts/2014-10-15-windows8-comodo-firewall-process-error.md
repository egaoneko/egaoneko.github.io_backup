---
layout: post
title: '[Windows8.1 & COMODO Firewall] Windows8.1에서 COMODO Firewall을 사용할 때, 프로세스 종료 문제'
date: 2014-10-15 15:15:39
categories: Etc
tags: [comodo firewall, OS, Windows8.1]
published: true
fullview: false
comments: true
---

![screenshot]({{ site.url }}/assets/media/comodo.png)

현재 제가 사용하고 있는 보안 프로그램의 조합은 Avast!(백신) + COMODO Firewall(방화벽) 입니다.

얼마전에 다시 Windows8.1 embedded를 노트북에 깔면서 명령 프롬프트가 종료되지 않는 문제가 발생했어요. 심지어 "taskkill"에 "/f" 옵션을 줘도 종료가 되지 않더라고요. 종료 방법은 오로지 "exit"을 프롬프트에 직접 줘야만 되더라고요. dokuwiki on stick의 서버와 같이 입력 불가능한 프롬프트는 PC의 계정 로그아웃이나 재부팅 밖에 방법이 없었습니다.

해당 문제를 해결하기 위해서 여러 사이트에 질문을 올려봤지만 뾰족한 해결책을 찾을 수 없더라고요.



오늘 여러 사이트들 중 하나였던 [MS 커뮤니티](http://answers.microsoft.com/ko-kr/windows/forum/windows8_1-performance/cmd%EC%B0%BD%EC%9D%98-x/bc45429b-7ddd-417d-a4e0-6f628d32837f)에 답글이 올라와 확인해봤습니다.

> Windows 8.1 사용 중 명령 프롬프트에서 닫기 버튼을 통한 종료가 정상적으로 되지 않는 문제로 문의를 주셨습니다.
>
> 문의주신 내용의 경우, 해당 부분만으로는 정확한 원인 파악에 다소 어려움이 있는 점 양해의 말씀을 드립니다.
>
> 우선, 아래의 방법을 참고하여 점검을 진행해보실 것을 권장 드립니다.
>
> **작업 방법 1. 시작 프로그램에서 타사 프로그램 내리기 (클린부팅 방법)**
>
> * [클린부팅](http://answers.microsoft.com/ko-
kr/windows/forum/windows_8-performance/windows-8-%EC%8B%9C%EC%9E%91/5e5f4fff-
dc9c-4a0f-8c9f-9139218819dd)
> * [Windows 8/8.1](http://answers.microsoft.com/ko-
kr/windows/forum/windows_8-performance/windows-8-%EC%8B%9C%EC%9E%91/5e5f4fff-dc9c-4a0f-8c9f-9139218819dd)
>
> **작업 방법 2. 시스템 파일 및 Dism 검사**
>
>   1. **Windows 로고키 + X키**를 누른 후 **명령 프롬프트(관리자)**를 클릭합니다.
>   2. 명령 프롬프트에서 아래 명령을 입력하고 **Enter**키를 누릅니다.
>   **sfc /scannow**
>   3. 시스템 검사가 완료되면 명령 프롬프트에 아래 명령을 입력하고 **Enter **키를 누릅니다.
>	**Dism /online /cleanup-image /restorehealth**
>	* PC 환경에 따라 시스템 검사 시간이 오래 소요될 수 있습니다. (20분이상 )
>   4. 검사가 완료 되면 **Exit** 명령어 입력 후 컴퓨터를 다시 시작하여 문제점을 확인 합니다.
>
>
> * 명령어를 마우스로 드래그하여 마우스 오른쪽 버튼 클릭 후 [복사 및 붙여넣기] 하여 진행하실 수 있으나, 붙여넣기 단축키(Ctrl키+V키)는 사용할 수 없습니다.
>
> 또한, 명령어를 직접 입력하는 경우에는 띄어쓰기에 주의하시기 바랍니다.
>
> **작업 방법 3. *바이러스 검사**
>
> 백신 프로그램을 이용하여 전체 검사를 시도해 보시고, 별도의 백신 프로그램이 필요한 경우에는 아래의 내용을 참고해보실 수 있습니다.
>
> [Malwarebyte](http://www.malwarebytes.org/mbam.php)
>
> 바이러스, 웜, 트로이 목마, 루트킷, 스파이웨어,등 악성 프로그램을 제거 할 수 있는 Tool 으로 Malwarebytes 사에서 제공된 프로그램 입니다.
>
> **작업 방법 4. 새 계정에서 확인**
>
> 현재 사용 중인 계정 이외에 새 계정(관리자 권한을 가진 로컬 계정 및 Microsoft 계정)을 생성하여 로그인 한 후 해당 계정에서도 동일한 증상이 발생하는지 확인합니다.
>
> 만약, 현재 최고 관리자 계정(Administrator)을 활성화하여 사용하고 있는 경우, 관리자 권한을 가진 로컬 / Microsoft 계정을 사용하실 것을 권장 드립니다.
>
> * [새 계정 만들기](http://windows.microsoft.com/ko-kr/windows/create-user-account#create-user-account=windows-8)


작업 방법 4가지 중에 첫번 째 방법으로 바로 원인을 찾았습니다. COMODO Firewall이 그 원인이었습니다. ~~(Git문제로도 속석이더니 여기서도.. [해당링크]({% post_url 2014-10-14-git-comodo-firewall-loading %}))~~

COMODO Firewall의 HIPS기능이 해당 문제를 발생시켰네요. HIPS기능을 사용하지 않으면 해당 문제는 발생하지 않더라고요. 문제는 HIPS기능을 사용하면서 명령 프롬프트만 종료될 수 있게 하고자 여러 옵션을 만져봤는데요. 일단 원인이 발견되는 옵션을 찾았습니다.

* "Adanved Settings" > "Security Settings" : 고급 설정을 열어서 보안 설정으로 이동합니다.
* "Defense+" > "HIPS" > "HIPS Settings" : 보안 설정의 "Defense+(지킴이)로 이동하여 HIPS 설정으로 이동합니다.
* "Enable HIPS" > "Safe Mode" > "Monitoring Settings" : HIPS 사용에서 안정 방식이로 드롭다운되어있는데 그 옆에 감시 설정이 있습니다.
* "Activites to Monitor" > "Processes' Terminations" : 감시할 활동 부분의 프로세스 제거 부분을 체크 해제하면 됩니다.

위의 방법을 사용하면 발생하는 문제를 해결할 수는 있었어요. 물론 이게 보안에 어떤 영향을 끼칠지 모르고 했지만요. 해당 부분 체크해도 Windows7에서는 문제가 발생하지 않네요. 이 부분에 대해서는 추가 문의를 했어요.저는 일단 해당 부분만 체크 해제하고 사용하고 있습니다. 추가로 응답이 오면 추가하도록 할게요.
