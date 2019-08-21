---
layout: post
title: Digital Image Processing - Digital image fundamental_1 (chapter2), (Rafael C. Gonzales)
author: Sunwoo Kim
categories: Computer Vision
tags: [Digital Image Processing]
---

## 시각적 인지의 요소(Elements of visual perception)
<center><img src="/public/img/Digital Image Processing-Chapter2/img01.png" width="40%"></center>

우리 눈의 구조는 위와 같이 생겼다. 위 그림에서 사물을 인지 하는데 있어서 중요한 역할을 하는 조직들이 있다.

Cornea(각막) : 빛을 모으는 역할

Iris(홍채) : 빛의 양을 조절하는 역할

Lens(각막) : 빛을 굴절 시켜, 빛을 한 점으로 모으는 역할

Retina(망막) : 빛이 도달하는 지점

[다음 영상](https://www.youtube.com/watch?v=rlmIfIoyiSM)을 보면, 눈에 상이 맺히는 과정을 이해할 수 있을 
것이다.

즉, 물체의 한 지점을 통과한 다양한 각도의 빛이 눈으로 들어와서, 각막을 통과 하여, 망막의 한 지점에 맺히게 
되면 뚜렷하게 보이게 된다. 이와 반대로, 망막보다 앞이나 뒷 지점에서 빛이 한 점에 모이게 되면, 정작 망막에 
맺히게 될때에는 빛이 퍼져서 맺히게 되므로, 물체의 한 지점이 망막의 한 지점에 맺히는게 아니고, 여러 지점에 
퍼져서 맺히게 되므로, 물체가 흐릿하게 보여지게 되는 것이다.

책에서는 카메라와 우리의 눈을 비교하여 설명해 놓은 부분이있다. 카메라는 렌즈의 위치를 조절하여 상이 맺히는 
지점을 조절하지만, 사람은 각막의 위치를 움직일 수 업식 때문에, 각막의 두께를 조절하여, 빛이 꺽이는 정도를 
조절하여 초점을 조정한다.

<center><img src="/public/img/Digital Image Processing-Chapter2/img02.png" width="50%"></center>

<center><img src="/public/img/Digital Image Processing-Chapter2/img03.png" width="50%"></center>

<center><img src="/public/img/Digital Image Processing-Chapter2/img04.png" width="50%"></center>

카메라의 초점조절에 관한 영상은 [다음영상](https://www.youtube.com/watch?v=R1Md9XFSp08)을 참조하자.

  망막에 상이 맺히고 난 후, 눈에 있는 시신경 세포를 통하여 뇌에 정보가 전달되어 우리는 사물을 인지할 수 있게된다. 그러므로 중간 다리 
역할을 해주는 시신경세포에 대해서 알아볼 필요가 있다. 시신경 세포는 **원뿔세포(Cone cell)**와 **간상세포(Rod cell)**로 이루어져있다. 

**원뿔세포**는 색깔에 민감하며, 세세한 것들을 식별할 수 있다. 원뿔세포가 세세한 것들을 식별할 수 있는 이유는 각 원뿔세포 하나가 신경 하나에 
일대일로 연결되어있기 때문이다. 마치 기계식 키보드가 멤브레인 키보드에 비해서 동시 입력이 더 잘되는 것과 비슷하다.

**간상세포**는 원뿔세포보다 그 수가 많다. 하지만, 원뿔세포와는 반대로, 밝은 상황에서 색에 민감하기 보다, 어두운 상황에서의 조도에 민감하다. 
그리고, 간상세포는 일대일이 아니고, 다대일로 신경세포에 연결이 되있어서, 세세한 식별을 하기보다는 전체적인 형상을 식별한다.

다음 그래프는 빛의 세기에 따른, 인지한 주관적 밝기의 변화 정도를 나타난다.

<center><img src="/public/img/Digital Image Processing-Chapter2/img05.png" width="40%"></center>

scotopic vision(암소시), photopic vision(명소시)는 각각 간상세포, 원뿔세포가 주로 인지에 관여하는 영역이다. 즉, 빛의 세기가 낮을때는 
간상세포가, 빛의 세기가 높을때는 원뿔세포가 관여하는 것을 알 수 있다. 그리고 중간단계의 빛의 세기에서는 두 세포가 모두 관여하는 모습을 볼 
수 있다.

위 그래프를 보면, $B_a$, $B_b$라고 나와있는 지점이 있다. 두 지점의 의미는 다음과 같다. 우리의 눈이 빛의 세기가 $B_a$인 지점에 적응하고 
있을 때, **한 순간(갑자기)** 빛의 세기를 낮추거나 높혔을 떄 우리는, 짧은 $B_a$를 지나는 짧은 그래프로 밝기를 인식한다는 것이다. 그러나 
짧은 그래프의 임계치를 아래인, $B_b$아래로 빛의 세기가 낮아진다면, 우리의 눈은 아무것도 없는 흑색배경으로 인지하고, 임계치보다 빛의 세기를 
높힌다면, 아무것도 없는 백색배경으로 인지할 것이다.

이와 같이 생각해보면 편할것이다. 만화에서 어떤 킬러가 건물안의 사람들을 죽이기 위해, 미리 눈을 감고있어서 어둠에 적응해 놓는다. 그 후 
조력자가 그 장소의 불을 끈 후, 킬러는 눈을 뜨고 사람들을 암살하려고 한다. 건물안에 있던 다른 사람들은 눈을 떠도 아무것도 안보이겠지만, 
눈을 미리 감아놓은 킬러는 사람들의 전체적인 형상이 보이게된다. 이는 위에 설명한 것 처럼 킬러는 빛의세기가 낮은 지점에 눈이 적응해 있었고, 
다른 사람들은, 빛의 세기가 높은지점에 눈이 적응해 놓았기 때문에, 불이 꺼졌을 때, 건물에 있는 사람들은 빛의 세기가 인지할 수 있는 임계치를 
넘어서 아무것도 안보인 것이다. 빛의 세기가 상위 임계치를 넘기는 경우는 섬광탄의 경우를 생각해보면 바로 이해가 갈 것이다.

<center><img src="/public/img/Digital Image Processing-Chapter2/img06.png" width="40%"></center>
위 그림은 **Mach bands**라고 한다. 각 색의 경계에서, 인지하는 밝기의 그래프를 보면 경계에서 살짝 씩 튀는 모습이 관측된다. undershoot와 
overshoot가 관측된다고 할 수 있다. 

<center><img src="/public/img/Digital Image Processing-Chapter2/img07.png" width="40%"></center>
위 그림을 보면, 가운데 회색 사각형은 같은 밝기인데도, 주변 영역의 색깔이 어떤지에 따라 선명하게 보이거나 , 더 흐릿하게 보인다.

<center><img src="/public/img/Digital Image Processing-Chapter2/img08.png" width="40%"></center>
위 그림을 보면 첫 번째 그림에는 사각형이 없는데 사각형이 있는 것처럼, 두 번째 그림에서는 마치 원이 있는 것 처럼, 세 번째 그림에서는 꺽새 
사이에 있는 두 선분의 길이가 같지만, 밑에있는 선분의 길이가 더 길어보이고, 네 번째 그림에서 평행한 직선들이 마치 평행하지 않은 것 처럼 눈에 
보이게 된다.

사람의 눈은 실시간으로 변하는 주변환경, 연속적인 빛의 입력, 시신경들의 상호작용 등으로 인하여 이러한 시각적 인지의 오류를 발생시키게 된다.

