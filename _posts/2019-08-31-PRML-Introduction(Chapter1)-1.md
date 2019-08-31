---
layout: post
title: Digital Image Processing - PRML-Introduction(Chapter1)-1
author: Sunwoo Kim
categories: Machine Learning
tags: [PRML]
---

## 1. Introduction

### 수학적 표기법(Mathematical notations)

1. Vectors : small letter(소문자)로 표기, 언급 없는이상 **열벡터(Column vecetor)** ex) \mathrm{x, y, v, w ....}

2. 위 첨자T(subscript T) : 전치(Transpose)를 의미, 즉 $x^T$는 행벡터를 의미한다.

3. Matrix : big letter(대문자)로 표기, ex) **M**과 같이 표기. ()

4. ($w_1, w_2 ,..., w_n$)의 표기는 n개의 원소를 가진 **행벡터**를 의미한다. 즉, (\mathit{w_1, w_2 ,..., w_n})^T는 **열벡터**이다.

5. [a,b] : 닫힌구간($a<=x<=b$) , (a,b) : 열린구간($a<x<b$) 을 의미한다.

6. MxM크기의 항등행렬(Identity matrix) 는 $I_M$으로 표기하낟.

7. $E_x[f(x,y)]$ : 확률변수 x에 대한 함수 f(x,y)의 기대값을 의미한다. 만약 x에 대한 분포가 다른 변수 z에 대해 조건부면, 해당 조건부 기대값은 
$E_x[f(x)|z]$와 같이 적었다. 비슷하게 분산 : $var[f(x)]$이라 적고, 공분산 : $cov[x,y], cov[x,x]=cov[x]$와 같이 적는다.

8. \mathrm{x}=\mathit{x_1}



<center><img src="/public/img/Digital Image Processing-Chapter2/img26.png" width="50%"></center>
