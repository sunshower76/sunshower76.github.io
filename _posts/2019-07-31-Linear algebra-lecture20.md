---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 20)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---

## Inverse matrix
[Lecture 19](https://sunshower76.github.io/mathematics/2019/07/31/Linear-algebra-lecture19/)에서 **여인수(cofactor)**에
대해서 배웠다면 역행렬을 구하는 방법을 아는 것은 매우 간단하다.

먼저 2x2행렬의 예제에서 살펴보고, 3x3행렬의 예제를 보자.

<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img01.png" width="40%"></center>
그림을 보면 알 수 있듯이, 여인수 행렬의 **전치행렬(수반행렬)**에다가 A의 행렬식의 역수를 곱해준 꼴임을 알 수 있다.

이러한 방법을 적용하여 3x3행렬의 행렬식을 살펴보면 다음과 같다.

<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img02.png" width="70%"></center>

그런데 그렇다면, 이 역행렬을 구하는 공식은 어디서부터 나온 것일까?

다음 식을 살펴보자.
<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img03.png" width="30%"></center>
<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img04.png" width="70%"></center>

위의 두 공식이 성립하고, 위 두 공식으로 부터, A의 역행렬 공식이 도출되었음을 알 수 있다.

## Cramer's Rule (크레머 공식)
크레머 공식은 Ax=b 라는 식이 있을때, 해당 식의 해를 행렬식을 이용하여 표현하는 방법이다.

다음 그림을 보면서 과정을 이해해보자

<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img05.png" width="30%"></center>

<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img06.png" width="70%"></center>

<center><img src="/public/img/2019-07-31-linear algebra-lecture20/img07.png" width="70%"></center>


출처 : https://twlab.tistory.com/43?category=668741
