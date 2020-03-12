---
layout: post
title: Linear Algebra - 1. Row & Column pictures
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
> 선형대수학에 관한 포스트는 유튜브에 올라와있는 MIT의 Gilbert Strang 교수님의
강의와 교재를 참고하여 이루어질 예정입니다.

## Row & Column Pictures
1장에서는 선형대수의 핵심인 linear combination(선형결합)의 예를 보여주기 위해서,
Row picture와 Column picture에서 부터 출발한다. 다음 예제를 살펴보자.

ex)
n개의 linear equations 과 n개의 unknowns(미지수)가 있다고 가정하자.(n=2)
<center><img src="/public/img/2019-07-02-linear algebra-lecture01/img1.gif" width="50%"></center>

### Row picture(AX=b)
강의에서 교수님은 Row picture을 "One equation at a time" 이라고 말씀하셨다.
다음의 식에 대해서, 아래의 그림을 보고 위의 말을 파악해보자.
<center><img src="/public/img/2019-07-02-linear algebra-lecture01/img2.gif" width="30%"></center>
<center><img src="/public/img/2019-07-02-linear algebra-lecture01/img3.gif" width="70%"></center>

위 그림이 바로 Row picture이다. 어디서 많이 본 그림이다.
두 행을 참고하여, x,y에 대한 직선의 방정식을 만들어, 직선을 그리고, 그 교점을 찾았다.
그렇다 우리는 **행(Rows)**을 참고했다.
Row picture는 matrix=A, unkonwns=X, constant=b 라고 하여, AX=b로 표현할 수 있다.

### Column picture(au+bv+cw+...=n)
이번에는 column picture에 대해서 살펴보자.
Row picture에 대해서 감을 잡았다면, Column picture도 금방 이해할 수 있다.
Row picture가 행을 보는 거였다면, Column picture는 열을 보면 된다.

<center><img src="/public/img/2019-07-02-linear algebra-lecture1/img4.gif" width="50%"></center>
<center><img src="/public/img/2019-07-02-linear algebra-lecture1/img5.gif" width="70%"></center>

Column picture을 그림으로 표현하면 위와 같다. 말 그대로 **열(Columns)**을 참고하여, 벡터로 표현한 것이 눈에 띌 것이다.
그리고 그것을 상수x벡터 의 합(au+bv+cw+...=n, 위 예제 에서는 xu+yv=n)의 형태로 표현했다.
**Column picture로 나타내는것이 바로 linear combination(선형결합)이다.**
