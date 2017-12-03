---
layout: post
title: 'IntelliJ를 사용할 때 Spring Boot에서 Jsp를 사용하지 못하는 문제'
date: 2015-05-24 16:30:00
categories: Tool
tags: [IntelliJ, Spring Boot, Jsp]
published: true
fullview: false
comments: true
---

{% include toc.html %}

필자는 IntelliJ를 사용하고 있고 최근에 Spring Boot를 사용해보고 있다. 문제는 Spring Boot에서 Jsp를 사용하고자 하면서 발생하였다. Spring Boot에서 Jsp를 사용하기 위해 Gradle을 다음과 같이 작성하고 ``build``하였다.

```java
buildscript {
    repositories {
        maven { url "http://repo.spring.io/libs-snapshot" }
        mavenLocal()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.1.10.RELEASE")
    }
}

group 'lab_study'
version '1.0-SNAPSHOT'

apply plugin: 'java'
apply plugin: 'war'
apply plugin: 'idea'
apply plugin: 'eclipse'
apply plugin: 'spring-boot'

war {
    baseName = 'lab_study'
    version =  '0.0.1'
}

compileJava.options.encoding = 'UTF-8'

repositories {
    mavenCentral()
    jcenter()
    maven { url "http://repo.spring.io/libs-snapshot" }
}

dependencies {

    compile 'org.springframework.boot:spring-boot-starter-web:1.2.3.RELEASE'
    compile 'org.springframework.data:spring-data-jpa:1.8.0.RELEASE'

    //Required dependency for JSP
    providedRuntime 'org.apache.tomcat.embed:tomcat-embed-jasper'

    testCompile group: 'junit', name: 'junit', version: '4.11'
}

```

다음과 같이 작성하고 Application을 실행하면 Jsp가 제대로 작동하지 않는 것을 볼 수 있다. 아래와 같이 Jsp의 내용을 그대로 보여주는 현상이다.

![screenshot]({{ site.baseurl }}/assets/media/springboot-jsp.png)

이를 IntelliJ에서 해결하기 위해 여러가지를 시도해본 결과, Gradle Task중에 ``bootRun``을 실행하면 Jsp가 제대로 작동한다.

이 현상을 해결해보기 위해 Maven도 사용해보았지만 동일하게 Jsp가 작동하지 않았다. 물론, 필자가 사용하고 있는 MacOS만의 문제인지는 모르겠다.

단, 똑같은 프로그램으로 eclipse에서 실행할 경우 문제없이 작동한다.

필자가 실행한 환경은 다음과 같다.

	MacOS 10.8+
	IntelliJ 14.1.3
