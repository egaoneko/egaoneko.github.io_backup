---
layout: post
title: 'URL parsing'
date: 2018-01-29 23:50:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## 들어가며

매번 JS에서 URL을 파싱하기 위한 방법을 감섹히건 헸는데, 이번 기회에 기록하고자 한다.

## 정규 표현식

```javascript
// parseUri 1.2.2
// (c) Steven Levithan <stevenlevithan.com>
// MIT License

function parseUri (str) {
	var	o   = parseUri.options,
		m   = o.parser[o.strictMode ? "strict" : "loose"].exec(str),
		uri = {},
		i   = 14;

	while (i--) uri[o.key[i]] = m[i] || "";

	uri[o.q.name] = {};
	uri[o.key[12]].replace(o.q.parser, function ($0, $1, $2) {
		if ($1) uri[o.q.name][$1] = $2;
	});

	return uri;
};

parseUri.options = {
	strictMode: false,
	key: ["source","protocol","authority","userInfo","user","password","host","port","relative","path","directory","file","query","anchor"],
	q:   {
		name:   "queryKey",
		parser: /(?:^|&)([^&=]*)=?([^&]*)/g
	},
	parser: {
		strict: /^(?:([^:\/?#]+):)?(?:\/\/((?:(([^:@]*)(?::([^:@]*))?)?@)?([^:\/?#]*)(?::(\d*))?))?((((?:[^?#\/]*\/)*)([^?#]*))(?:\?([^#]*))?(?:#(.*))?)/,
		loose:  /^(?:(?![^:@]+:[^:@\/]*@)([^:\/?#.]+):)?(?:\/\/)?((?:(([^:@]*)(?::([^:@]*))?)?@)?([^:\/?#]*)(?::(\d*))?)(((\/(?:[^?#](?![^?#\/]*\.[^?#\/.]+(?:[?#]|$)))*\/?)?([^?#\/]*))(?:\?([^#]*))?(?:#(.*))?)/
	}
};

// optional
parseUri.options.strictMode = true;
```

* [parseUri 1.2: Split URLs in JavaScript](http://blog.stevenlevithan.com/archives/parseuri)
* [metafeather/URL parsing Regex.js](https://gist.github.com/metafeather/202974/34c2d31bd82f59c2486f38790054bbbc0b10ca8b)

상단의 코드와 같이 정규 표현식을 사용하여 URL을 파싱할 수 있다. 구버전 브라우저에서도 정상적으로 동작을 하겠지만, Anchor 태그를 사용하는 방법와 달리 별도로 코드를 관리해야해서 구버전 브라우저를 지원해야하는 경우가 아니라면 Anchor 태그를 사용하는 방법을 선호한다.

## Anchor 태그

```javascript
var parser = document.createElement('a');
parser.href = "http://example.com:3000/pathname/?search=test#hash";

parser.protocol; // => "http:"
parser.hostname; // => "example.com"
parser.port;     // => "3000"
parser.pathname; // => "/pathname/"
parser.search;   // => "?search=test"
parser.hash;     // => "#hash"
parser.host;     // => "example.com:3000"
```

* [jlong/uri.js](https://gist.github.com/jlong/2428561)

정규표현식보다 훨신 간단하게 사용할 수 있으며, IE8 부터 사용이 가능하다고하여 애용하고 있는 방법이다.

## window location

```javascript
window.location.host // you'll get sub.domain.com:8080 or sub.domain.com:80
window.location.hostname // you'll get sub.domain.com
window.location.protocol // you'll get http:
window.location.port // you'll get 8080 or 80
window.location.pathname // you'll get /virtualPath
```

* [How to extract the hostname portion of a URL in JavaScript
 - stack overflow](https://stackoverflow.com/questions/1368264/how-to-extract-the-hostname-portion-of-a-url-in-javascript)

현재 URL을 파싱해야하는 경우에는 다른 방법을 사용하지 않고도 `window.location`을 사용할 수 있다.