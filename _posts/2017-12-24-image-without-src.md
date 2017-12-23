---
layout: post
title: '이미지에 src가 없을 때 처리'
date: 2017-12-24 01:00:00
categories: frontend
tags: [image, til]
published: true
fullview: false
comments: true
---

이미지를 지연해서 불러오기 위해서 `src` 값을 `data-src` 속성에 넣어두고 `src`를 비워두었더니, 이미지가 로드되기전에 `src` 가 없어 이미지를 불러오지 못하는 에러 표시가 나타났다.

이를 해결하기 위해서 찾아보고 아래와 같이 `base64` 이미지를 사용하였다.

```xml
<img src="data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=" width="0" height="0" alt="" />
```

개인적인 생각으로는 사실 이 방법보다는 임시 이미지가 있어야된다고 생각한다. 이 외에도 다른 방법들이 있으니 아래글을 참고하자.

* 참고
  * [What's the valid way to include an image with no src? - stack overflow](https://stackoverflow.com/questions/5775469/whats-the-valid-way-to-include-an-image-with-no-src)