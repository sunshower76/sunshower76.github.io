---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 25)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---

## 대칭행렬(Symmetric matrix)
예전에 대칭행렬에 대해서 배운 적이 있었다. 대칭행렬이란 무엇인가? 다음과 같은 조건을 만족하는 행렬을 
대칭행렬 이라고 한다.

>조건
1. $A=A^T$
2. 정방행렬

[Lecture 22](https://sunshower76.github.io/mathematics/2019/07/31/Linear-algebra-lecture22/)에서 배웠던 
대각화를 생각해보자. 대각화는 고유벡터로 이루어진 행렬과, 고유값의 대각행렬로 하나의 행렬을 분해하는 과정이었다. 
이때, 대칭행렬은 특별한 성질을 가지고 있기 때문에, **대칭행렬을 대각화**할 때도 이러한 성질이 돋보인다.

>대칭행렬의 성질
1. 대칭행렬의 모든 고유값은 **실수**이다.
2. 대칭행렬의 서로다른 고유벡터는 서로 **직교**한다.

위와 같은 성질때문에, 대칭행렬을 대각화 할 때, 다르게 쓸 수 있다.

$A=Q\Lambda Q^{-1}$

$=Q\Lambda Q^T$

왜냐하면, 고유벡터는 서로 직교하기 때문에, 고유벡터로 이루어진 행렬 Q는 직교행렬이기 때문이다. 직교행렬의 성질을 
다시 기억해보면, $QQ^T=QQ^{-1}=I$라는 성질이 기억날 것이다.

## 스펙트럼 정리(Spectral theorem)
이제는 스펙트럼 정리에 대해서 배워볼 것이다. 스펙트럼 정리를 배우기 앞서 왜 대칭행렬을 이야기했을까? 그 이유는 
대칭행렬과 스펙트럼 정리가 연관이되어있기 때문이다. **대칭행렬의 대각화는 스펙트럼 정리의 특수케이스** 라고 볼 수 있다.

**스펙트럼 정리(Spectral Theorem)** 이란 nxn크기의 정방행렬인 에르미트 행렬(Hermitian matrix)를 고유값으로 
이루어진 대각행렬과 유니터리 행렬(Unitary matrix)로 대각화 할 수 있다는 것이다.

<center>$A=U\Lambda U^{H}$</center>

### 에르미트 행렬(Hermitian Matrix)
그렇다면 위에서 설명한, 에르미트 행렬(Hermitian matrix)란 무엇인가?
<center><img src="/public/img/2019-08-01-linear algebra-lecture25/img01.png" width="40%"></center>

위와 같은 성질을 만족하는 행렬을 에르미트 행렬이라고 한다. 말로 풀어쓰면, 

**대각 원소는 모두 실수(Real value)** 이면서, **자기자신과 켤레전치행렬이 같은** 행렬 을 의미한다.

### 유니터리 행렬(Unitary Matrix)
유니터리 행렬(Unitary matrix)은 다양한 방법으로 표기된다.
<center>$U^{H}=U^{*}=U^{†}$</center>

즉, 위첨자로 표시된 부분이 해당 행렬을 **켤레전치행렬**로 만든다는 표시이다.

유니터리 행렬은 다음과 같은 성질을 지닌다.
<center><img src="/public/img/2019-08-01-linear algebra-lecture25/img02.png" width="60%"></center>

즉, 유니터리 행렬은 **직교행렬** 이라는 뜻이다.


**대칭행렬의 대각화가 스펙트럼 정리의 특수한 케이스라고 설명한 이유는, 대칭행렬은 에르미트 행렬의 허수부가 0인 경우이기 때문이다.**

## 대칭행렬 & 대각화 & 투영행렬(Symmetric matrix & Diagonalization & Projection matrix)
A라는 대칭행렬이 있을때, A를 대각화 시키면, 다음과 같은 표현으로 쓸 수 있다.
<center><img src="/public/img/2019-08-01-linear algebra-lecture25/img03.png" width="80%"></center>

여기서 **식10.3**을 주목하자! 식10.3은 $\lambda$라는 상수와, $qq^T$의 곱의 합으로 이루어진 모습을 볼 수 있는데, $qq^T$는 우리가 [Lecture15](https://sunshower76.github.io/mathematics/2019/07/30/Linear-algebra-lecture15-16/)에서 배웠듯이 **투영행렬**이라는 것을 알 수 있다.

<center>즉, **nxn 대칭행렬 A는 n개의 투영행렬과 그 고유값의 조합으로 표현**이 가능하다는 것이다!!!!</center>

그리고, $q_1, q_2, ..., q_n$은 서로 직교하는 벡터이므로, 최종적으로 정리하면,

**nxn크기의 대칭행렬 A는 상호수직인 투영행렬들의 그 고유값의 조합으로 표현이 가능하다** 라고 볼 수 있다.

만약에, $A\vec{v}$라는, 벡터v를 대칭행렬A로 선형변환 시키는 식이 있을때, 그 결과는, 다음과 같다.
<center><img src="/public/img/2019-08-01-linear algebra-lecture25/img04.png" width="100%"></center>

즉, 벡터v를 대칭행렬A의 고유벡터에 투영시킨 벡터와 그 고유벡터에 대응하는 고유값을 곱한 항들의 합이 벡터v를 대칭행렬A로 선형변환 시킨 결과이다.









