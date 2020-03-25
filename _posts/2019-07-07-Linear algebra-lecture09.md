---
layout: post
title: Linear Algebra - 9. Independence, span, basis, dimension
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 다음에 관해서 배운다.
- Linear independence
- Spaning a space
- Basis and diemnsion

---
## Independency & Span & Basis

### Independency
만약 벡터 $x_1, x_2,...x_n$의 선형결합의 결과를 0을 만드는 어떤 결합도 존재하지 않는다면,
벡터$x_1, x_2,...x_n$은 서로 independent하다고 말할 수 있다.(단, zero combination 제외)

($c_1x_1 + c_2x_2 +...+c_x_n =/ 0$)

이 말을 간단하게 하면 무엇일까?

서로가 서로에게 영향을 끼치지 않는게 독립이라면, 벡터에서는 서로가 서로를 만들 수 없으면 독립인 것이다.
예를 들어, $V={v_1=(1,1), v_2=(2,5)}$이라면, $v_1$과 $v_2$는 서로 상수배를 하여 서로를 만들 수 없다.

하지만, $V={v_1=(1,1), v_2=(2,5), v_3=(1,4)}$인 경우, $v_3 = 1*v_2+(-1)*v_1$ 이므로, 독립이 아니다.

### Span
벡터 v_1,...,v_l이 공간을 **span**한다는 것의 의미 : 해당 공간이 v_1,...,v_n의 모든 선형 결합으로
구성된다는 뜻이다.

### Basis(기저)
어떤 공간에 대해서 벡터 v_1,...,v_d이 다음 2가지 성질을 만족하면 해당 벡터는 해당 공간의 기저라고 한다.

1. v_1,...,v_n은 독립이다.
2. v_1,...,v_n이 공간을 span한다.

정리하면 벡터가 어떤 공간의 기저라는 것은, 독립된 벡터들이 선형결합을 통해서 해당 공간을 표현할 수 있으면,
그 독립된 벡터들을 기저라고 하는 것이다.

---
## Dimension(차원)
어떤 공간을 구성하는 기저는 **같은 수**의 벡터를 가지고있다. 이 때, 그 수가 dimension(차원)이다.

예를 들어, 어떤 공간 $S_1은 $v_1=(1,0)$이 만드는 선 모양의 공간이고,

$S_2는 v_2=(-2,3)$이 만드는 선 모양의 공간이라고 했을 때,

$S_1의 기저는 (1,0), S_2의 기저는(-2,3)$으로 기저는 다르지만, 기저의 개수는 모두 1개이다.
우리는 이 두 공간을 1차원 공간이라고 부른다.

### Dimension & rank
**rank(A) = # pivot colmuns = dimension of C(A) = dimension of R(A)
dim C(A) = r, dim R(A) = r, dim N(A) = n-r = # free varaibles**

여기서 랭크가 dimension of R(A)라는 것은 쉽게 이해할 수 있을 것이다.

왜냐하면, 우리가 Elimination을 row들간의 연산을 통해서 수행했기 때문이다.

모두 0이 되지않고 남은 행은, 나머지 행들의 선형결합으로 만들어 질 수 없는 행 이므로, 
모두 독립인 행들이다. 그러므로, rank(A)는 dimension of R(A)이다.

그렇다면, 왜 rank(A)=dimension of C(A)일까? 그 이유는 알고보면 간단하다.

pivot columns들로 A에 존재하는 모든 columns들을 만들어 낼 수 있기 때문이다.

그렇기 때문에, rank의 개수가 dimension of C(A)이다.

마지막으로 Nullspace의 차원을 알아보자.

이제 Null space를 구하는 과정은 모두 알 것이라고 가정하겠다. 모르는 사람들은
Lecture6,7을 보고 오자!

Nullspace를 구할 때, varaible이 $x_1,..,x_n$까지 있을 때, free varaible이 3개($x_2,x_3,x_4$)
라면, $(x_1,1,0,0,....,x_n)$, $(x_1,0,1,0...,x_n)$, $(x_1,0,0,1,...,x_n)$인 column vectors가
Nullspace를 구성하는 3개의 기저가 되었었다. 그러므로 **Null space의 차원은 free varaibles의 수 라고
말할 수 있다.**
