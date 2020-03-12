---
layout: post
title: Feature detector-Shi-Tomasi corner detector
author: Sunwoo Kim
categories: CV(ComputerVision)
tags: [SLAM, Digital Image Processing, Feature detector]

---

## Shi-Tomasi corner detector

이번에 다룰 코너 검출기는 전에 배웠던 [Harris corner detector](https://sunshower76.github.io/cv(computervision)/2020/03/12/Feature-detector-Harris-corner-detector/)와 별 다르지 않다. harris corner detector에서 flat, corner, edge를 정하는 threshold인 **R**에 대한 함수만 달라지는 것이다.

harris corner detector 에서 R은 다음과 같이 정의됬었다.

<center>$R=det(M) -k*tr(M)$</center>

<center>$R=\lambda_1\lambda_2 -k(\lambda_1+\lambda_2)$</center>

그렇다면 Shi-Tomasi corner detector에서 R값은 다음과 같이 정의된다.

<center>$R=min(\lambda_1\ , lambda_2)$</center>

그리고 이를 그림으로 나타내면 다음과 같다.

<center><img src="/public/img/Feature detector-Shi-Tomasi corner detector/img_1.png" width="80%"></center>

<center>파란색 영역 = Flat</center>

<center>주황색 영역 = Edge</center>

<center>연두색 영역 = Corner</center>