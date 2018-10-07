---
layout: post
title: '[Coding The Matrix] 벡터의 생성과 동차 선형시스템의 해집합의 관계에 대한 예제'
date: 2018-09-30 22:20:00
categories: math
tags: [math, linear algebra]
published: true
fullview: false
comments: true
---

Coding The Matrix 4장(원서 3장) 벡터공간에서 원점을 포함하는 flat의 다음 두 가지 표현에 대해 설명한다.

* 어떤 벡터들의 생성으로서
* 동차 선형시스템의 해집합으로서

이 글에서 작성하고자 하는 내용은 이 표현들의 설명은 아니다. 단지, 책에서 이 두가지 표현에 대한 예제가 나오는데 해당 예제에 대해서 스터디에서 많은 의견이 오고가서 이에 대해 정리하고자 한다. 두 예제는 아래와 같다.

---

* Example 4.3.7

평면

$$
Span \{[1, 0, 1.65], [0, 1, 1]\}
$$

은 다음과 같이

$$
\{(x, y, z) \in R^3 : [1.65, 1, -1] \cdot [x, y, z] = 0\}
$$

우변이 영인 선형방적식의 해집합으로 나타낼 수 있다.

---

* Example 4.3.10

직선

$$
Span \{[-1, -2, 2]\}
$$

은 다음과 같이

$$
\{(x, y, z) \in R^3 : [4, -1, 1] \cdot [x, y, z] = 0, [0, 1, 1] \cdot [x, y, z] = 0\}
$$

두 동차 선형방정식 쌍들의 해집합으로 나타낼 수 있다.

---

Example 4.3.7의 도출은 $ax+bx+cz=0$에 대입하여 연립방정식을 정리하면 쉽게 도출할 수 있었다. 하지만, Example 4.3.10의 경우는 저 두 평면이 교차하는 직선(두 동차 선형방정식의 해집합)이 생성과 동일하다는 것만 이해되고 어떠한 기준에서 두 동차 선형방정식이 도출된 것인지 알 수 없었다.

우선 Example 4.3.7의 경우, 생성과 해집합 모두 깔끔하게 아래와 같은 평면으로 표현할 수 있어서 이해하기 쉬웠다.

<figure><img src="/images/math/ctm/ctm-example-4.3.7.png" alt=""></figure>

* [Example 4.3.7 - GeoGebra](https://www.geogebra.org/3d/juu67nkr)

Example 4.3.10의 경우에도 각각 생성과 해집합이 아래와 같은 두 평면이 교차하는 직선인 것은 쉽게 이해할 수 있었다.

<figure><img src="/images/math/ctm/ctm-example-4.3.10.png" alt=""></figure>

* [Example 4.3.10](https://www.geogebra.org/3d/hvzctaes)

계속 의문을 가졌던 것은 위 생성에서 어떻게 두 동차 선형방정식을 도출할 수 있을까였다. 계속 이런 저런 이야기를 하다보니, 평면이 교차해서 생기는 직선은 무수히 많을 수 있다는 것을 간과하고 있다는 것을 Ro달았다. 예제에서 든 두 동차 선형방정식 외에도 얼마든지 다른 방정식의 쌍을 예로 들어도 되었을 것이다.

덧붙여서 이와 관련된 검색을 하던 도중에 흥미로운 StackExchange에 올라온 [How to turn span into linear equality constraint?](https://math.stackexchange.com/questions/1505789/how-to-turn-span-into-linear-equality-constraint)글의 질문과 답변을 보았다.

필자는 Example 4.3.7를 도출하기 위해 $ax+bx+cz=0$에 대입하여 연립방정식으로 정리하였는데, 위 질문에서는 이를 행렬로 정리하는 것을 보았다.

 $e_1=\begin{bmatrix}1\\\0\\\1.65\end{bmatrix}$ 와 $e_2=\begin{bmatrix}0\\\1\\\1\end{bmatrix}$ 의 생성에 놓여있는 $u=\begin{bmatrix}x\\\y\\\z\end{bmatrix}$ 는 $u=\lambda e_1+\mu e_2$를 만족할 것이다.

 이때 아래와 같이 가우스 정리를 통해 정리하면,

 $$
 \begin{bmatrix}1&0&x\\0&1&y\\1.65&1&z\end{bmatrix}
 \rightsquigarrow
 \begin{bmatrix}1&0&x\\0&1&y\\0&1&z-1.65x\end{bmatrix}
 \rightsquigarrow
 \begin{bmatrix}1&0&x\\0&1&y\\0&0&z-1.65x-y\end{bmatrix}
 $$

마지막 행의 마지막 요소인 $z-1.65x-y$은 우변인 $0$을 만족하여야한다.

$$
z-1.65x-y=0\ (-1을 곱하여 정리하면)\\1.65x+y-z=0
$$

위와 같이 가우스 소거법을 사용하여 정리하면 좀 더 계산하기 쉽게 동차 선형방정식을 도출할 수 있다. 특히 Example 4.3.7와 달리 더 높은 차원에서 동차 선형방정식을 구해야할 경우에 유용할 것 같다.