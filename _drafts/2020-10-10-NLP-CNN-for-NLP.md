---
title: '[자연어처리] CNN for NLP'

categories:
  - Natural Language Processing
tags:
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing
  - Deep Learning

last_modified_at: 2020-10-06T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 스탠포드 대학의 [cs224n](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/) 강의와 

[블로그 글](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)
을 공부한 기록입니다.

## 1. CNN for NLP 등장 배경

**RNN 문제점**

Recurrent neural nets cannot capture phrases without prefix context
Often capture too much of last words in final vector
E.g., softmax is often only calculated at the last step

## 2. CNN for NLP 개념



Main CNN/ConvNet idea:
• What if we compute vectors for every possible word subsequence of a certain length?
• Example: “tentative deal reached to keep government open” computes vectors for:
• tentative deal reached, deal reached to, reached to keep, to keep government, keep government open
• Regardless of whether phrase is grammatical
• Not very linguistically or cognitively plausible
• Then group them afterwards (more soon) 7

분류 문제 (감성분석, 스팸 탐지, 주제 분류 등)에 주로 활용
CNN이란
Window가 slideing하며 convolution 연산이 수행되는 과정을 포함한 네트워크
Convolution이란
필터를 움직이면서 weight를 곱한 후 더하는 “가중합(Weighted Sum)” 연산


![]({{site.url}}/assets/images/Convolution_schematic.gif)


CNN for NLP에서의 input은 문장 혹은 문서 전체의 matrix 형태가 된다.
matrix의 각 행은 하나의 token (이때 token은 주로 단어 혹은 문자)
즉 matrix의 각 행은 각각의 단어 벡터 (임베딩 벡터 혹은 원-핫 벡터)

예를 들어 단어 벡터의 차원이 100이고, 단어의 수가 10이라면
matrix 즉 input은 10 x 100 크기를 가짐

## 3. CNN for NLP 하이퍼파리미터(Hyperparameter)

- Input representation : 원-핫 벡터 혹은 임베딩 벡터 (Word2Vec, GloVe 등)
- Padding
- Filter(=kernel) size
- Stride size
- Pooling 
- Channels
- Activation function : 시그모이드 함수, 소프트맥스 함수, 하이퍼볼릭탄젠트 함수

### Padding ; 패딩

padding은 input의 가장자리에 특정 값을 갖는 행과 열을 추가해주는 것을 말합니다. 주로 가장자리에 0을 채우는 zero padding을 사용합니다. 이를 그림으로 나타내면 아래와 같습니다.

![]({{site.url}}/assets/images/conv10.png)

padding을 사용하는 이유는 크게 두가지가 있습니다. 먼저, padding을 통해 **모든 matrix의 모든 요소에 대해 convolution을 수행할 수 있게 됩니다.** 또한, padding을 하면 **input size와 동일한 혹은 좀 더 큰 output size를 얻을 수 있습니다.**

$ n_{out} = \left( n_{in} + 2 \times n_{padding} − n_{filter} \right) + 1 $

padding을 사용하지 않은 것을 narrow convolution, padding을 사용한 것을 wide convolution이라고 합니다. 이를 그림으로 나타내면 아래와 같습니다.

![]({{site.url}}/assets/images/Screen-Shot-2015-11-05-at-9.47.41-AM-1024x261.png)

왼쪽은 zero padding을 사용하지 않은 narrow convolution을, 오른쪽은 zero padding을 사용한 wide convolution을 보여줍니다. 왼쪽 narrow convolution의 output size는 $\left( 7 − 5 \right) + 1 = 3 $이 됩니다. 한편 오른쪽 wide convolution의 output size는 $\left( 7 + 2 \times 4 − 5 \right) + 1 = 11$이 됩니다.
 
### Filter(=kernel) size ; 필터의 크기

CV(Computer Vision) 분야에서 2d convolution을 사용하는 것과 달리, NLP 분야에서는 1d convolution을 사용합니다. 따라서 필터의 너비는 input인 matrix의 너비 즉 단어 벡터의 차원(dimension)과 같습니다. 필터의 높이(region size)는 보통 2~5개의 단어를 슬라이딩 할 수 있도록 2~5 정도로 설정합니다.

### Stride size

stride size는 각 단계에서 필터를 얼마만큼 움직일 것인지를 의미합니다. 

![]({{site.url}}/assets/images/Screen-Shot-2015-11-05-at-10.18.08-AM-1024x251.png)

왼쪽은 stride size를 1로, 오른쪽은 stirde size를 2로 한 예시를 보여줍니다. 보통 stirde size를 1로 설정하는데, stride size가 클수록 필터는 더 적게 사용되고 output size는 작아집니다. 더 큰 stirde size를 사용할수록 Recursive Neural Network와 비슷한 효과를 내 tree 구조를 만듭니다.

### Pooling

pooling은 subsample 혹은 downsaple과 같은 개념이라고 볼 수 있습니다. 주로 convolution layer 다음에 pooling layer가 적용되어, input 혹은 feature map(convolution layer의 결과) 값을 subsample하는 것을 말합니다.
CNN의 핵심은 pooling layer이다. 주로 convolution layer 다음에 적용된다. 
보통 일반적으로는 max pooling을 사용한다. Pooling은 전체 matrix에 대해서 적용할 필요없이 window 크기를 정하고 해당 window에 대해서 pooling을 진행하면 된다. 

아래의 그림은 2x2 window로 max pooling을 하는 것을 보여줍니다.


pooling은 고정된 크기의 output을 만들 수 있고, 각각 길이가 다른 문장들을 input으로 넣을 수 있도록 해주고, 크기(dimension)가 줄어도 중요한 정보는 모두 보존한다는 점에서 CNN의 핵심적인 역할을 합니다. 

- max pooling
- average pooling
- local max pooling
- k-max pooling

### 6. Channels

channel은 input이 무엇인지에 따라 개념이 달라질 수 있음!

CV에서는
컬러 이미지를 input으로 할 때 RGB 3개의 값을 각각 하나의 channel로 가짐

NLP에서는
Word2Vec으로 임베딩 한 벡터들이 모인 matrix를 하나의 channel로, GloVe로 임베딩 한 벡터들이 모인 matrix를 또 하나의 channel로 가짐
같은 문장에 대해 서로 다른 언어로 표현한 것을 각 channel로 가짐




