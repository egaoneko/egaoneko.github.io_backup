---
layout: post
title: 'Ajax 단위 테스트'
date: 2017-01-29 21:30:00
categories: JavaScript
tags: [JavaScript, Ajax, Test]
published: true
fullview: false
comments: true
---

{% include toc.html %}

## 들어가기 앞서

본 포스트에서는 [Mocha](https://mochajs.org/), [Sinon.JS](http://sinonjs.org/)를 사용하였다.

## Ajax 단위 테스트

개인적으로 만들고 있는 프로그램에서 Ajax를 사용하는 부분이 있어, 이를 위해 [JUI](jui.io)에서 Ajax를 구현한 부분을 가져와 사용하고 있다. 이에 대한 구현은 아래와 같다.

```javascript
/**
 * ajax call
 * {Object} inputOpts object
 */
function _ajax (inputOpts) {
	let xhr = null;
	let callback = null;

	let opts = Object.assign({
		url: null,
		type: METHOD_TYPE.get,
		contentType: "application/x-www-form-urlencoded; charset=UTF-8",
		data: null,
		async: true,
		success: null,
		fail: null
	}, inputOpts);

	// check url
	if (!base.typeCheck("string", opts.url)) {
		return;
	}

	// check success
	if (!base.typeCheck("function", opts.success)) {
		return;
	}

	// check XMLHttpRequest
	xhr = getXMLHttpRequest();
	if (xhr != null) {
		xhr.open(opts.type, opts.url, opts.async);
		xhr.setRequestHeader("Content-Type", opts.contentType);

		if (!base.typeCheck("null", opts.data)) {
			xhr.send(opts.data);
		}

		callback = function () {
			if (xhr.readyState === 4 && xhr.status == 200) {
				opts.success(xhr.responseText, xhr.status, xhr);
			} else {
				if (base.typeCheck("function", opts.fail)) {
					opts.fail(xhr, xhr.status, xhr.statusText);
				}
			}
		};

		if (!opts.async) {
			callback();
		} else {
			xhr.onreadystatechange = callback;
		}
	}
}

/**
 * get XMLHttpRequest
 * @returns {*} XMLHttpRequest or null
 */
function getXMLHttpRequest () {
	let xhr = null;

	if (!base.typeCheck("undefined", XMLHttpRequest)) {
		xhr = new XMLHttpRequest();
	} else {
		var versions = [
			"MSXML2.XmlHttp.5.0",
			"MSXML2.XmlHttp.4.0",
			"MSXML2.XmlHttp.3.0",
			"MSXML2.XmlHttp.2.0",
			"Microsoft.XmlHttp"
		];

		for (var i = 0, len = versions.length; i < len; i++) {
			try {
				xhr = new ActiveXObject(versions[i]);
				break;
			}
			catch (e) {
			}
		}
	}

	return xhr;
}
```

이 프로그램을 만들면서 단위 테스트를 만들고자 노력중인데, `XMLHttpRequest`를 어떻게 처리할까 고민하며 찾아보았다. 찾아보다 보니 [Sinon.JS](http://sinonjs.org/)를 사용한 [Unit testing Ajax requests with Mocha](https://www.airpair.com/javascript/posts/unit-testing-ajax-requests-with-mocha)라는 글을 보게되어 사용해보았다.

> Standalone test spies, stubs and mocks for JavaScript.
No dependencies, works with any unit testing framework.

`Sinon.JS`는 위의 소개와 같이 테스트를 위한 spy, stub, 와 mock을 제공해주는 라이브러리이다. 필자는 `Sinon.JS`를 사용하여 `XMLHttpRequest`의 가짜 구현체를 `Node`의 `global.XMLHttpRequest`에 넣어 사용하였다.

아래는 상단의 코드에 대해 작성한 단위 테스트이다.

```javascript
beforeEach(() => {
    global.XMLHttpRequest = sinon.useFakeXMLHttpRequest();
    xhr = global.XMLHttpRequest;
    requests = [];

    xhr.onCreate = function (xhr) {
        requests.push(xhr);
    }.bind(this);
});

afterEach(() => {
    xhr.restore();
});

describe('ajax Test', () => {
    it('GET Method Test', (done) => {
        var data = {foo: 'bar'};
        var dataJson = JSON.stringify(data);

        var opts = {
            url: 'test.com',
            type: 'GET',
            success: (result, status) => {
                assert.equal(status, 200);
                assert.equal(result, dataJson);
                done();
            }
        };
        ajax.ajax(opts);
        requests[0].respond(200, {'Content-Type': 'text/json'}, dataJson);

        assert.isTrue(Number.isInteger(requests.length));
        assert.equal(requests.length,1);
        assert.equal(requests[0].method, 'GET');
    });
});
```