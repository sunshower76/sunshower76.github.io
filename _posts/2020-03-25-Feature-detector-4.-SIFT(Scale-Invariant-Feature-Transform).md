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
  3. Key candidates 찾기
  4. Accurate keypoint localization
  5. Orientation Assignment
  6. Key point descriptor
  7. Key matching

[Scale에 대해서](https://sunshower76.github.io/cv(computervision)/2020/03/11/Feature-detector-Scale이란/)의 글을 읽고 scale에 대해서 안다고 하고, 바로 첫 번째 과정부터 설명을 하겠다.

### 1. Scale space 만들기

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_1.png" width="60%"></center>

<center> [그림1] Scale space in SIFT</center>

1. 원본 이미지를 2배 확대한 후, $\sigma = 1.6$인 Gaussian kernel로 blurring을 진행한 이미지가 Octave 1의 level1에 해당한다. 즉 이것이 시작이다.
2. $k=\sqrt2$로 설정한다. 즉 level2의 inner scale = $\sqrt2 \sigma$가 되도록 convolution을 진행해야한다. 그러므로 blurring 해야하는 가우시안 커널의 $\sigma$ 값은 $(\sqrt2 * 1.6)^2 = (1.6)^2 + \sigma ^ 2$가 된다. 이런 식으로 계속해서 더 높은 level을 계산한다.
3. 5level 까지 계산되었을 때, blurring을 멈추고 inner scale이 $2\sigma$인 부분의 이미지를 2배의 down-sampling을 진행한다. down-sampling된 이미지가 octave 2의 시작 이미지가 된다. 
4. Octave 4까지 위의 계산을 반복한다. 

### 2. DoG 계산

Scale space에 대해서 배울 때, DoG로 LoG를 근사한다고 하였다([LoG 및 DoG](https://sunshower76.github.io/cv(computervision)/2020/03/23/Laplacian-of-Gaussian(LoG)-&-Difference-of-Gaussian(DoG)/)도 가능하면 보고 오면 좋다). SIFT에서는 LoG커널을 통하여, 극점 을 찾아내어 blob검출을 필요로 한다. 그렇기 때문에, 대신 DoG를 계산하여 LoG를 근사한다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_2.png" width="90%"></center>

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_3.png" width="100%"></center>

<center> [그림2] DoG in SIFT</center>

### 3. Key candidates 찾기

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_4.png" width="50%"></center>

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_5.png" width="90%"></center>

<center> [그림3] Key candidates 찾기</center>

DoG 연산 후, 한 octave에서 5개의 이미지가 4개의 DoG 결과로 줄어든 모습을 확인할 수 있다. 그리고 4개의 DoG로 부터 3개씩 짝지어 극값을 찾아준다. [그림3]을 보았을 때, X표시한 지점이 주변의 모든 26개의 픽셀값 보다 크거나, 작을때 즉, maximum 혹은 minimum일 때만 key points 후보가 선정된다는 것이다. 최종적으로, 한 Octave에서 2장의 극값 이미지가 나오고, 4개의 octave가 존재하므로, 총 8장의 극값 이미지가 나오게 된다.

[그림3]에 보면 **Scale**이란 단어가 보인다. [그림2]의 이미지를 보면 level1과 level3의 inner scale은 $\sigma, 2\sigma$로 두 배 차이가 난다. 이 때 이것이 [그림3]에서 적혀있는 scale이다. 앞으로 s라고 언급하겠다. 이렇게 **극값 계산시 3개의 level의 DoG이미지를 이용**하기 때문에, 참고한 논문에서는 한 옥타브당 level의 개수는 **s+3**으로 설정해야 한다고 하였다.

그리고 s=2라면, 세 개의 DoG이미지를 모아놨을 때 scale=2라는 뜻이므로, scale 변화비율 k는 $k=(2)\frac{1}{s}$를 만족해야 한다. 

### 4. Accurate keypoint localization

3번 과정을 통해서 key후보를 찾았다. 이 후보들 중에는 contrast가 너무 낮거나(그러므로, 노이즈에 민감하다)  DoG가 엣지에 민감하기 때문에, 엣지위에 존재하는 잘못된 key후보들이 있을수가 있다. 

#### 4.1 low contrast 해결

논문에서는 low contrast문제를 해결하기 위해서, 테일러 급수를 통해서 interpolation된 극값의 위치를  잡아내어 해당 위치($\hat{X}$)에서, $D(\hat{X})$의 값이 일정 값 이하이면 keypoint의 후보에서 reject하는 방법을 사용하기로 하였다. 실제로 이 방법은 효과적이었다고 하였다. 해당 과정의 식은 다음과 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_22.png" width="50%"></center>

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_6.png" width="60%"></center>

<center> [식1] DoG의 2차 미분 까지 전개한 테일러 급수 </center>

DoG를 테일러 급수를 이용하여 전개한 것은 [식1]과 같다.  위 식이 이해가 안가는 분은 [Khan academy의 강의](https://www.youtube.com/watch?v=ClFrIg0PpnM)를 보고 오면 좋을것 같다. 우리는 이제 테일러 급수의 극값의 위치를 구해야 한다. 그렇기 때문에, 테일러 급수의 미분식을 구한 후, 미분식=0 을 만족시키는 X값을 찾아야 한다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_7.png" width="35%"></center>

<center> [식2] [식1]의 미분 = 0 을 만족시키는 X값 </center>

Paper에는 위와같이 결과값만 나와있는데, 풀어서 전개를 해봤다. 전개해본 과정은 아래와 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_8.png" width="80%"></center>

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_9.png" width="80%"></center>

<center> [그림4] DoG의 테일러 급수의 극값의 위치를 찾는 과정 </center>

이렇게 해서 극값의 위치를 찾아내었다. 그런 다음, [식2]에서 얻은 값을, [식1]에 대입 해서 극값을 얻어내자. 그 다음 그 극값이 0.03보다 작으면 key 후보는 reject된다. 과정을 식으로 나타내면 다음과 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_10.png" width="60%"></center>

<center> [식3] 극값을 찾은([식1]을 [식2]에 대입)후 thresholding  </center>

#### 4.2 key candidates sensitive to edge 해결

그 다음으로는, edge에서 민감하게 반응하여 잘못 선출된 key후보를 제거해야 한다. 논문에서는 **poorly defined peak in DoG function will have a large principal curvature across the edge but a small one in the perpendicular direction.** 즉, 잘못 검출된 지점은 엣지에서는 큰 주곡률을 가지지만, 수직방향으로는 작은주곡률은 가진다고 번역이된다. 주곡률이 무엇인지는 잘 모르겠지만,  고유값이 주곡률을 의미한다고 한다. 그래서 위와같은 성질을 이용해서 논문에서는 고유값의 비율을 체크해서 일정값 이하면, 작은주곡률 혹은 균일한 주곡률을 가졌다 판단하고, 비율이 일정값 이상이면, 큰 주곡률과, 작은 주곡률을 가졌다고 판단하여 reject한다. 식은 아래와 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_11.png" width="45%"></center>

<center> [식4] DoG의 Hessian matrix (2차미분)  </center>

Hessian matrix의 고유값을 각각 $\alpha, \beta$라고 하자. 선형대수에서 배웠듯이 서Hessian matrix의 trace와 determinent를 다음과 같이 구할 수 있다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_12.png" width="60%"></center>

<center> [식5] Hessian matrix의 trace와 determinent  </center>

이 때, $\alpha > \beta$라 하자.  그리고, 임의의 상수 $r$에 대하여,  $\alpha = r\beta$를 만족한다고 하자. 이 때 다음과 같은 비율을 정의하자.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_13.png" width="60%"></center>

<center> [식6] 논문에서 제안된 두 주곡률 사이의 비율  </center>

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_14.png" width="45%"></center>

<center> [식7] 두 주곡률 사이의 비율과 thresholding  </center>

위에서 제시된 [식7]을 만족 시키면 accept 하고, 만족 시키지 않으면 reject한다. 이 때, 논문에서는 $r=10$이 적절한 값이라고 말하고 있다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_15.png" width="60%"></center>

<center> [그림5] 부적절한 key candidate 제거 후 결과 </center>

### 5. Orientation Assignment

이제 key point들을 추려냈으니,  rotation invariant를 위해서 keypoint에 방향을 할당할 차례다.  **key 를 중앙으로 하여** 주변으로 16X16의 윈도우를 설정한 뒤, 그 윈도우 안의 이미지를 Gaussian blurring을 수행한다. 그 후 gradient의 방향과 크기를 정해준다. 

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_16.png" width="80%"></center>

<center> [식8] L(blur된 window)에 대한 gradient의 크기 및 방향 계산식 </center>

이 때, Gaussian blurring을 할 때 논문에서는 다음과 같이 말하고 있다. **The scale of the keypoint is used to select the Gaussian smoothed image, L, with the closet scale, so that all computations are performed in a scale-invaraiant manner** 즉 keypoint의 scale에 따라 smoothing 하는 gaussian kernel의 분산값이 달라지는 말로 해석된다. 어떻게 달라지는 지는 코드를 참고해봐야 알 것 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_17.png" width="55%"></center>

<center> [그림6] Gradient 크기, 방향 계산 결과 이미지 </center>

그리고 Gradient의 방향과 크기가 계산된 후에, Gradient magnitude map(image)의 원래 keypoint의 scale의 1.5배 를 만드는 분산($\sigma$)을 가지는 가우시안 커널로 Gradient magnitude image를 convolution하여 magnitude값을 재설정한다. 위의 과정을 요약하자면 다음과 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_18.png" width="70%"></center>

<center> [그림7] Gradient magnitude map에 가우시안 커널로 weight를 넣어준 결과 </center>

1. key를 중심으로 16X16 윈도우 설정
2. 윈도우 Gaussian blurring(분산은 keypoint의 scale에 따라 다름)
3. Gradient의 방향과 크기 계산
4. keypoint의 scale의 1.5배의 분산을 지닌 가우시안 커널로 gradient magnitude image를 convolution 하여 magnitude값 재설정(가중, weighting)

읽으면서, 각 keypoint 들에 대해서 위와 같은 연산을 하고 있다는 것을 기억하면서 따라오자. 지금 까지 수행된 과정은 16X16 window의 각 픽셀의 gradient와 그 크기를 구하였다. 아직, keypoint에 대해서 방향을 할당하지 않았다. 이제 key point에 방향을 할당해보자.

0~360도를 기준으로, 36개의 bin을 설정하자. 즉 구간을 0~9, 10~19, ... , 350~359로 나누자는 뜻이다. 위에서 계산한 gradient 의 방향을 나타내는 이미지와 weighted gradient magnitude map에서 gradient크기 값을 사용해야한다. 만약에 (1,1) pixel의 gradient 방향이 15도 이고, weighted gradient크기가 20 이었다면, 10~10의 bin에 20만큼 채우는 것이다. 이렇게 모든 픽셀에 대해서 똑같은 과정을 반복하여 히스토그램을 형성한다. **만약에 최고점의 80%이상 되는 bin들이 더 존재한다면, 그 방향도 keypoint의 방향으로 취급한다.** 즉, 복수의 방향이 할당될 수 있다는 뜻이다. 히스토그램 형성 결과는 아래와 같다.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_19.png" width="90%"></center>

<center> [그림8] Keypoint 중심 윈도우들의 그레디언트 방향 히스토그램</center>

### 6. Key Point Descriptor

이제 keypoint에 descriptor를 만들어줄 차레다. decriptor를 번역해보자면 '설명자'정도로 할 수 있겠다. 그런데 생각해보면 이미 keypoint의 방향,크기 까지 구했는데 이걸로 keypoint를 설명할 수 있는거 아닌가? 이미 keypoint의 특징을 구했는데 이걸로 비교를 하면 되지 않을까? 라고 생각이 들것이다. [그림9]를 보고 얘기해보자.

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_20.png" width="80%"></center>

<center> [그림9] 왼: 원본그림의 keypoint, 오: 회전된 그림의 keypoint </center>

얼핏 보면 같아보인다. 그런데 과연 왼쪽과 오른쪽 그림의 keypoint를 비교하여 찾아낼 수 있을까? **우리가 두 그림이 같은 그림이고, 회전된 그림이라는 것**을 안다면 가능할 것이다. 왜냐하면 다시 돌려서 비교하면 되기 때문이다. 그런데 우리가 하려는 것은 그런게 아니다. 우리는 입력받는그림의 스케일이 다르던, 회전된 정도가 다르던 이러한 정보를 모르는 상태에서 매칭을 진행하고 싶은 것이다. 그렇다면 우리가 위에서 알아낸 것들은 도대체 무엇일까?  마치 절대좌표와 상대좌표같은 개념을 다루기 위한 keypoint의 정보(특징)을 알아낸 것이다. 예를 들어, 우리가 일반적으로 사용하는 데카르트 좌표계와 극좌표계, 아니면 선형대수에서 다루는 기저가 다르다고 할 수도 있을 것이다. 아니면 똑같인 1일이래도 지구에서 1일과, 화성에서 1일이 다르다는 이런 개념이다.

즉,**해당 이미지의 기준**만 고려해서 구해진 keypoint의 특징이라는 것이다.  그렇기 때문에 다른 상태에 놓인 이미지들과 비교해주기 위해서 다른 정보가 더 필요한데 그것이 바로 descriptor이다. 그렇다면 descriptor가 무엇인지 어떻게 decriptor 역할을 하는지 살펴보자.

5번 과정과 상당히 유사하다. 5번 과정과 똑같이 제일 먼저 **keypoint**를 기준으로 16X16윈도우를 만들어준다. 그리고, **window크기의 1/2인 값을** $\sigma$(여기서는 아마 8)로 가지는 Gaussian kernel을 **곱해주어 가중치를 준다.**  이는 keypoint로 부터의 거리에 따라 영향력을 달리하기 위함이다. 그 다음 16X16윈도우에서 똑같이 Gradient 계산을 해준다 그 후,  16X16 window를 16개의 4X4윈도우로 나눈다. 그 후, 4X4 윈도우에서 5번 과정에서 했던것과 똑같은 방식으로 히스토그램을 그린다. **이 때, 히스토그램 생성시 bin은 8개만! 만들어준다. 즉 0~44, 45~89, ... , 320~359로 나눈다.** 그렇게 하면  여기서는 5번 과정처럼 크기가 큰 방향만 고르지 않고 모든 방향을 모두 선택한다. 그렇게 한다면 총, 한 개의 4x4윈도우 에서 8개의 방향이 나오게 된다. 그렇게 된다면 16개의 윈도우 에서 총 128개의 feature vectors가 생성되는 것이다.

그리고 가장 중요한 과정이 남았다. **16X16 윈도우의 중심이 되는 keypoint가 있었단는 것을 다시 기억해내자.** 우리는 그 keypoint를 중심으로 16X16윈도우를 만들고 4X4 윈도우로 나누었다. 그 중5번 과정에서 keypoint의 방향과 크기를 구했었다. **이제 마지막으로 4X4윈도우에서 구한 vector들에서 중심이 되는 keypoint의 orientation vector를 빼주자.** 이렇게 한다면 rotation invariant한 성질이 완성된다. 

<center><img src="/public/img/Feature detector-SIFT(Scale Invariant Feature Transform)/img_21.png" width="90%"></center>

<center> [그림10] Descriptor 생성 결과 </center>

그리고 또한, 밝기에 불변한 값을 가지기 위해서 정규화(Normalization)과정이 필요하다 한다. feature vectors의 크기를 1로 만드는 과정이다.  추가적으로, 비선형적 밝기 변화에 대해서 값이 0.2 이상으로 나오면 제대로 검출이 안되기 때문이라고 한다. 그렇기 때문에, 최대값 max값을 찾고, 그 값을 a라고 할 때, 전체를 5a로 나눈다면 0.2이상의 값이 검출이 안될 것이다. 이렇게 처리하는 부분은 참고한 링크에서 인용하였다. 

### 7. Key matching & Clustering

Key matching은 key의 모든 descriptor간의 유클리디안 거리를 파악하여 key간의 거리를 파악하여 비교하는것 같습니다. 또한 이렇게 하여 가까운 key끼리 matching을 한 후, matching이 잘 이루어 졌는지 확인하기 위하여 GHT(Generalized Hough Transform)을 수행하여, matching된 부분이 centroid로 잘 대응되는지 inlier를 확인한다고 합니다.

유클리디안 거리가 뭔지도 알고, GHT가 무엇인지도 개략적으로 알지만, 구현부를 보지 않는 이상 석연치 않는 부분이 많은 것 같습니다. 아직 공부할게 많아서 시간을 내기 힘들겠지만, 무언가 SIFT의 다른 오픈소스를 보고 customizing을 해보는 것을 해본다면 많은 공부가 될 것 같다는 생각이 들어 꼭 시간을 내어 해보고 싶은 생각이 듭니다. SIFT에 관한 게시물은 이번에는 이것으로 마치며, 다음에 더 아는게 생기거나, 코드 customizing을 해볼 시, 추가 업데이트를 하도록 하겠습니다. 감사합니다.

## References

- [Distinctive Image Features from Scale-Invariant Keypoints](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf)
- [bskyvision 심교훈 - SIFT](https://bskyvision.com/21#comment7852883)
- [한국과학기술원 전기 및 전자공학과 최성필 님의 SIFT 정리자료](https://salkuma.files.wordpress.com/2014/04/sifteca095eba6ac.pdf)