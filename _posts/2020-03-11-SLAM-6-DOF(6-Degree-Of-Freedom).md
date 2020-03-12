---
layout: post
title: SLAM-6-DOF(6 Degree Of Freedom)
author: Sunwoo Kim
categories: SLAM
tags: [SLAM, 6-DOF]

---

처음 SLAM공부를 시작하면서 관련된 키워드들에 대해서 정리중입니다. 틀린 부분이 있다면 말씀해주세요! 

## 6-DOF(6 Degree Of  Freedom)

6-DOF는 6자유도라고 번역할 수 있다. 즉, 6개의 자유도를 의미한다. 6개의 자유도는 아래 그림과 같다.

<center><img src="/public/img/SLAM-6-DOF(6 Degree Of Freedom)/img_1.png" width="60%"></center>

<center>그림1. 6-DOF</center>

사용자는 이 6개의 자유도를 모두 관측하여 사용할 수 있고,필요에 따라서 이 중 일부의 자유도만 사용할 수 있다. 대체적으로 6-DOF중 3-DOF씩 2개로 분류하여 언급한다.

## 3-DOF

### Rotational DOF

말 그대로 회전과 관련된 DOF이다. 위 그림에서 알 수 있듯이, 회전과 관련된 **Pitch,Yaw, Roll** 이렇게 세가지의 자유도를 의미한다. VR기기를 예로들면, 일반적인 저가형 VR기기에서는 위 3가지의 자유도만 제공한다. VR헤드셋을 착용하고 가만히 있는 상태에서 머리를 회전시키거나 위아래로 움직일 때, 화면이 잘 따라온다는 것이다. 이런 기능을 **Rotational tracking** 이라고 한다.

### Positional DOF

위치와 관련된 DOF이다. **x, y, z축의 자유도**를 의미한다. 사용자가 VR헤드셋을 쓴 상태에서 앉았다 일어나거나, 움직이는 등 머리의 위치가 바뀌었을 떄 이러한 점을 화면에도 반영시킬 수 있다는 것이다. 이것을 **Positional tracking**이라고 한다.

Rotational tracking만 가능한 경우 VR에서 투사체가 날아와서 머리를 움직이거나, 실제로 이동할 경우에 이런 사용자의 동작이 화면에는 반영이 안된다는 뜻이다. 그러므로 rotational tracking만 가능할 경우 가만히 앉아서 감상하거나 즐길 수 있는 컨텐츠에 국한될 수 밖에 없다. 

Positional tracking만 가능한 경우 rotation이 안되므로, 이동은 가능하지만 머리를 꼳꼳이 고정한 상태로 VR기기를 이용해야 한다. 그러므로 rotational tracking만 제공하는 vr기기들은 있지만, positional tracking만 제공하는 기기는 없다.

Rotational tracking과 positional tracking이 모두 제공되는 기기는 6-DOF가 모두 제공되는 것이라고 볼 수 있다. 6-DOF가 모두 제공된다면, 물체를 만지거나 사용자가 움직이는 등, 보다 더 사용자가 능동적으로 움직일 수 있다. 간다한 예로, VR게임으로 유명한 비트세이버가 6-DOF가 모두 지원되는 예이다.

출처
그림1. https://commons.wikimedia.org/wiki/File:6DOF.svg