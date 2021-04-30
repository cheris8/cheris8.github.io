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

http://taewan.kim/post/cnn/

[블로그 글](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)
을 공부한 기록입니다.


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

### 합성곱, Convolution
합성곱, Convolution의 위키피디아 정의는 다음과 같습니다.

합성곱 연산은 두 함수 f, g 가운데 하나의 함수를 반전(reverse), 전이(shift)시킨 다음, 다른 하나의 함수와 곱한 결과를 적분하는 것을 의미한다. 출처: https://ko.wikipedia.org/wiki/%ED%95%A9%EC%84%B1%EA%B3%B1
위키피디아의 정의는 상당히 난해합니다. <그림 1>은 2차원 입력데이터(Shape: (5,5))를 1개의 필터로 합성곱 연산을 수행하는 과정을 소개합니다. 합성곱 처리 결과로 부터 Feature Map을 만듭니다.

1.2 채널, Channel

이미지 픽셀 하나하나는 실수입니다. 컬러 사진은 천연색을 표현하기 위해서, 각 픽셀을 RGB 3개의 실수로 표현한 3차원 데이터입니다.(<그림 2> 참조) 컬러 이미지는 3개의 채널로 구성됩니다. 반면에 흑백 명암만을 표현하는 흑백 사진은 2차원 데이터로 1개 채널로 구성됩니다. 높이가 39 픽셀이고 폭이 31 픽셀인 컬러 사진 데이터의 shape은 (39, 31, 3)3으로 표현합니다. 반면에 높이가 39픽셀이고 폭이 31픽셀인 흑백 사진 데이터의 shape은 (39, 31, 1)입니다.

Convolution Layer에 유입되는 입력 데이터에는 한 개 이상의 필터가 적용됩니다. 1개 필터는 Feature Map의 채널이 됩니다. Convolution Layer에 n개의 필터가 적용된다면 출력 데이터는 n개의 채널을 갖게 됩니다.

1.3 필터(Filter) & Stride

필터는 이미지의 특징을 찾아내기 위한 공용 파라미터입니다. Filter를 Kernel이라고 하기도 합니다. CNN에서 Filter와 Kernel은 같은 의미입니다. 필터는 일반적으로 (4, 4)이나 (3, 3)과 같은 정사각 행렬로 정의됩니다. CNN에서 학습의 대상은 필터 파라미터 입니다. <그림 1>과 같이 입력 데이터를 지정된 간격으로 순회하며 채널별로 합성곱을 하고 모든 채널(컬러의 경우 3개)의 합성곱의 합을 Feature Map로 만듭니다. 필터는 지정된 간격으로 이동하면서 전체 입력데이터와 합성곱하여 Feature Map을 만듭니다. <그림 3>은 채널이 1개인 입력 데이터를 (3, 3) 크기의 필터로 합성곱하는 과정을 설명합니다.


필터는 입력 데이터를 지정한 간격으로 순회하면서 합성곱을 계산합니다. 여기서 지정된 간격으로 필터를 순회하는 간격을 Stride라고 합니다. <그림 4>는 strid가 1로 필터를 입력 데이터에 순회하는 예제입니다. strid가 2로 설정되면 필터는 2칸씩 이동하면서 합성곱을 계산합니다.


입력 데이터가 여러 채널을 갖을 경우 필터는 각 채널을 순회하며 합성곱을 계산한 후, 채널별 피처 맵을 만듭니다. 그리고 각 채널의 피처 맵을 합산하여 최종 피처 맵으로 반환합니다. 입력 데이터는 채널 수와 상관없이 필터 별로 1개의 피처 맵이 만들어 집니다. <그림 5 참조>

하나의 Convolution Layer에 크기가 같은 여러 개의 필터를 적용할 수 있습니다. 이 경우에 Feature Map에는 필터 갯수 만큼의 채널이 만들어집니다. 입력데이터에 적용한 필터의 개수는 출력 데이터인 Feature Map의 채널이 됩니다.

Convolution Layer의 입력 데이터를 필터가 순회하며 합성곱을 통해서 만든 출력을 Feature Map 또는 Activation Map이라고 합니다. Feature Map은 합성곱 계산으로 만들어진 행렬입니다. Activation Map은 Feature Map 행렬에 활성 함수를 적용한 결과입니다. 즉 Convolution 레이어의 최종 출력 결과가 Activation Map입니다.

1.4 패딩(Padding)

Convolution 레이어에서 Filter와 Stride에 작용으로 Feature Map 크기는 입력데이터 보다 작습니다. Convolution 레이어의 출력 데이터가 줄어드는 것을 방지하는 방법이 패딩입니다. 패딩은 입력 데이터의 외각에 지정된 픽셀만큼 특정 값으로 채워 넣는 것을 의미합니다. 보통 패딩 값으로 0으로 채워 넣습니다.


<그림 6>은 (32, 32, 3) 데이터를 외각에 2 pixel을 추가하여 (36, 36, 3) 행렬을 만드는 예제입니다. Padding을 통해서 Convolution 레이어의 출력 데이터의 사이즈를 조절하는 기능이 외에, 외각을 “0”값으로 둘러싸는 특징으로 부터 인공 신경망이 이미지의 외각을 인식하는 학습 효과도 있습니다.

1.5 Pooling 레이어

풀링 레이어는 컨볼류션 레이어의 출력 데이터를 입력으로 받아서 출력 데이터(Activation Map)의 크기를 줄이거나 특정 데이터를 강조하는 용도로 사용됩니다. 플링 레이어를 처리하는 방법으로는 Max Pooling과 Average Pooning, Min Pooling이 있습니다. 정사각 행렬의 특정 영역 안에 값의 최댓값을 모으거나 특정 영역의 평균을 구하는 방식으로 동작합니다. <그림 7>은 Max pooling과 Average Pooling의 동작 방식을 설명합니다. 일반적으로 Pooing 크기와 Stride를 같은 크기로 설정하여 모든 원소가 한 번씩 처리 되도록 설정합니다.


Pooling 레이어는 Convolution 레이어와 비교하여 다음과 같은 특징이 있습니다.

학습대상 파라미터가 없음
Pooling 레이어를 통과하면 행렬의 크기 감소
Pooling 레이어를 통해서 채널 수 변경 없음

CNN에서는 주로 Max Pooling을 사용합니다.

2. 레이어별 출력 데이터 산정

Convloution 레이어와 Pooling 레이버의 출력 데이터 크기를 계산하는 방법을 소개합니다.

2.1 Convolution 레이어 출력 데이터 크기 산정

입력 데이터에 대한 필터의 크기와 Stride 크기에 따라서 Feature Map 크기가 결정됩니다. 공식은 다음과 같습니다.

입력 데이터 높이: H
입력 데이터 폭: W
필터 높이: FH
필터 폭: FW
Strid 크기: S
패딩 사이즈: P

식 1. 출력 데이터 크기 계산
O
u
t
p
u
t
H
e
i
g
h
t
=
O
H
=
(
H
+
2
P
−
F
H
)
S
+
1
O
u
t
p
u
t
W
e
i
g
h
t
=
O
W
=
(
W
+
2
P
−
F
W
)
S
+
1


<식 1>의 결과는 자연수가 되어야 합니다. 또한 Convolution 레이어 다음에 Pooling 레이어가 온다면, Feature Map의 행과 열 크기는 Pooling 크기의 배수여야 합니다. 만약 Pooling 사이즈가 (3, 3)이라면 위 식의 결과는 자연수이고 3의 배수여야 합니다. 이 조건을 만족하도록 Filter의 크기, Stride의 간격, Pooling 크기 및 패딩 크기를 조절해야 합니다.

2.2 Pooling 레이어 출력 데이터 크기 산정

Pooling 레이어에서 일반적인 Pooling 사이즈를 정사각형입니다. Pooling 사이즈를 Stride 같은 크기로 만들어서, 모든 요소가 한번씩 Pooling되도록 만듭니다. 입력 데이터의 행 크기와 열 크기는 Pooling 사이즈의 배수(나누어 떨어지는 수)여야 합니다. 결과적으로 Pooling 레이어의 출력 데이터의 크기는 행과 열의 크기를 Pooling 사이즈로 나눈 몫입니다. Pooling 크기가 (2, 2) 라면 출력 데이터 크기는 입력 데이터의 행과 열 크기를 2로 나눈 몫입니다. pooling 크기가 (3, 3)이라면 입력데이터의 행과 크기를 3으로 나눈 몫이 됩니다. <식 2 참조>


