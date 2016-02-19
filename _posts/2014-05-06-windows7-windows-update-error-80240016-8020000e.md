---
layout: post
title: '[Windows7] Windows 업데이트 오류 80240016 & 8020000E'
date: 2014-05-06 13:25:33
categories: Tips
tags: [OS, Windows7]
published: true
---

![screenshot]({{ site.url }}/assets/media/windows-error1.PNG)

원인을 알 수 없는(아마, 우분투 설치로 파티션을 너무 많이 변경해서일지도...) 문제로 **Windows 업데이트 오류 80240016문제**가 발생했습니다. 그래서 업데이트 도중에 위와같은 불길한 느낌이 드는 빨간 딱지와 조우하게 됬고요. 이를 해결하기위해 네이버와 구글 검색을 동원해서 찾아보았습니다.

![screenshot]({{ site.url }}/assets/media/windows-error2.PNG)

해당 오류를 해결하기 위해 위의 절차로 진행했어요. 문제는 저는 저게 해결된 건지 아닌 건지 모르고 그 다음 오류와 다시 조우했습니다. 해당 오류는 **"WindowsUpdate_8020000E"** 였고 이를 해결하기 위한 방법으로 아래와 같이 수행했습니다.

![screenshot]({{ site.url }}/assets/media/windows-error3.PNG)

위의 절차를 따랐지만 해당 과정을 수행해도 오류는 남아있었고, 그래서 위 글의 조언에 따라 [Windows Update Agent](http://support.microsoft.com/kb/949104)의 링크로 가서 프로그램을 다운받아 수행한 후 해결되었습니다. 위의 어느 과정에서 해결될 지는 모르겠으나 이것이 제가 해결했던 전 과정입니다.

