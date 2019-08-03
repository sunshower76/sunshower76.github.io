---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 29)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---

## 특이값 분해(Singular Value Decomposition, SVD)
<center> $A = U\Sigma V^{T}, \Sigma =diagonal, U,V=Orthonormal$  <center>

특이값 분해의 공식은 다음을 의미한다.

$A = U\Sigma V^{T}$

$AV= U\Sigma$

여기서 V를 행렬A의 row space의 정규직교벡터, U를 행렬A의 column space의 정규직교벡터라고 하자.

그렇다면, **특이값분해는 행렬A의 row space를 행렬A로 선형변환시켜, 행렬A의 column space의 직교벡터로 사상시키는** 
**역할을 하는 것이다**. 여기서 행렬A의 row space의 정규직교벡터를 행렬A의 column space의 직교기저로 사상시킨다면, 그 벡터의 크기가 1이 아니기 
때문에, \Sigma는 이를 정규직교로 변환해주는 Scale factor의 역할을 한다.


그렇다면 SVD의 과정을 살펴보자.

<center><img src="/public/img/2019-08-03-linear algebra-lecture29/img02.png" width="100%"></center>

위의 설명과 같이, $V\Sigma^2 V^T$와 $U\Sigma^2 U^T$는 대칭행렬($A^A, AA^T$)가 대각화되었으므로, 

**V는 행렬A의 row space의 정규직교이면서, $A^TA$의 고유벡터인 벡터들로 이루어진 행렬**이고,

**U는 행렬A의 column space의 정규직교이면서, $AA^T$의 고유벡터인 벡터들로 이루어진 행렬**이다.

**그리고, $AA^T$와 $A^TA$의 고유값은 동일하므로, \Sigma 는 동일하다.**

이제 예제를 하나 살펴보자.
<center><img src="/public/img/2019-08-03-linear algebra-lecture29/img05.png" width="30%"></center>
<center><img src="/public/img/2019-08-03-linear algebra-lecture29/img01.png" width="70%"></center>

<center><img src="/public/img/2019-08-03-linear algebra-lecture29/img03.png" width="70%"></center>
원래는 $A^TA, AA^T$모두를 계산하여, 고유벡터를 계산하여 $U,V$를 구해야 하지만 여기서는 특별히, 행렬A의 
row space와 column space가 하나의 벡터로 구성되있기 때문에, 위 설명과 같이 단순하게해서 구했다.

최종적으로는 다음과 같은 결과가 나온다.
<center><img src="/public/img/2019-08-03-linear algebra-lecture29/img04.png" width="70%"></center>






