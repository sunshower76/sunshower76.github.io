---
layout: post
title: Feature detector-5. SIFT(Scale Invariant Feature Transform)
author: Sunwoo Kim
categories: CV(ComputerVision)
tags: [SLAM, Digital Image Processing, Feature detector]
---

## SIFT(Scale Invariant Feature Transform)

전에 배웠던 harris corner detector, shi-tomasi corner detector는 scale변화에 약하다는 장점이 있다고 했었다. 다른 점에대해서도 전에 배웠던 두 corner detector보다 더 강인하고 scale에 대해서도 훨씬 더 강인한 알고리즘이 SIFT이다. 즉, SIFT는 이미지의 크기, 회전, 방향등의 affine transform에서 나오는 성질들과 스케일에 강인한 알고리즘이다.

- SIFT의 과정
  1. scale space만들기
  2. DoG(Difference of Gaussian)
  3. Keypoints 찾기
  4. Bad keypoints 제거
  5. 최종적인 SIFT 특징 산출

[Scale에 대해서]([https://sunshower76.github.io/cv(computervision)/2020/03/11/Feature-detector-Scale%EC%9D%B4%EB%9E%80/](https://sunshower76.github.io/cv(computervision)/2020/03/11/Feature-detector-Scale이란/)의 글을 읽고 scale에 대해서 안다고 하고, 바로 첫 번째 과정부터 설명을 하겠다.

1. Scale space 만들기

   scale space는 여러 스케일이 존재하는 공간이라는 것이다. 한 이미지에 대해서 여러 스케일을 갖게하기 위해서 점차적으로 이미지를 줄인다. 만약에 **원본 이미지의 크기를 1**이라고 했을 때, 4, 1, 1/4, 1/16크기의 이미지가 있다면 scale space는 4개의 scale로 구성이 된다고 할 수 있을 것이다.

