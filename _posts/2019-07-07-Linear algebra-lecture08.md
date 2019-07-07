---
layout: post
title: Linear Algebra - Gilbert Strang (Lecture 08)
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 다음에 관해서 배운다.
- Complete solution of Ax=b ($x=x_p + x_n$)
- Rank 'r' about solution

(r=m : Solution exsits, r=n Solution is unique)

---
## Complete solution of Ax+b($x=x_p + x_n$)
Complete solution이란 무엇일까?

한 마디로 정리하면 **Ax=b에 대한 일반해(general solution)**이라 할 수 있다.

[Linear algebra-Lecture 07](https://sunshower76.github.io/mathematics/2019/07/07/Linear-algebra-lecture07/)에서 
Ax=0에 대한 일반해를 구하는 법은 배웠지만, [Linear algebra-Lecture 06](https://sunshower76.github.io/mathematics/2019/07/07/Linear-algebra-lecture06/)에서 
Ax=b에 대해서 배울 때에는, 일반해를 구해는 보았지만, 명확히 구하는 방법에 대해서는 배우지 않았다.

그러므로, 앞으로 이번 강의에서는 Ax=b에 대한 일반해를 구하는 방법을 배울 것이다.

ex)

<center>x_1 + 2x_2 + 2x_3 + 2x_4 = b_1</center>

<center>2x_1 + 4x_2 + 6x_3 + 8x_4 = b_2</center>

<center>3x_1 + 6x_2 + 8x_3 + 10x_4 = b_3</center>

위 식을 Augmented matrix의 형태로 표현하면 다음과 같다.
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img01.png" width="25%"></center>

[Linear algebra-Lecture 06](https://sunshower76.github.io/mathematics/2019/07/07/Linear-algebra-lecture06/) 배웠던 내용을 생각해보자.
Ax=b가 해를 갖기 위한 b의 조건은 무엇일까?

전에 배웠듯이, **b가 A의 열공간(C(A))에 속해있으면 Ax=b는 해를 갖는다.**

또는, A의 모든 행의 선형결합이 0이고, b를 A와 같은 계수로 선형결합을 시켰을 때 0이면 된다.

(두 말은 같은 말이다. 모르겠으면 식을 전개해보자.)


### Steps of finding complete solution to Ax=b
원리는 간단하다.

<center>$  Ax_p=b$</center>
<center>$ +Ax_n=0$</center>
<center>$->A(x_p + x_n)=b$</center>

이게 원리 전부이다.

#### 1. $x_{particular}$ : 모든 free varaible=0로 두고, Ax=b를 푼다.

(Pivot varaible에 대해서만 방정식을 푼다고 생각해도 되겠다.)

ex)

<center>$x_1 + 2x_3 = 1$</center>

<center>$2x_3 =3$</center>


<center>-> $x_1=-2, x_3=3/2$</center>

<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img02.png" width="25%"></center>

#### 2. $x_{nullspace}$
다시 한 번 Nullspace를 구하는 과정을 복습해보자.
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img03.png" width="50%"></center>
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img04.png" width="50%"></center>

#### 3. Combine x_p with x_n
최종적인 일반해는 다음과 같다.
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img05.png" width="50%"></center>

---
## Rank 'r' about solution
m by n 형태를 가진 행렬A가 rank가 r이라고 하자.

### 1. r<n, r<m
우리가 여태까지 접했던 가장 일반적인 경우이다.

방정식에 비교해보면, 이런 맥락이다.

방정식이 5개, 미지수가 5개였는데, 다른 방정식을 가지고, 2개의 방정식을 만들 수가 있어서,
3개의 방정식과, 5개의 미지수만 남은 상황이다. 이때는 당연히 비 밖에 나오지 않으므로 무수히
많은 해를 지닐 수 밖에 없다. 아니면 완벽히, 미지수가 제거 되지 않아, 해가 없을 수도 있다.


### 2. r=n (n<m) : no free varaibles
잘 생각해보면 r=n일 때, rank가 full이라는 것을 알 수 있다.

앞에서 배운것 같이, Null space의 기저(biases)의 개수는 free varaibles의 개수와 같다.
그런데, free varaible이 존재하지 않으므로, $N(A)={zero vector}$이고,
$X=x_p$라고 할 수 있다. 이 때, x는 unique solution을 갖는다.

**unique solution**은 해가 존재하지 않거나, 해가 1개만 존재하는 것을 의미한다.

모르겠으면 다음 중간까지 풀이 과정을 보고 생각해보자.
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img06.png" width="30%"></center>

위 경우는, 중학교때 배운 방정식의 관점에서 생각해보면ㅡ 다음과 같은 경우이다.

방정식이 5개가 있고, 구하려는 미지수가 3개가 있다. 이 때, 2개의 방정식이 제거되고,
나머지 세 방정식은 서로에 대해서 모두 독립이다. 그럴 때에는, 해가 딱 1개만 있다.
하지만, 1번 경우와 같이, 방정식 제거 도중, 미지수가 완전히 제거되지 않을 때, 해가 아예 없다


### 3. r=m (n<m) : n-r(n-m) free varaibles($0<=num(free)<=n-r$)

우리가 중학교때 배웠던 방식대로 생각해보면 다음과 같다.
예를들어, 방정식은 2개인데, 미지수는 4개인 경우이다. 이 때, 미지수에 대한 비만 구할 수 있으므로,
해는 무수히 많다.

이것을 선형대수적으로 바라보면 어떻게 해야할까? 우리가 배웠던 Nullspace를 구하는 방법을 이용하면 된다.

예시를 하나 보자.
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img07.png" width="40%"></center>

위와 같이 Nullspace의 기저가 존재하게 되면, complete solution은 선, 면과 같은 모양의 공간을 형성하게 되
므로, 무수히 많은 해를 지닌다고 볼 수 있다.


### 4. r=m=n (m=n) 

r=m=n인 경우, 정방행렬에서, full rank를 가지는 조건이다.

예를 보자.

A=2x2의 행렬인 경우, R=rref(A)=I가 된다. 그러므로 단 1개의 해만 존재한다.

### abstraction
<center><img src="/public/img/2019-07-07-linear algebra-lecture08/img08.png" width="100%"></center>







