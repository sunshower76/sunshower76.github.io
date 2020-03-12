---
layout: post
title: SLAM-Visual Inertial Odometry(VIO)
author: Sunwoo Kim
categories: SLAM
tags: [SLAM, Visual Inertial Odometry]
---

처음 SLAM공부를 시작하면서 관련된 키워드들에 대해서 정리중입니다. 틀린 부분이 있다면 말씀해주세요! 

## Visual Inertial Odometry(VIO)

Visual - 시각의, Inertial - 관성의, Odometry - 기기의 state측정 이라고 해석했다. 여기서 **state**는 기기의 orientation(방향), altitude(고도), linear velocity(속도), rotation(회전) 등이 존재한다. 여기서 orientation, altitude, rotation을 합쳐서 **pose(position & rotation)**라고 한다. pose는 보통 시작점 origin(0,0,0)을 기준으로 측정한다. 요약하자면, **visual inertial odometry는 시각과 관성정보를 이용해서 기기의 state를 관측하는 기술이다.** 조금 더 풀어쓰자면, 현실세게에서 디바이스의 현재 상태를 추적하는 것으로, 디바이스가 어느 위치, 회전값, 속도로 존재하는지 아는 것이다. 이것은 곧 SLAM의 **localization**을 위한 정보로 쓰인다.

그렇다면, odometry가 무엇인지는 알겠는데, 왜 visual정보, inertial정보를 모두 사용할까? 라고 생각해볼 수도 있고, visual odometry와 inertial odometry가 모두 있지 않을까 생각할 수 있을 것이다. 두개 다 같은 말이며, 맞는 말이다. 그리고 또한, 왜 visual, inertial정보만 사용하지? 다른 수단은 없을까? 라고 생각할 수도 있다. 참고논문[1]에 따르면, 크게 5개의 odometry를 위한 수단이 있다고 한다. wheel, Radar, Inertial, visual, laser정보를 이용하는 방법이 존재한다고 한다. 그리고 상황에 따라서 사용할 수 있는 장비 혹은 적용할 수 있는 장비가 다르기 때문에 적용하는 방법론이 달라진다.

### inertial

inertial(관성) 정보는 IMU(Inertial Measurement Unit)에 있는 accelerometer와 gyroscope로 부터 기기가 받고있는 힘, 가속, 회전 등을 의미한다. 즉 IMU 센서에 가해지는 **motion**을 측정하는 것이다. IMU센서는 빠른모션의 측정에 유리하지만, 미소한 모션에 대해서는 bias나 noise로 인하여 drift가 누적되는 문제가 발생한다고 한다.

### visual

visual(시각)정보는 카메라로부터 얻어지는 영상으로 부터 얻어진다. 카메라는 IMU와 반대로 저속의 움직임에서 비교적 정확한 모션을 검출하는데 유리하다고 한다. 하지만,  조명, 속도에 큰 영향을 받는다, 예를 들어, 조명이 어두울 경우, 이미지가 어두워져 정확한 정보를 얻을 수 없고, 조명이 너무 밝을거나 빛이 번지는 경우 화면이 아예 하얗게 되어 정확한 정보를 얻을 수 없게된다. 그리고 속도가 너무 빠를 경우에는 motion blur가 생기게 되어 정확한 정보를 얻을 수 없다.

위와 같은 두가지 센서의 특징은 서로 **서로 보완적(complementary)** 이다. 그렇기에 같이 사용되는 것이라는 것을 알 수 있다.

자세한 기법이나 공식들은 후에 공부하여 추가하도록 하겠습니다!







Reference

[1] [2019. A Survey on Odometry for Autonomous Navigation Systems](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8764393)