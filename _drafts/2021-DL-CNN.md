---
title: '[NLP] CNN (Convolution Neural Networks) ; 합성곱 신경망'

categories:
  - Deep Learning
tags:
  - Deep Learning
  - Natural Language Processing

last_modified_at: 2020-10-03T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 RNN의 개념, 구조, 순전파, 역전파, 그리고 파이토치를 활용한 RNN 구현에 관한 기록입니다.

을 공부한 기록입니다.

### CNN 개념

CNN은 이미지를 다루는 데에 활용되는 DNN입니다.

예를 들어, 아래와 같이 `4x4x3` RGB 이미지가 있다고 가정해봅시다.

![4x4x3 RGB image]({{site.url}}/assets/images/CNN-input-image.jpeg)

CNN의 목표는 예측에 도움이 되는 특징(feature)의 손실 없이 이미지를 줄이는 것입니다. 이를 위해 CNN에서는 Convolution layer와 Relu layer, Pooling layer를 통해 feature를 학습하고, Fully connected layer를 통해 이미지를 분류합니다. 이를 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/CNN-overview.jpeg)

즉 CNN은크게 Convolution layer, Pooling layer, Full-connected layer 세가지 layer로 구성됩니다.

> `[((CONV => RELU) * m => POOL) * n => (FC => RELU) * k ] m=1~5, n=큰 수, k=0~2`

- Convolution Layer
  - Filter 개수
  - Filter 크기
  - Stride
- Pooling Layer
- Fully-Connected Layer

## CNN의 아키텍처


*예제: 아래에서 더 자세하게 배우겠지만, CIFAR-10 데이터를 다루기 위한 간단한 ConvNet은 `INPUT-CONV-RELU-POOL-FC` 로 구축할 수 있다.

- INPUT 입력 이미지가 가로32, 세로32, 그리고 RGB 채널을 가지는 경우 입력의 크기는 `32 x 32 x 3`
- CONV 레이어 : 입력 이미지의 일부 영역과 연결되어 있으며, 이 연결된 영역과 자신의 가중치의 내적 연산 (dot product) 을 계산하게 된다. 결과 볼륨은 `32x32x12`와 같은 크기를 갖게 된다.
- RELU 레이어 : max(0,x)와 같이 각 요소에 적용되는 액티베이션 함수 (activation function)이다. 이 레이어는 볼륨의 크기를 변화시키지 않는다 `32x32x12`
- POOL 레이어 : (가로,세로) 차원에 대해 다운샘플링 (downsampling)을 수행해 `16x16x12`와 같이 줄어든 볼륨을 출력한다.
- FC (fully-connected) 레이어 : 클래스 점수들을 계산해 `1x1x10`의 크기를 갖는 볼륨을 출력한다. 10개 숫자들은 10개 카테고리에 대한 클래스 점수에 해당한다. 레이어의 이름에서 유추 가능하듯, 이 레이어는 이전 볼륨의 모든 요소와 연결되어 있다.


- Convolution 합성곱
- Channel 채널
- Filter 필터
- kernel 커널
- Strid 스트라이드
- Padding 패딩
- Feature Map 피쳐맵
- Activation Map
- Pooling


Padding

$P=\frac{K−1}{2}$
$O = \frac{(W − K + 2P)}{S} + 1 S
$O$: output height/length
$W$ : input height/length
$K$: filter size
$P$: padding size
$S$: stride size

## Convolution ; 합성곱

합성곱(convolution)은 이미지로부터 특징(feature)을 추출하기 위해 사용되는 연산입니다. 이를 그림으로 나타내면 다음과 같습니다.

![Convoluting a 5x5x1 image with a 3x3x1 kernel to get a 3x3x1 convolved feature]({{site.url}}/assets/images/CNN-overview.jpeg)

- 초록색 부분 : `5x5x1` 입력 이미지
- 노란색 부분 : `3x3x1` 필터 ; Filter = Kernerl
- 빨간색 숫자 : Filter의 값 = weight parameter (학습 가능 파라미터)
- 빨간색 부분 : Feature Map

위 그림에서는 `5x5x1` 입력 이미지에 대하여 `3x3` 필터(filter size=3)를 1개 사용하여 한칸씩 움직이며(stride=1) 매번 이미지의 일부와 필터 간 matrix operation을 수행하여 최종적으로 `3x3` 특성맵을 추출합니다. 이때 matrix operation, 즉 각 값들끼리 곱하고 이를 모두 더하는 연산을 convolution이라고 합니다. 필터는 왼쪽 상단에서부터 오른쪽 하단으로 이동하는데, 위 그림에서 필터는 총 9번 이동합니다.


<!--
위 그림에서는 `5x5x1` 입력 이미지에 대하여 `3x3` 필터(filter size=3)를 1개 사용하여 한칸씩 움직이며(stride=1) 매번 해당 영역 내 이미지의 픽셀 값과 filter의 값을 곱하고 곱한 값을 모두 더하여 최종적으로 `3x3` 특성맵을 추출합니다. 이때 각 값들끼리 곱하고 이를 모두 더하는, 이미지 일부와 filter 간 matrix operation 연산을 convolution이라고 합니다.
-->

filter 
filter size
stride : 필터를 움직이는 간격
feature map : convolution 수행 결과

### Filter = Kernel

위에서 Convolution을 설명할 때 이미지에서 특징을 추출하기 위해 여러 가지 값이 정해진 Filter 를 사용했었다. 이전처럼 고정된 Filter가 아니라 학습을 통해 만들어지는 Filter를 사용한다는 점에서 다르다. 그래서 CNN을 적용하고자 하는 문제에 따라 Filter 값이 달라질 수 있다.

또한, 위에서 확인해 본 것처럼 하나의 이미지에도 다양한 Filter를 사용하면 다양한 특징들이 추출된다. 그렇다면 CNN에서는 몇 개의 Filter를 사용하는게 적절할까? 그리고 각 Filter의 크기는 어떤 것이 적절할까?

### Filter의 개수

Filter의 개수를 정하는 데에 왕도는 없습니다. 다만 일반적으로는 입력 이미지 근처에서는 적게 사용하고, 멀어질수록 더 많이 사용하는 경향이 있다.

Filter의 개수를 결정하는 일반적인 방법은 각 Layer에서 연산 시간/양을 일정하게 유지하여 시스템의 균형을 맞추는 방향으로 결정된다. 즉, 각 Layer에서 Feature map (Feature map은 보통 이미지를 Filter로 Convolution한 결과를 의미함)의 개수와 Pixel 수의 곱을 일정하게 유지할 수 있게 Filter 개수를 결정하면 된다.

예를 들면, Convolution layer에서 2x2 Subsampling을 하는 경우 Pixel 수가 1/4로 줄어들기 때문에 그 다음 Convolution 할 때는 Filter 수를 4배로 증가시켜 Feature map 수를 4배로 증가시키면 된다.

### Filter의 크기

Filter의 크기는 여러 논문에서 다양한 형태로 나타나는데, 이미지의 크기가 클수록 더 큰 Filter를 사용한다. 그렇다면 큰 크기의 Filter를 하나 사용하는 것과 작은 크기의 Filter를 여러 개 사용하는 것 중에 뭐가 더 좋을까?

정답은 작은 크기의 Filter를 여러 개 사용하는 것이다. 여러 개를 사용하면 중간에 비선형화 과정을 통해 특징을 더 돋보이게 만들 수 있다 (이해 X). 또한, 연산량도 더 줄일 수 있다. 조금 더 자세한 내용은 추후에 다룰 예정이니 우선 결론을 알고 있자.

이 외에도 Filter로 Convolution 시 고려할 수 있는 Hyperparameter로 Stride와 Zero padding이 있다.

### Stride

Stride는 Convolution 시 건너 뛸 픽셀 수를 의미한다. 아래의 이미지는 Stride가 1인 경우다.


그렇다면 Stride 값이 커지면 어떻게 될까? Stride 값이 커지면 중복되는 부분이 줄어들고 Convolution이 시도되는 범위가 줄어, Local feature의 특성을 다 고려하지 못한 Global feature가 만들어질 수도 있다.

그래서 통상적으로 Stride를 1로 두고 Subsampling 작업을 하지만, 입력 영상 크기가 큰 경우 연산량을 줄이기 위한 목적으로 입력 Layer 가까운 쪽에서 적용하기도 한다 (AlexNet).


### 1.3 필터(Filter) & Stride

필터는 이미지의 특징을 찾아내기 위한 공용 파라미터입니다. Filter를 Kernel이라고 하기도 합니다. CNN에서 Filter와 Kernel은 같은 의미입니다. 필터는 일반적으로 (4, 4)이나 (3, 3)과 같은 정사각 행렬로 정의됩니다. CNN에서 학습의 대상은 필터 파라미터 입니다. <그림 1>과 같이 입력 데이터를 지정된 간격으로 순회하며 채널별로 합성곱을 하고 모든 채널(컬러의 경우 3개)의 합성곱의 합을 Feature Map로 만듭니다. 필터는 지정된 간격으로 이동하면서 전체 입력데이터와 합성곱하여 Feature Map을 만듭니다. <그림 3>은 채널이 1개인 입력 데이터를 (3, 3) 크기의 필터로 합성곱하는 과정을 설명합니다.

필터는 입력 데이터를 지정한 간격으로 순회하면서 합성곱을 계산합니다. 여기서 지정된 간격으로 필터를 순회하는 간격을 Stride라고 합니다. <그림 4>는 strid가 1로 필터를 입력 데이터에 순회하는 예제입니다. strid가 2로 설정되면 필터는 2칸씩 이동하면서 합성곱을 계산합니다.

입력 데이터가 여러 채널을 갖을 경우 필터는 각 채널을 순회하며 합성곱을 계산한 후, 채널별 피처 맵을 만듭니다. 그리고 각 채널의 피처 맵을 합산하여 최종 피처 맵으로 반환합니다. 입력 데이터는 채널 수와 상관없이 필터 별로 1개의 피처 맵이 만들어 집니다. <그림 5 참조>

하나의 Convolution Layer에 크기가 같은 여러 개의 필터를 적용할 수 있습니다. 이 경우에 Feature Map에는 필터 갯수 만큼의 채널이 만들어집니다. 입력데이터에 적용한 필터의 개수는 출력 데이터인 Feature Map의 채널이 됩니다.

Convolution Layer의 입력 데이터를 필터가 순회하며 합성곱을 통해서 만든 출력을 Feature Map 또는 Activation Map이라고 합니다. Feature Map은 합성곱 계산으로 만들어진 행렬입니다. Activation Map은 Feature Map 행렬에 활성 함수를 적용한 결과입니다. 즉 Convolution 레이어의 최종 출력 결과가 Activation Map입니다.

### 1.2 채널, Channel

이미지 픽셀 하나하나는 실수입니다. 컬러 사진은 천연색을 표현하기 위해서, 각 픽셀을 RGB 3개의 실수로 표현한 3차원 데이터입니다.(<그림 2> 참조) 컬러 이미지는 3개의 채널로 구성됩니다. 반면에 흑백 명암만을 표현하는 흑백 사진은 2차원 데이터로 1개 채널로 구성됩니다. 높이가 39 픽셀이고 폭이 31 픽셀인 컬러 사진 데이터의 shape은 (39, 31, 3)3으로 표현합니다. 반면에 높이가 39픽셀이고 폭이 31픽셀인 흑백 사진 데이터의 shape은 (39, 31, 1)입니다.

Convolution Layer에 유입되는 입력 데이터에는 한 개 이상의 필터가 적용됩니다. 1개 필터는 Feature Map의 채널이 됩니다. Convolution Layer에 n개의 필터가 적용된다면 출력 데이터는 n개의 채널을 갖게 됩니다.

### Padding ; 패딩

패딩은 입력 데이터의 외곽에 지정된 픽셀만큼 특정 값으로 채워넣는 것을 의미합니다. 보통 패딩 값으로 0을 사용합니다.

Convolution 레이어에서 Filter와 Stride의 영향으로 Feature Map의 크기는 입력 이미지의 크기보다 작습니다. 이때 이러한 Convolution 레이어의 출력 데이터가 줄어드는 것을 방지하는 방법이 패딩입니다. 

<그림 6>은 (32, 32, 3) 데이터를 외각에 2 pixel을 추가하여 (36, 36, 3) 행렬을 만드는 예제입니다. Padding을 통해서 Convolution 레이어의 출력 데이터의 사이즈를 조절하는 기능이 외에, 외각을 “0”값으로 둘러싸는 특징으로 부터 인공 신경망이 이미지의 외각을 인식하는 학습 효과도 있습니다.

### Zero padding

Zero padding은 Convolution 후 Feature map의 크기가 입력 크기보다 작아지는 것을 막기 위해 사용하는 방법이다. 말 그대로 입력 이미지의 경계에 0을 추가해 Convolution 하더라도 크기가 유지되게 만드는 방법이다.

위 그림에서 보는 것처럼 Zero padding을 추가하면, Convolution을 하더라도 기존 이미지와 크기가 동일한 Feature map이 만들어지는 것을 볼 수 있다. 그리고 Zero padding을 하는 이유는 Convolution 후 Feature map 크기를 입력 이미지 크기로 유지하는 것도 있지만, 경계면의 정보 획득할 수 있다는 점도 있다.


### Pooling layer

Pooling Layer는 Convolution Layer의 출력을 입력으로 받아 크기를 줄이거나 특정 데이터를 강조하는 용도로 사용됩니다.

![3x3 pooling over 5x5 convolved feature]({{site.url}}/assets/images/CNN-pooling.gif)

이를 통해 차원을 축소할 수 있고, 지배적인 특징(feature)를 추출할 수 있습니다.


| |Convolution Layer | Pooling Lyaer |
|-|------------------|---------------|
|학습 대상 파라미터|있음 | 없음|
|행렬 크기 | |행렬의 크기 감소|
|채널 수 ||채널 수 변경 없음|

일반적으로 Pooing 크기와 Stride를 같은 크기로 설정하여 모든 원소가 한 번씩 처리 되도록 설정합니다.

![Types of Pooling]({{site.url}}/assets/images/CNN-pooling-types.png)

Pooling의 종류에는 크게 Max Pooling, Average Pooling, Min Pooling이 존재합니다.

- Max Pooling : 영역 내 최대값 반환
- Average Pooling : 영역 내 평균값 반환
- Min Pooling : 영역 내 최소값 반환

CNN에서는 일반적으로 Max Pooling을 사용합니다.

## Fully connected layer



## 2. 레이어별 출력 데이터 산정

Convloution 레이어와 Pooling 레이버의 출력 데이터 크기를 계산하는 방법을 소개합니다.

2.1 Convolution 레이어 출력 데이터 크기 산정

입력 데이터에 대한 필터의 크기와 Stride 크기에 따라서 Feature Map 크기가 결정됩니다. 공식은 다음과 같습니다.

입력 데이터 높이: H
입력 데이터 폭: W
필터 높이: FH
필터 폭: FW
Strid 크기: S
패딩 사이즈: P


<식 1>의 결과는 자연수가 되어야 합니다. 또한 Convolution 레이어 다음에 Pooling 레이어가 온다면, Feature Map의 행과 열 크기는 Pooling 크기의 배수여야 합니다. 만약 Pooling 사이즈가 (3, 3)이라면 위 식의 결과는 자연수이고 3의 배수여야 합니다. 이 조건을 만족하도록 Filter의 크기, Stride의 간격, Pooling 크기 및 패딩 크기를 조절해야 합니다.

2.2 Pooling 레이어 출력 데이터 크기 산정

Pooling 레이어에서 일반적인 Pooling 사이즈를 정사각형입니다. Pooling 사이즈를 Stride 같은 크기로 만들어서, 모든 요소가 한번씩 Pooling되도록 만듭니다. 입력 데이터의 행 크기와 열 크기는 Pooling 사이즈의 배수(나누어 떨어지는 수)여야 합니다. 결과적으로 Pooling 레이어의 출력 데이터의 크기는 행과 열의 크기를 Pooling 사이즈로 나눈 몫입니다. Pooling 크기가 (2, 2) 라면 출력 데이터 크기는 입력 데이터의 행과 열 크기를 2로 나눈 몫입니다. pooling 크기가 (3, 3)이라면 입력데이터의 행과 크기를 3으로 나눈 몫이 됩니다. <식 2 참조>

## 다양한 CNN 계열 아키텍처

There are various architectures of CNNs available which have been key in building algorithms which power and shall power AI as a whole in the foreseeable future. Some of them have been listed below:

- LeNet
- AlexNet
- VGGNet
- GoogLeNet
- ResNet
- ZFNet

## 참고자료

실제로 합성곱 신경망 (Convolutional Neural Network, CNN)은 1989년 LeCun의 논문 “Backpropagation applied to handwritten zip code recognition”
[Backpropagation applied to handwritten zip code recognition]()
[A Comprehensive Guide to Convolutional Neural Networks — the ELI5 way](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)
[](http://taewan.kim/post/cnn/)