---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 5)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 PA=LU, Vector space & Sub-space
에 대해서 학습한다.

---
## P : Permutation matrix
### PA=LU (LU decomposition with permutation)
P : Permutations - execute row exchanges
갑자기 여기서 P는 왜 언급이 된 것일까? 그것은 바로,
그냥 A를 이용하여 가우스 소거법을 진행했을 때, 중간 피벗이
0이 되는 경우가 발생할 수 있기 때문이다.
그렇게 된다면, 0이 되지 않는 행과 위치를 바꿔줘야한다.(row exchange)
이때 사용되는 행렬이 P(Permutation matrix)이다.
PA는 즉, 행 교환이 일어난 A행렬을 의미한다. 이렇게 행을 바꾸고 난 후,
LU분해를 진행하여 나온 식이 ** PA = LU **이다.

### The number of permuation matrix
Permutation matrix는 [Linear algebra-Lecture 2](https://sunshower76.github.io/mathematics/2019/07/02/Linear-algebra-lecture2/)
에서 언급했었다.
Permutation matrix는 행 또는 열의 위치를 서로 바꿔주는 행렬을 의미한다.
nxn의 permutation matrix는 몇 개가 존재할까?

정답은 바로 n!개 이다.(세 사람이 순서가 있는 세 자리에 앉는 경우의 수와 동일하다.)

3x3행렬의 예제를 살펴보자.
<center><center><img src="/public/img/2019-07-04-linear algebra-lecture5/img1.gif" width="50%"></center></center>

4x4행렬은 4! 이므로, 24개가 나올것이다. 시간이 있다면 직접 한 번 해보자.

### Property of permutation matrix
** 또한, permutation matrix는 다음과 같은 중요한 성질을 만족한다. **
<center>$P^{-1}=P^T$ -> $P^TP=I$</center>

---
## S : Symmetric matrix
### Symmetric matrix
**Symmetric matrix(대칭행렬)**은 주 대각선을 기준으로 마주보는 원소가 같은 행렬이다.
식으로 표현하면 다음과 같다.
<center>$A_{ij}=A_{ji}$</center>

그리고 위 식은 다음과 같은 성질을 만족함을 의미한다.
<center>$A=A^T$</center>

### Properties of symmetric matrix
1. $A=A^T$
2. $AA^T = S$

proof

>$(A^TA)^T = (A^T)(A^T)^T
(A^T)(A^T)^T=(A^T)A
이므로 대칭행렬의 성질인 1번 성질을 만족.

---
## Vector spaces








