---
title: '[NLP] 트랜스포머 (Transformer)'

categories:
  - Natural Language Processing
tags:
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing
  - Deep Learning

last_modified_at: 2020-11-11T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 원 논문인 [Attention Is All You Need](https://arxiv.org/pdf/1409.0473.pdf)을 공부한 기록입니다.

## 0. Abstract

지배적인 sequence transduction models(시퀀스 전달 모델)은 인코더와 디코더를 포함하는 복잡한 순환 신경망 (Recurrent Neural Network) 또는 합성곱 신경망(Convolution Neural Network)을 기반으로 한다. 가장 성능이 좋은 모델들 또한 어텐션 메커니즘을 통해 인코더와 디코더를 연결한다. 우리는 순환 신경망과 합성곱 신경망을 완전히 없애고 오직 어텐션 메커니즘에 기반하는, 새로운 단순한 네트워크 아키텍처인 트랜스포머를 제안한다.

두 개의 기계 번역 task에 대한 실험은 트랜스포머가 더 parallelizable하고 train하는 데에 훨씬 더 적은 시간을 필요로 하는 반면 품질 퀄리티에 있어서는 더 우수하다는 것을 보여준다. 우리의 모델은 WMT 2014 English- to-German translation task에서 28.4 BLEU를 달성하여 앙상블을 포함한 기존 최고의 결과를 2 BLEU 이상 향상시켰다. 
WMT 2014 영어-프랑스어 번역 과제에서 우리의 모델은 8개의 GPU에서 3.5일 동안 training한 후 41.0의 새로운 단일 모델 BLEU 점수를 설정한다.

WMT 2014 English-to-French translation task에서 GMT 8일 동안 교육을 받은 후 41.0의 새로운 단일 모델 BLEU 점수를 설정한다. 최고의 모델들의 훈련 비용.

## 1. Introduction

RNN, LSTM, GRU는 언어 모델링과 기계 번역과 같은 sequence modeling과 transduction 문제에서 최신식의 접근으로 확실하게 자리잡았다.
그 이후로 수많은 노력들이 recurrent language models과 encoder-decoder architectures의 경계를 넓히기 위해 계속되어 왔다.

Recurrent models는 
반복 모델은 일반적으로 입력input 및 출력output 시퀀스의 symbol 위치를 따라 계산을 인자화한다. 위치를 계산 시간의 스텝에 맞춰 조정하면서, Recurrent models는 
이전 hidden state $h_{t-1}$과 위치 $t$에 대한 input의 함수로서 hidden states $h_t$라는 시퀀스를 생성한다. 

이러한 본질적으로 순차적 특성은 훈련 예시 내에서 병렬화를 배제하며, 이는 메모리 제약이 예시 전체에 걸친 일괄 처리를 제한하기 때문에 더 긴 시퀀스 길이에 중요해진다. 최근의 연구는 인자화 요령[18]과 조건부 연산[26]을 통해 연산 효율을 크게 향상시키는 한편 후자의 경우 모델 성능도 향상시켰다. 그러나 순차적 계산의 근본적인 제약은 여전하다.

## 2. Background


주의 메커니즘은 입력 또는 출력 시퀀스의 거리와 무관하게 종속성을 모델링할 수 있도록 다양한 작업에서 매력적인 시퀀스 모델링 및 변환 모델의 필수적인 부분이 되었다[2, 16]. 그러나 몇 가지 경우를 제외하고 모두 [22], 그러나 그러한 주의 메커니즘은 반복적인 네트워크와 함께 사용된다.
이 작업에서 우리는 모델 아키텍처인 Transformer를 제안한다. Transformer는 재발을 방지하고 대신 입력과 출력 사이의 전지구적 의존성을 끌어내기 위한 주의 메커니즘에 전적으로 의존한다. 트랜스포머는 8개의 P100 GPU에서 12시간 정도 훈련을 받은 후에 훨씬 더 많은 병렬화가 가능하며 번역 품질에서 새로운 예술 상태에 도달할 수 있다.

## 3. Model Architecture

대부분의 경쟁적 신경 시퀀스 전달 모델은 인코더-데코더 구조를 가지고 있다[5, 2, 29]. 여기서 인코더는 입력된 기호 표시 순서(x1,...,xn)를 연속 표시 z = (z1,...,zn)로 매핑한다. 그런 다음 디코더는 한 번에 한 요소씩 기호의 출력 시퀀스(y1, ..., ym)를 생성한다. 각 단계에서 모델은 자동 억제 [9]로, 이전에 생성된 기호를 다음 단계를 생성할 때 추가 입력으로 소비한다.
Transformer는 각각 그림 1의 왼쪽과 오른쪽 절반에 나타난 인코더와 디코더 모두에 대해 누적된 자기 주의력과 점으로 완전히 연결된 층을 사용하여 이 전체 아키텍처를 따른다.

### 3.1 Encoder and Decoder Stacks

인코더: 
인코더는 N = 6개의 동일한 레이어 스택으로 구성된다. 각 층에는 두 개의 층이 있다.
부차적인 첫 번째는 다중 헤드 자기 주의 메커니즘이고, 두 번째는 단순하고 위치적으로 완전히 연결된 피드-포워드 네트워크다. 우리는 두 하위 레이어 각각에 대한 잔존 연결[10]을 사용하고, 그 다음에 계층 정규화[1]을 사용한다. 즉, 각 하위 계층의 출력은 LayerNorm(x + 하위 계층(x))이며, 여기서 Sublayer(x)는 하위 계층 자체에 의해 구현되는 기능이다. 이러한 잔류 연결을 용이하게 하기 위해, 내장 레이어뿐만 아니라 모델의 모든 하위 레이어는 치수 dmodel = 512의 출력을 생성한다.

디코더: 
디코더는 또한 N = 6개의 동일한 레이어 스택으로 구성되어 있다. 디코더는 각 인코더 층에 있는 두 개의 서브 레이어 외에 세 번째 서브 레이어를 삽입하는데, 이 세 번째 서브 레이어는 인코더 스택의 출력에 대해 멀티헤드 주의를 수행한다. 인코더와 유사하게, 우리는 각 서브레이어 주변에 잔여 연결을 사용하고, 그 다음에 레이어 정규화를 사용한다. 우리는 또한 디코더 스택의 자기 주의 하위 계층을 수정하여 후속 포지션에 대한 포지션의 참석을 방지한다. 이 마스킹은 출력 임베딩이 한 위치에 의해 상쇄된다는 사실과 결합되어 위치 예측이 i보다 작은 위치에서 알려진 출력에만 의존할 수 있음을 보장한다.

### 3.2 Attention

주의 기능은 쿼리, 키, 값 및 출력이 모두 벡터인 출력에 쿼리와 키 값 쌍 집합을 매핑하는 것으로 설명할 수 있다. 출력은 값들의 가중치 합으로 계산되며, 여기서 각 값에 할당된 가중치는 해당 키와의 쿼리의 호환성 함수에 의해 계산된다.

#### 3.2.1 Scaled Dot-Product Attention

우리는 우리의 특별한 관심을 "스케일 도트-제품 주의"라고 부른다(그림 2). 입력은 치수 dk의 쿼리와 키, 치수 dv의 값으로 구성된다. 우리는 쿼리의 도트 제품을 모든 키로 계산하고, 각각을 값으로 나눈다.
dk, 그리고 소프트맥스 함수를 적용하여 에 대한 가중치를 구한다.
실제로, 우리는 매트릭스 Q로 함께 포장된 질의 집합에서 동시에 주의 기능을 계산한다. 또한 키와 값은 행렬 K와 V로 함께 포장된다. 우리는 다음과 같이 출력 매트릭스를 계산한다.

$tlr : Attention(Q, K, V ) = softmax( √
$

가장 일반적으로 사용되는 두 가지 주의 기능은 첨가 주의 [2]와 도트 제품(다중)이다.
dk
양면적인) 주의 Dot-product 주의는 스케일링 계수를 제외하고 우리의 알고리즘과 동일하다.
dk
한 겹의 숨은 층 두 사람은 이론적 복잡성이 비슷한 반면, 점제품에 대한 관심은 다음과 같다.
고도로 최적화된 매트릭스 곱셈 코드를 사용하여 구현될 수 있기 때문에 훨씬 더 빠르고 공간 효율성이 높다.
dk의 작은 값의 경우 두 메커니즘이 유사한 성능을 발휘하는 반면, 부가적인 주의력은 보다 우수하다.
dk [3]의 더 큰 값에 대해 스케일링 없이 제품에 주의를 집중한다. 우리는 의 많은 가치에 대해
.
dk, dot 제품은 크기가 커져서 소프트맥스 기능을 41개가 있는 지역으로 밀어넣는다.
극히 작은 구배 이러한 효과를 상쇄하기 위해  we 주의력만큼 도트 제품을 스케일링한다.
dk

#### 3.2.2 Multi-Head Attention

#### 3.2.3 Applications of Attention in our Model

### 3.3 Position-wise Feed-Forward Networks

## 3.4 Embeddings and Softmax

## 3.5 Positional Encoding

# 4 Why Self-Attention
# 5 Training