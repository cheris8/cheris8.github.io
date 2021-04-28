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


