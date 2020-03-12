---
layout: post
title: Feature detector-1. Scale이란?
author: Sunwoo Kim
categories: CV(ComputerVision)
tags: [SLAM, Digital Image Processing, Feature detector]
---

영상과 관련된 자료들을 보다보면 scale이란 단어를 한 번씩은 본적이 있을 것이다. 그렇다면 과연 scale이란 무엇일까?

스케일(scale)이란 말은 우리 일상에서도 쓰이는 용어이다. "야 너 스케일 크다!" 라는 말이 있다. 보통 일을 크게 벌릴 때 쓰는 말이다. 또, 우리가 만수르가 쓰는 돈을 보면 스케일이 크다라고 생각하지만, 만수르 입장에서는 크지 않다. 이미지에서 스케일도 이와 비슷하다.

이미지에서 스케일을 생각해보기 위해서 마음속에 사각형 하나를 그려보자. 그것을 우리는 **window**라고 할 것이다. 그리고 아래 그림을 보자.노란색 사각형이 우리가 생각한 윈도우다. 그리고 그 창을 통해서 어떤 풍경을 바라본다고 하면 위와 같은 그림이 될 것이다. 

<center><img src="/public/img/Feature detector-Scale이란/img_1.png" width="70%"></center>

그렇다면 우리가 더 조금보거나 더 많이 보려면 어떻게 해야할까? **우리가 어떤 풍경을 볼 때, 큰 창을 통해서 보는 것과 작은 창을 통해서 보는 것을 생각해보자.** 큰 창을 통해서 보면 풍경이 더 많이보이고, 작은 창을 통해서 보면 풍경이 더 적게 보인다. 그리고, **가까이서 보면 풍경이 더 적게 보이고, 멀리서 보면 풍경이 더 많이 보인다.**

즉, 이것이 핵심이다.

1. 창의 크기를 조절한다.
2. 더 다가가서 보거나, 멀리서 본다. 그런데 이미지는 그렇게 할 수 없으므로, 이미지를 확대하거나 축소 시킨다고 생각하자.

밑에 그림을 보고 이해해보자.

<center><img src="/public/img/Feature detector-Scale이란/img_2.png" width="70%"></center>

<center><img src="/public/img/Feature detector-Scale이란/img_3.png" width="70%"></center>

차이가 보일 것이다. 왼쪽의 경우가 원본 이미지 대비 더 많은 영역이 담겼고, 오른쪽 영역이 더 좁은 영역이 담겼다. 이를 요약해서 보면 다음과 같다.

<center><img src="/public/img/Feature detector-Scale이란/img_4.png" width="70%"></center>

그렇다면 이 정도 느낌까지는 받았을 것이다. 뭔가 많이 담기면 large scale이고, 조금 담기면 small scale인거 같다. 그런데 뭔가 확실하게 large scale은 뭐고 small sacle은 뭐인지에 대한 느낌이 안 올 것이다. 밑에 그림을 보자.

<center><img src="/public/img/Feature detector-Scale이란/img_5.png" width="80%"></center>

왼쪽 오른쪽 이미지의 크기는 같다. 이 크기를 Window라고 생각하자. 그러면 같은 크기의 윈도우 안에 왼쪽 이미지가 더 많은 영역이 담기고, 오른쪽 이미지가 더 좁은 영역이 담겼다. 윈도우 크기에 비해서 이미지가 더 많이 담겼을 때, large scale, 조금 담겼을 때, small scale이라고 생각하면된다. 이것을 이해했다면, 밑에 그림도 바로 이해할 수 있을 것이다.

<center><img src="/public/img/Feature detector-Scale이란/img_6.png" width="60%"></center>

이러한 스케일에 대해서 아는게 왜 중요하냐면 이미지 처리에서 scale로 인한 문제들이 생기기 때문이다. 이를 예시로 filtering을 봐보자.

<center><img src="/public/img/Feature detector-Scale이란/img_7.png" width="60%"></center>

(small, large scale의 기준은 **원본**이미지 대비 많은 영역을 담고 있는가, 적은 영역을 담고 있는가의 여부이다.) 우선, 스케일을 신경쓰지 말고 보자. 노란색 네모는 filter의 크기이다. 똑같이 window라고 생각하고 보자. large scale 밑에있는 이미지의 경우에느 확실히 코너지점이 있는지 없는지 알 수가 있다. 하지만 small scale밑에 있는 이미지의 경우에는 코너가 확실히 있는지 없는지 알 수가 없다.

만약에 우리가 어떤 알고리즘이나 딥러닝 네트워크를 설게했는데, 3x3크기의 필터밖에 존재하지 않는다고 하자. 그런데 입력으로 들어오는 이미지가 자동차 사진이라고 한다면, 가까이서 찍힌 자동차 사진, 멀리서 찍힌 자동차 사진들이 있을 것이다. 우리가 위에 배웠던 것과 같이 거리가 달라지면 이미지의 스케일이 달라진다. 그렇다면 위의 곡선 그림과 같은 문제가 생긴다. 모든 스케일에 대해서 한 크기의 필터만 사용해서는 모든 스케일의 이미지에 대해서 코너를 가지고 있는지, 더 나아가서는 동일한 특징을 지니고 있는지를 검출해낼 수 없다는 것이다.

즉, 이를 해결하기 위해서는 **다양한 크기의 필터를 사용**하는 것이다. (딥러닝 같은 경우에는 이미지를 확대, 축소해서 다양한 scale의 이미지를 입력으로 넣어서 훈련시켜도 될 것 같다.) 그렇다면 위의 그림처럼 다양한 scale에 대해서도 대응을 할 수 있게 될 것이다.