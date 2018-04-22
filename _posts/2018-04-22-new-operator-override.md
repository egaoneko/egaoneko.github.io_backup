---
layout: post
title: 'new 생성자 override'
date: 2018-04-22 18:30:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

```javascript
function A () {
  this.name = 'A1';
  this.prop = 123;
  return {
    name: 'A2',
    prop: 456
  };
}
let a = new A();
```

JS에서 생성자 함수를 통해 `new` 호출할 때, 명시적으로 객체를 반환한다면 해당 객체로 재정의(override)하게 된다. 위 코드의 결과를 결과를 보면 변수 `a`에 할당된 객체는 `A` 생성자 함수에서 반환한 `{name:"A2", prop: 456}` 객체이다.

```javascript
function B () {
  A.call(this);
  this.name = 'B1';
}
B.prototype = Object.create(A.prototype);
B.prototype.constructor = B;
let b = new B();
```

만약 위 코드와 같이 생성자 함수 `A`를 상속하는 생성자 함수 `B`가 있을때, 변수 `b`에 할당된 객체는 생성자 함수 `A`에서 반환된 객체와 관계없이 `{name:"B1", prop:123}`이다. 생성자 함수 `A`에서 생성될 때 `name:"A2"`와 `prop:123`가 할당되고, 생성자 함수 `B`에서 생설될 때 `name:"B1"`이 재정의 된다.

```javascript
class A {
  constructor() {
    this.name = 'A1';
    this.prop = 123;
    return {
      name: 'A2',
      prop: 456
    };
  }
}
let a = new A();
```

ES2015에 도입된 `class` 키워드를 사용해서 `A`를 생성하면 위에서 보았던 것과 같이 변수 `a`에는 반환된 객체인 `{name:"A2", prop: 456}`가 들어있다.

``` javascript
class B extends A {
  constructor() {
    super();
    this.name = 'B1';
  }
}
let b = new B();
```

반면 `Prototype` 상속을 통해 상속하였던 것과 마찬가지로 `class`의 `extends` 키워드를 사용하여 상속하여 반환값을 비교해보면 `{name: "B1", prop: 456}`로 `Prototype` 상속과 다른 값이 할당되어 있는 것을 볼 수 있다. `A`에서 반환된 객체가 부모가 되고, `B`에서 할당한 값들이 재정의되는 것에서 차이점이 있다.

ES2015에 도입된 `class` 키워드가 단순히 문법적 양념(syntac sugar)라고 생각해왔는데, 위와 같이 일부 다르게 동작되는 부분이 있다는걸 알 수 있었다. 새롭게 추가되는 스펙들에 대해서 좀 더 면밀히 알아봐야됨을 느꼈다.

## Reference

* [new operator - MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new)
* [상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)