---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 17)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---

## Orthogonal basis

### 단위벡터(Unit Vector)
단위 벡터는 크기가 1인 벡터를 의미하며 다음과 같이 정의된다.

<center>$\vec{v}=\frac{\vec{v}}{||\vec{v}||}$</center>

즉, 해당 벡터에다가, 해당 벡터의 크기를 나눠주면 단위벡터(크기가 1인 벡터)가 되는 것이다.

또한, **크기를 1로 만드는 작업을 정규화(Normalization)이라고 하고, 그 벡터를 정규화 벡터라고도 부른다**
정규화는 서로 다른 스케일의 값 또는 벡터를 동일한 관점(스케일)에서 바라보기 위해 사용된다.

### 직교벡터(Orthogonal Vector) & 정규직교벡터(Orthonormal vector)
직교벡터란 서로 다른 두 벡터 $\vec{q_i}$와 $\vec{q_j}$가 존재할 때, ${\vec{q_i}}^T\vec{q_j}=0$을 만족하면, 두 벡터

$\vec{q_i}$와 $\vec{q_j}$를 직교벡터라고 한다. 이 때, $\vec{q_i}$와 $\vec{q_j}$의 **크기가 1** 이라면,

**두 벡터 $\vec{q_i}$와 $\vec{q_j}$ 을 정규직교벡터(Orthonormal Vector)라고 한다.**

---
## 직교행렬(Orthogonal Matrix)
어떤 행렬 Q가 다음과 같은 조건을 만족하면, 그 행렬을 직교행렬 이라고한다. 그 조건은 다음과 같다.

행렬의 row끼리의 내적이 0 & column끼리의 내적이 0 즉, 모든 행과, 모든 열은 서로 직교해야 하며,
**동시에, 모든 행과, 열의 크기는 1이어야 한다.**(이점을 생각하면 Orthonormal matrix가 아닌가 싶다..)

정의에 따라서, **$Q^TQ=I$** 임을 쉽게 파악할 수 있다.

그리고 만약에 **Q가 정방행렬 이라면, Q^T=Q^{-1}**을 만족하고, 그 결과 QQ^T=I 라고도 할 수 있다.**

### 그람-슈미트 과정(Gram-Schmidt Process)
그람 슈미트 과정이란 무엇인가? 간단히 말해서 정규직교기저를 만드는 과정이라고 보면 된다.

우리가 흔히 아는 데카르트 평면(x,y좌표계)가 수직평면 이듯이, 기저를 직교기저로 표현한다면 계산, 표현
상에서 많은 이점이 있다. 이런 정규직교기저로 변환하는 방법이 그람-슈미트 과정이다.

그람-슈미트 과정은 어렵지 않다. 대략적인 알고리즘은 다음과 같다.

만약 행렬A가 3x3 Matrix라 가정하였을 때, 행렬A의 세 열벡터를 $\vec{a}, \vec{b}, \vec{c}$라 하자.

과정1. $\vec{a}$ = $\vec{q_1}$ 이라 정의한다.

과정2. $\vec{q_2} = \vec{b}를 \vec{q_1}$과 수직인 벡터에 투영(projection)시켜 벡터

과정3. $\vec{q_3} = \vec{c}를 \vec{q_1}과 \vec{q_2}$모두에 수직인 벡터에 투영시킨 벡터이다.

이렇게 세 과정을 모두 반복하여 $\vec{q_1}$, $\vec{q_2}$, $\vec{q_3}$를 모두 구하면 그람-슈미트과정 종료다.

이제 이 과정을 자세히 살펴보자.

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img01.png" width="70%"></center>

위 그림을 살펴보면, 전에 설명했던 과정을 상상해볼 수 있을 것이다. 이제 수식의 관점에서 살펴보자.

$\vec{e} = \vec{b} - \vec{p}$ 인 것을 그림을 통해서 쉽게 알 수 있다. [Lecture15-16](https://sunshower76.github.io/mathematics/2019/07/30/Linear-algebra-lecture15-16/)을 통해서, $\vec{p}=\frac{\vec{a}\vec{a}^T}{\vec{a}^T\vec{a}}\vec{b}$라는 것을 배웠다.

그러면 다음과 같이 다시 쓸 수 있다.

$\vec{e} = \vec{b} - \frac{\vec{a_1}\vec{a}^T}{\vec{a}^T\vec{a}}\vec{b}$

그리고, 위 알고리즘에서 설명했듯이 바로 $\vec{e} = \vec{q_2}$이다.

$\vec{q_2} = \vec{b} - \frac{\vec{a}\vec{a}^T}{\vec{a}^T\vec{a}}\vec{b}$

그렇다면 이제 $\vec{q_3}$는 어떻게 구할까?

먼저 $\vec{c}$를 $\vec{q_1}$에 $\vec{q_2}$를 구하는 과정을 수행한다.

$\vec{e_{c1}} = \vec{c} - \frac{\vec{a}\vec{a}^T}{\vec{a}^T\vec{a}}\vec{c}$ 의 결과가 나올것이다.

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img02.png" width="70%"></center>

그러면 위 그림과 같은 형태가 나올 것이다. 여기서 봐야할 점은, $\vec{q_2}$와 $\vec{e_{c1}}$가 모두

**$\vec{q_1}$에 수직하다는 것이다. 그렇다면, $\vec{q_2}$와 $\vec{e_{c1}}$를 기저로 하여 만들어진 평면상에 존재하는**
** 모든 벡터들은 $\vec{q_1}$에 수직할 것이라는 것을 알 수 있다.**

$\vec{e_{c1}}$를 $\vec{q_2}$에 투영하여 나온 벡터$\vec{p_{c1}}$은 그림과 같이 표시할 수 있다. 

그리고 최종적으로, $\vec{e_{c1}}$를 황색 점선벡터에 투영하면 그림은 다음과 같이 나온다.

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img04.png" width="70%"></center>

이렇게 되면 최종적으로 수직인 세 벡터가 나오게 된다. 이때, $\vec{q_3}$와$\vec{q_1}$이 직교하는 이유는 위에 굵은글씨
로 언급했던 것 처럼 $\vec{q_3}$는  $\vec{q_2}$와 $\vec{e_{c1}}$를 기저로 하여 만들어진 평면상에 존재하는 벡터이기 때
문이다.

위 과정의 계산 과정은 다음과 같다.

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img05.png" width="50%"></center>

최종적인 결과는 다음과 같이 요약된다.
<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img06.png" width="40%"></center>

그리고 각 벡터의 크기로 각각 나누어 orthonormal vectors로 만들면 그람-슈미트 과정이 종료된다.

결과를 보면, 패턴이 보이기 때문에, 일일히 구하는게 아니고, 공식을 외워서 사용하면 된다.

### QR분해(QR decomposition, factorization)
QR분해란 A=QR의 형태로 나타내는 것이다. 여기서 Q는 직교행렬, R은 상삼각행렬을 나타낸다.

Q는 그람슈미트 과정을 이용하여 정규직교벡터로 이루어진 직교행렬을 의미한다.

R은 다음 과정을 통해서 구할 수 있다.

$A=QR$은, **A가 정방행렬일때, $Q^T=Q^{-1}$이므로** $Q^TA=R$로 표현할 수 있다.

(**Q가 정방행렬이 아니라면, $Q^{-1}대신, Q^†=(Q^TQ)^{-1}Q^T을 곱해준다.$**)

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img08.png" width="80%"></center>

여기서, R이 상삼각행렬인 이유는 다음 그림을 보면서 생각해보자

<center><img src="/public/img/2019-07-31-linear algebra-lecture17/img07.png" width="70%"></center>

그림에서, $\vec{b}$는 $\vec{q_1}$과 $\vec{q_2}$의 선형 결합으로 만들어 질 수 있다. 즉, 같은 평면에 존재한다. 그런데, $\vec{q_3}$는 $\vec{q_1}$과 $\vec{q_2}$에 모두 수직하므로, 해당 평면에 수직하다고 볼 수 있으며, 그렇게 된다면, $\vec{q_3}$는 해당평면에 존재하는 모든 벡터와 수직하므로, $\vec{b}$와도 수직하다.

**이런 식의 원리를 모두 적용시키면, $\vec{q_i}$는 $\vec{a_j}$에 대해서, i>j인 경우, $$\vec{q_i}$\vec{a_j}=0$이 된다.** 이러한 원리로 R은 상삼각행렬이 성립된다.

QR분해는 총 세가지 방법이 있다고한다. Gram-Schmidt 방법, Givens rotation 방법, householder reflection 방법이 있다.

자세한 방법은 다음에 다루도록 하겠다.










