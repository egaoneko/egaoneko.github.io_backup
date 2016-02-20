---
layout: post
title: 'Travis CI를 사용해서 Jekyll Build error message 보기'
date: 2015-05-09 15:17:00
categories: Blog
tags: [Blog, Jekyll, Travis CI]
published: true
fullview: false
comments: true
---

Wordpress에서 Jekyll로 migration을 시도하고 Jekyll에 맞춘 파일은 github로 올렸을 때 페이지가 생성되지 않는 오류가 있었다. Local에서는 문제없이 실행되던 것들이 github로 올리면 문제가 발생하였고 이를 해결할 방법을 열심히 찾았었는데 그 방법은 github의 help에 있었다.

github의 help 중 [Troubleshooting GitHub Pages build failures](https://help.github.com/articles/troubleshooting-github-pages-build-failures/#viewing-build-error-messages)를 보면 error message들을 확인하는 방법을 소개한다. 필자는 Local에서 error message가 발생하지 않아서 Travis CI를 통해 error를 확인하였다.

단지 Travis CI를 사용할 때 ruby버전이 낮아서 help에 소개된 방법으로는 오류가 발생하였고 이를 해결하기 위해 ``.travis.yml``을 다음과 같이 바꾸어 설정하였다.

help에 소개되어 있는 ``.travis.yml``

```bash
language: ruby
script: "bundle exec jekyll build"
```

변경한 ``.travis.yml``

```bash
language: ruby
rvm:
  - 2.1.0
  - jruby-18mode
  - jruby-19mode
  - rbx-2
  - ruby-head
  - jruby-head
  - ree

script: "bundle exec jekyll build"
```

