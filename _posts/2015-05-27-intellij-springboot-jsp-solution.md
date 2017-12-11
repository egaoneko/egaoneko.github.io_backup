---
layout: post
title: 'IntelliJ를 사용할 때 Spring Boot에서 Jsp를 사용하지 못하는 문제 해결책'
date: 2015-05-27 21:20:00
categories: tool
tags: [intellij, spring boot, jsp]
published: true
fullview: false
comments: true
---

{% include toc.html %}

얼마전 IntelliJ에서 Spring Boot로 Jsp를 사용할 때 문제를 겪어 ["IntelliJ를 사용할 때 Spring Boot에서 Jsp를 사용하지 못하는 문제"]({% post_url 2015-05-24-intellij-springboot-jsp %})라는 글을 작성하였다. 이 문제에 대해 JetBrains에 문의에 문의를 해보았고 다음과 같은 답변을 받았다.

>This is a Spring Boot problem.
>
>See http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-web-applications.html#boot-features-jsp-limitations section "26.3.4 JSP limitations"
>
>Solutions:
>
>1. Do not use JSP at all (recommended)
>
>2. As workaround, enable special Gradle task "Build xxx.war artifact" in run configuration to simulate "bootRun" Gradle task (see screenshot)

![screenshot]({{ site.baseurl }}/assets/media/Run_Debug_Configurations.jpg)

답변의 링크와 같이 Spring Boot application을 실행할 때 JSP지원에 제한이 있다고 한다. 이에 대한 해결책으로는 Jsp를 사용하지 않는 것과 상단에 첨부된 사진같이 Application에 환경 설정을 하면된다. 필자가 직접 설정하고 실행해보니 Gradle로 ``bootRun``한 것과 동일하게 실행된다.
