---
layout: post
title: Feature detector-4. Scale space
author: Sunwoo Kim
categories: CV(ComputerVision)
tags: [SLAM, Digital Image Processing, Feature detector]
---

## Scale space

전에 [Scale이란?](https://sunshower76.github.io/cv(computervision)/2020/03/11/Feature-detector-Scale이란/)에서 스케일에 대해서 배웠다. 그리고 Harris corner detector와 Shi-Tomasi corner detector를 살펴보면서, 두 검출기는 다양한 스케일에 대해서 강인하지 못하다는 점을 언급하였다. 그렇다면 스케일에 강인하기 위해서 나온 기법들이 있다. 그 중 살펴볼 기법이 **SIFT(Scale Invariant Feature Transform)**이며, 기초가 되는 기념이 **scale space**이다. scale space는 이후에도, CNN네트워크에서 이미지 피라미드를 구성하는 방향으로도 응용이 되는 개념이니 잘 짚고 넘어가면 후에 왜 피라미드를 구성하여 네트워크를 구성하였는지에 대한 이해도 어렵지 않게 받아들일 수 있을것이다.

### Scale invariant

 그런데 scale space가 무엇이길래 scale space를 만들어야 할까? 그것은 바로 **scale invariant**한 성질을 만들기 위함이다. 위키피디아 에서는 scale invariance에 대하여 '"In [physics](https://en.wikipedia.org/wiki/Physics), [mathematics](https://en.wikipedia.org/wiki/Mathematics) and [statistics](https://en.wikipedia.org/wiki/Statistics), **scale invariance** is a **feature of objects or laws that do not change** if scales of length, energy, or other variables, are multiplied by a common factor, and thus represent a universality."'라고 말하고 있다. 굵은 글씨인 부분을 보면, 스케일이 변할 떄에도 변하지 않는 특징이나 객체, 법칙에 대하여 scale invariant하다고 기술해 놓았다. 또, [stackExchange의 scale space에 대한 답변](https://physics.stackexchange.com/questions/90883/what-is-scale-invariance)을 참고해보면, scale space는 **self-similarity**라고 말한다. 이미지를 얼마나 zoom-in 혹은 zoom-out하든 똑같이 보인다는 것이다. 위 링크에 있는 도형이나 프랙탈, 함수 가지고는 이해가 잘 안될수도 있으니, 우리가 모두다 잘 알고 직관적인 **power of laws** 즉, n차함수를 봐보자. 여기서는 간단하게 이차함수를 볼것이다.

<center><img src="/public/img/Feature detector-Scale space/img_1.png" width="90%"></center>

<center> [그림1] power of laws </center>

위 그림은 $y=x^2$의 함수이다. 다만, x, y축을 보면 왼쪽과 오른쪽 그래프의 스케일이 다른 것을 볼 수 있다. 그럼에도 불구하고, 모양이 똑같은것을 볼 수 있다. 마치, 왼쪽 그래프를 zoom-in해서 보면, 오른쪽 그래프가 나오는 느낌이 들 것이다. **다만 log scale로 변환하는등 모든 scale변환에 대해서 invariant하지는 않다...** 보통 분야마다 scale invariant라고 말할 때, 적용되는 대상이 어떤 대상이고 어떤 scale transform에 대해서 invariant한지에 대해서 언급하는 것이 다른듯 하다.

<center><img src="/public/img/Feature detector-Scale space/img_2.png" width="90%"></center>

<center> [그림2] Scales</center>

전에 scale에 대해 배웠던 것에 대해서 떠올려보자. 이미지를 확대하거나 축소하게 되면 스케일이 변하게 된다는 것을 말했을 것이다. 즉 위의 경우와 다르게 **영상**은 **scale invariant**하지 않다. 이처럼 스케일이 다를 때, 전에 말했던 것 처럼 연산 결과를 최대한 비슷하게 만들기 위해서는 이미지 자체를 다시 조정해서 스케일을 맞추는게 힘드니, 필터크기를 다양하게 하여 스케일이 달라도, 계산되어 나온 특징을 최대한 비슷하게 만들 수 있다고 scale에대해 다룬 게시물 에서 언급하였다.

이번에는 scale invariant를 위해 제시된 다른 방법인 **scale space**에 대해서 살펴볼 것이다. 

**Scale space**이름을 보았을 때, 스케일 공간이다. 선형대수에 벡터공간이 여러가지 벡터가 모여서 만드는 공간이듯이, scale  space또한 **여러 스케일들**이 모여 이루는 공간이라는 뜻이다. 그러면 **여러 스케일**을 만들어야 공간을 만들텐데 어떻게 여러가지 스케일을 만들까? 

### Scale space 만들기

<center><img src="/public/img/Feature detector-Scale space/img_3.png" width="90%"></center>

<center><img src="/public/img/Feature detector-Scale space/img_4.png" width="90%"></center>

<center> [그림3] Scale space</center>

Scale space를 만드는 과정은 위 그림이 전부다.

1. 원본 이미지의 가로 세로 두 배 확대, 확대된 이미지가 시작 이미지
2.  $\sigma=\sqrt2$ 로 설정한 후 $\sigma$를 $\sqrt2$배씩 늘리면서 Gaussian blurring  **5회** 진행
3. 그 후,  **세 번째 이미지**의 크기를 가로 세로 두 배 줄인 후, $\sigma = 2\sigma$로 설정한 후, 똑같이 $\sqrt2$배씩 늘리면서 blurring 5회 진행
4. 이 과정을 octave개수 만큼 반복.

그렇다면 왜 이러한 과정을 거치는지 생각을 해보자. [Scale이란?](https://sunshower76.github.io/cv(computervision)/2020/03/11/Feature-detector-1.-Scale이란/) 을 꼭! 보고오자.





## References

1. https://salkuma.wordpress.com/tag/gaussian/
2. [Causality]( https://staff.fnwi.uva.nl/r.vandenboomgaard/IPCV20172018/LectureNotes/IP/ScaleSpace/scalespaceCausality.html)
3. [Dark programar : scale space](https://darkpgmr.tistory.com/137)
4. [Scale space theory in computer vision(11-15p)](http://www.diva-portal.org/smash/get/diva2:440615/FULLTEXT01.pdf)
5. [Scale invariance in cognition - Nick Chater, dept of Psychology, Univ colleg London](https://www.slideserve.com/august/scale-invariance-in-cognition)
6. [Scale invariance of power law functions]( http://felix.physics.sunysb.edu/~allen/540-05/scaling.html)
7. [SIFT정리](https://salkuma.files.wordpress.com/2014/04/sifteca095eba6ac.pdf)
8. [Multiple Drosophila Tracking System with Heading Direction](https://www.researchgate.net/publication/312264140_Multiple_Drosophila_Tracking_System_with_Heading_Direction)

