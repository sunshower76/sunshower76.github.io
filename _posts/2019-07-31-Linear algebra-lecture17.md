---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 15)
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
## Orthogonal Matrix





