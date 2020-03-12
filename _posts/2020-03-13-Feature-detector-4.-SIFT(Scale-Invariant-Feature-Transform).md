---
layout: post
title: Feature detector-4. SIFT(Scale Invariant Feature Transform)
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

