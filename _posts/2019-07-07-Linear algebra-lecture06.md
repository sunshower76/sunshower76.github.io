---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 06)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 다음에 대해서 배운다.
- Column space of A : Solving Ax=b
- Nullspace of A

---
## Column space of A : Solving Ax=b
다음 예제에 대해서 생각해보자.
<center><img src="/public/img/2019-07-07-linear algebra-lecture06/img1.gif" width="25%"></center>
위 예제의 Column space는 어떻게 표시되는가?
Column space of A=$C(A)$=All linear combnations of columns of A 이다.

그렇다면 여기서 질문을 하나 던져보자.

**Ax=b의 식에서, A는 모든 b에 대해서 해를 갖는가?**

답은 **No**이다. 그 이유에 대해서 알아보자.

<center><img src="/public/img/2019-07-07-linear algebra-lecture06/img2.gif" width="50%"></center>

위 관계식 보았을 때, 어떤 b의 값들이 위 관계식의 해를 갖게 할까?

답은 **b가 A의 Column들의 linear combination으로 이루어지는 것이다.**

쉽게 말해서 b=$c_1Col_1(A) + c_2Col_2(A) + c_3Col_3(A)$로 표현되면 된다.

그렇게 된다면, $x_1=c_1, x_2=c_2, x_3=c_3$가 될 것이다.

<center><img src="/public/img/2019-07-07-linear algebra-lecture06/img3.gif" width="50%"></center>
위 예시에서 해는 무엇을까? x=(1,1,0) or (0,0,1) or (2,2,-1) 등 무수히 많을 것이다.

그렇다면, X=c_1(1,1,0) + c_2(0,0,1) 단 (c_1=c_2=0 제외) 로 나타낼 수 있다.

그런데 이 방정식의 해 중에서 x=(0,0,0)이 포함되지 않으므로, 
X는 공간을 만든다고 볼 수없고, 단지, 두 벡터로 이루어진 평면을 만들 뿐이다.

---
## Nullspace of A
**Nullspace(영공간)**이란 무엇일까?

Nullspace of A는 $AX=0$를 만족시키는 모든 X의 선형결합이 만드는 공간이다.
<center><img src="/public/img/2019-07-07-linear algebra-lecture06/img4.gif" width="50%"></center>

이때 X=(0,0,0), (1,1,-2), (2,2,-4) ...이다. ( (0,0,0)은 항상 해가 된다. )

그리고 X = c(1,1,-1)로 표현될 수 있다.(c는 임의의 상수)

또한, AX=b의 경우와 다르게, x=(0,0,0)이 위 방정식의 해 중에서 존재하므로, 
x는 선 모양의 공간을 형성한다고 할 수 있겠다.
