---
layout: post
title: Linear Algebra - 4. LU decomposition, Properties of inverse and transpose
author: Sunwoo Kim
categories: Mathematics
tags: [Linear Algebra]
---
>이번 강의에서는 저번에 미쳐 다루지 못한 Inverse of AB, A^T와
Product of elimination matrices, A=LU(no row exchanges)
에 대해서 다루도록 하겠다.
이번 장은 비교적 간단하게 넘어 가도록 하겠다.

---

## Some properties
이번 장에서 세세한 증명은 없지만, 전치(Transpose)연산과
역행렬(Inverse)연산에 대해서 간략하게 설명이 나온다.

### Inverse and Transpose
1.$ AA^{-1}=A^{-1}A=I$ (역행렬의 정의)

2.$ (AB)^{-1}=B^{-1}A^{-1}$

proof
>$ABB^{-1}A^{-1}=I$

3.$ (A^T)^{-1}=(A^{-1})^T$

proof
>$AA^{-1}=I$
$(AA^{-1})^T = (A^{-1})^TA^T = I$
$(A^{-1})^T=(A^T)^{-1}$

4.$L^T = U$ (여기서 L이나 U는 형태만을 의미한다.)

5.$U^T = L$

6.$L^{-1}=L$

7.$U^{-1}=U$

---

## LU decomposition
위의 성질들을 모두 알았다면, LU분해는 매우 간단하다. 앞 장에서 우리는 가우스 소거
법을 이용하여 A를 U형태로 변환시켰다. 만약 A=3x3 의 행렬이라고 한다면,

$E_{32}E_{21}A=U$ 의 형태를 얻었다.

$E_{32}E_{21}$는 어떤 형태의 행렬일까? 

$E_{32}E_{21}=L$ 이다. 왜냐하면, 밑에 행에서 위에 행을 빼는 연산이기 때문에,

위 연산은, 주 대각선 기준으로, 윗 부분이 아닌, 아랫 부분에 수가 붙는다.

그러면, $E_{32}E_{21}A=U$ 는 $LA=U$로 바꿔 쓸 수 있다.

$A=L^{-1}U$ 이고, 위에 설명한 성질 중 **7**번 성질에 의하여 $L=L^{-1}$이므로,

$A=LU$를 만족한다.(물론 $L$과 $L^{-1}$의 값은 다르다. 여기서는 형태 만을 고려한 식이다.)

위 과정을 거치면 LU분해가 모두 끝난다.
