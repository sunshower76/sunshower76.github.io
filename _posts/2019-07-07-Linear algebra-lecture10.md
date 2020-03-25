---
layout: post
title: Linear Algebra - 10. Four fundamental subspace(row,comlumn space and null spaces)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 다음에 관해서 배운다.
- Four fundamental subsapce(for matrix A)
: 4개의 subspace는 다음을 말한다.

1. rowspace C($A^T$) in $R^n$
2. left nullspace N($A^T$) in $R^m$
3. columns space C(A) in $R^m$ (생략)
4. nullspace N(A) in $R^n$ (생략)

3번과 4번은 이미 다뤗기 때문에 생략한다.
---
## 4 sub spaces
<center><img src="/public/img/2019-07-07-linear algebra-lecture10/img01.png" width="70%"></center>

### 1. rowspace C($A^T$) in $R^n$
바로 예시를 살펴보자

<center><img src="/public/img/2019-07-07-linear algebra-lecture10/img02.png" width="30%"></center>

A라는 행렬에서 R을 구했다.

이때, C(A) = C(R)인가? 답은 **No**이다. 같지않다!

왜일까?

왜냐하면, 우리는 R을 구할 때, **행 연산(Row operation)**을 했기 때문에,

**행 공간은 보존**되지만, 열공간은 보존되지 않기 때문이다.

그렇다면 여기서 행렬A의 행공간의 기저는 무엇일까?

R의 pivot row인, 1행 2행이다.

---
### 2. left nullspace N($A^T$) in $R^m$

우선 말해두지만, 기존의 Null space를 구하는 방법 그대로 해도된다.

다만, A를 $A^T$로 바꾼다음 같은 과정을 거치는 것이다.

이 강의에서는 다른 더 좋은 접근 방법을 알려준다.

<center><img src="/public/img/2019-07-07-linear algebra-lecture10/img03.png" width="50%"></center>
위 그림의 과정까지 이해가 가는가?

그렇다면 여기서 방법을 한 번 생각해보자!

힌트는 위에 나왔던 R을 이용하는 방법이다.

바로 $y^T$를 A를 R로 변환시키는 변환행렬로 보는 것이다.

이렇게 보는 이유는 무엇일까?

그 이유는 바로 R(Reduced row echelon form)에는, 행 전체가 0인 행이 있기 때문이다.

다음 예제를 살펴보자.

<center><img src="/public/img/2019-07-07-linear algebra-lecture10/img04.png" width="50%"></center>

E의 3행을 보면, 행렬 A에 대해서, 모든 연산을 수행시 0 이라는 값을 내뱉는 모습을 볼 수 있다.

즉, 3행이 left null space의 기저인 것이다.

### 3. 요약($C(A^T) & N(A^T)$는 R부터 구하자!)
1. 행공간의 기저는 R의 pivot row이다.

2. left nullspace의 기저는, EA=R을 만족시키는 E에서, R행렬의 $row_n(R)=0$을 만족시키게 하는 행이다.

그러므로, left nullspace기저의 개수는, R에서 0인 행의 개수가 될 것이다.


