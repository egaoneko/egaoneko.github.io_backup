---
layout: post
title: '[Django] python3.x mysql connector'
date: 2014-09-04 12:30:25
categories: Python
tags: [Python, Connector, Django, MySQL, Web]
published: true
fullview: false
comments: true
---

Django에서 MySQL을 이용하기 위해서 Python과 MySQL을 이어줄 Connector가 필요했는데 Python의 MySQL Connector는 MySQL 공식 홈페이지에서 다운받아 사용하였다.


> [python-mysql connector 다운로드 페이지](http://dev.mysql.com/downloads/connector/python/)


작성일(14.09.04) 기준에 MS Windows에서 MSI로 설치 가능한 Python버전은 "2.7, 3.2, 3.3"이었으며 그 이상의 버전(3.4)에서 설치하기 위해서 **Platform Independent**에서 Zip 파일을 다운받아 별도의 방법으로 설치하였다.


> [Platform Independent로 설치하는 방법](http://dev.mysql.com/doc/connector-python/en
/connector-python-installation-source-windows.html)

Django에서 MySQL Connector를 이용하는 방법은 Django 공식 홈페이지에서 사용하는 방법과는 다르며 해당 방법또한 MySQL 공식페이지에 있다.

> [Django에서 mysql-connector 사용하기](http://dev.mysql.com/doc/connector-python/en
/connector-python-django-backend.html)
