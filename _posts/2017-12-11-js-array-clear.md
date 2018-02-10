---
layout: post
title: 'Array clear'
date: 2017-12-11 23:30:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

## 들어가며

```javascript
let A = [1, 2, 3, 4];
A = [];
```

배열을 단순하게 상단과 같이 초기화하곤 했었는데 아래와 같은 코드 리뷰를 받았다.

```javascript
let A = [1, 2, 3, 4];
A.length = 0;
A = [];
```

리뷰를 받고 대충 "A에 대한 모든 값을 날려주니 깔끔하게 지워지겠네."라고 생각이 들었지만, 문든 배열의 초기화에 대해 제대로 찾아본 적은 없어 검색을 해보았다.

검색을 해보니 Stack Overflow에 Philippe Leybaert가 [How do I empty an array in JavaScript?](https://stackoverflow.com/questions/1232040/how-do-i-empty-an-array-in-javascript)라는 질문에 좋은 답변을 한 것이 있어 정리해본다. 이 글에서는 배열에 대한 4가지의 초기화 방법에 대해 제시하고 비교한다.

## 방법 1

```javascript
A = [];
```

변수 `A`에 빈배열을 할당함으로써 기존배열을 지운다. 만약 변수 `A`가 참조한 배열에 다른 참조가 없다면 가장 빠른 해결책이 될 것이다.

```javascript
var arr1 = ['a','b','c','d','e','f'];
var arr2 = arr1;  // Reference arr1 by another variable 
arr1 = [];
console.log(arr2); // Output ['a','b','c','d','e','f']
```

만약 위와 같이 다른 참조가 남아있다면, 이 배열은 제거되지 않고 남아있는 문제가 발생할 것이다.

## 방법 2 (as [suggested](https://stackoverflow.com/a/1234337/113570) by [Matthew Crumley](https://stackoverflow.com/users/2214/matthew-crumley))

```javascript
A.length = 0
```

배열의 길이를 0으로 설정하여 기존배열을 지운다. 일부 JavaScript 구현체에서 작동하지 않을 수 있다고 주장하는 사람들도 있다. ECMAScript5에서 `"strict mode"`할때도 배열의 길이는 읽기/쓰기 속성이므로 잘 작동한다.

## 방법 3 (as [suggested](https://stackoverflow.com/a/8134354/113570) by [Anthony](https://stackoverflow.com/users/1047275/anthony))

```javascript
A.splice(0,A.length)
```

`.splice()`를 사용하면 완벽하게 작동한다. `.splice()`함수는 제거 된 항목이 모두있는 배열을 반환하여 실제로 원래 배열의 복사본을 반환하겠지만, 벤치마킹을 해보면 이는 성능에 아무런 영향이 없다고 한다.

## 방법 4 (as [suggested](https://stackoverflow.com/a/17306971/113570) by [tanguy_k](https://stackoverflow.com/users/990356/tanguy-k))

```javascript
while(A.length > 0) {
    A.pop();
}
```

이 방법은 간결하지 못하며, 앞선 방법들보다 벤치마크시 성능이 좋지 못하다.

## 성능

<figure><img src="/images/javascript/js-array-clear-benchmark.png" alt=""></figure>

[이 벤치마크](http://jsben.ch/hyj65)에 따르면 기존 배열을 지우는 방법 중에서 방법 2와 3은 성능면에서 매우 유사하며 방법 4보다 빠른 성능을 보여준다.

## 마치며

원본 배열에 대한 다른 참조가 없다면 방법 1이 가장 성능이 좋겠지만, 참조 여부를 신경쓰지 않고 배열을 확실하게 초기화하고자 한다면 방법 2나 3을 사용하면 될 것 같다.
