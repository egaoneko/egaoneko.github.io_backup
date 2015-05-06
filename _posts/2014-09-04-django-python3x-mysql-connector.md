---
layout: post
title: '[Django] python3.x mysql connector'
date: 2014-09-04 12:30:25
categories: Python
tags: [Python, Connector, Django, MySQL, Web]
published: true
---

![screenshot]({{ site.url }}/assets/media/django.jpg)

[Copyrightⓒ](https://www.flickr.com/photos/hoerner_brett/2901426375/in/photolist-4b39Ci-927Yj-dF6H1K-5ij6yy-bmYVz7-bmYVR3-bmYVuY-bmYVBS-bzTKLP-3pKbiV-5qoyYF-HKB4U-4QxCpS-4QxCpW-dNr5LE-9JdN84-HAZtp-dKHLMZ-7ihZJ-bmYViw-bzTL96-bmYVES-bzTKzX-bzTKFV-bmYVbU-bmYVxu-bmYVHC-bzTKJk-wN4re-5bALjQ-dKHMzn-dKHMSv-dKPgy5-dKHN7T-4JmVmc-63P8TP-59xMaK-qoEdp-dRqXHM-3pKcH4-6koLtL-5cGYq7-5xQHxx-9HLF7b-9mapF1-6kgroa-4SCbhn-5aRvib-gP6Gwf-4aJ7gQ)

Django에서 MySQL을 이용하기 위해서 Python과 MySQL을 이어줄 Connector가 필요했는데 Python의 MySQL Connector는 MySQL 공식 홈페이지에서 다운받아 사용하였다.


> [python-mysql connector 다운로드 페이지](http://dev.mysql.com/downloads/connector/python/)


작성일(14.09.04) 기준에 MS Windows에서 MSI로 설치 가능한 Python버전은 "2.7, 3.2, 3.3"이었으며 그 이상의 버전(3.4)에서 설치하기 위해서 **Platform Independent**에서 Zip 파일을 다운받아 별도의 방법으로 설치하였다.


> [Platform Independent로 설치하는 방법](http://dev.mysql.com/doc/connector-python/en
/connector-python-installation-source-windows.html)

Django에서 MySQL Connector를 이용하는 방법은 Django 공식 홈페이지에서 사용하는 방법과는 다르며 해당 방법또한 MySQL 공식페이지에 있다.

> [Django에서 mysql-connector 사용하기](http://dev.mysql.com/doc/connector-python/en
/connector-python-django-backend.html)

