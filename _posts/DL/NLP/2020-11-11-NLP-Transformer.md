---
title: '[NLP] 트랜스포머 (Transformer) - 모델 아키텍처를 중심으로'

categories:
  - Deep Learning
tags:
  - Deep Learning
  - Natural Language Processing
  - Text Mining
  - Text Preprocessing

last_modified_at: 2020-11-11T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 트랜스포머의 모델 아키텍처를 중점적으로 다룬 기록입니다.

## Transformer

트랜스포머(Transformer)는 어텐션(Attention)만으로 인코더-디코더(Encoder-Decoder) 구조를 설계한 모델입니다.

![]({{site.url}}/assets/images/DL/NLP/transformer_attention_overview.PNG)

## Hyperparameter

- $d_{model}$ : 인코더와 디코더의 input 및 output의 크기 = 임베딩 벡터의 크기 (원 논문에서 $d_{model} = 512$)
- $num\\\_layers$ : 인코더와 디코더 각각을 몇 개 쌓을 것인지 (원 논문에서 $num\\\_layers = 6$)
- $num\\\_heads$ : 몇 개의 어텐션을 병렬로 수행할 것인지 (원 논문에서 $num\\\_heads = 8$)
- $d_{ff}$ : FFNN의 hidden layer의 차원 (원 논문에서 $d_{ff} = 2048$)

## Positional Encoding

단어를 순차적으로 입력 받아 단어의 위치 정보를 알 수 있는 RNN과 달리, 트랜스포머는 단어를 순차적으로 입력 받지 않고 문장 전체를 한번에 행렬로 입력 받아 단어의 위치 정보를 알 수 없습니다. 이러한 문제점을 해결하기 위해 트랜스포머에서는 각 단어의 임베딩 벡터에 위치 정보를 나타내는 벡터를 더하여 인코더와 디코더의 input으로 사용하는데, 이를 Positional Encoding이라고 합니다. 

트랜스포머는 위치 정보를 나타내는 값을 만들기 위해 아래의 두 개의 함수를 사용합니다.

$ PE_{(pos, 2i)} = \sin ( pos/10000^{2i/d_{model}} ) $  
$ PE_{(pos, 2i+1)} = \cos ( pos/10000^{2i/d_{model}} ) $

- $pos$ : position : 문장 행렬에서의 행 인덱스 : input 문장에서의 임베딩 벡터의 위치 인덱스 
- $i$ : dimension : 문장 행렬에서의 열 인덱스 : 임베딩 벡터 내의 차원의 인덱스
- $d_{model}$ : input 차원 = output 차원 = 임베딩 벡터의 차원 (원 논문에서 $d_model = 512$)

다시 말해, 임베딩 벡터 내의 차원의 인덱스가 짝수인 경우에는 사인 함수의 값을 위치 정보를 나타내는 값으로 사용하고, 임베딩 벡터 내의 차원의 인덱스가 홀수인 경우에는 코사인 함수의 값을 위치 정보를 나타내는 값으로 사용합니다. 이렇게 만든 행렬을 input 문장 행렬에 더함으로써 단어의 위치 정보를 알 수 있게 됩니다. 이를 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer7.PNG)

## Multi-Head Attention

Multi-Head Attention이란 여러 개의 어텐션을 병렬로 수행하는 것을 말합니다. 즉 $d_{model}$을 $num_heads$로 나눈 $d_{model} / num\\\_heads$ 차원을 갖는 $Q, K, V$에 대하여 $num\\\_heads$개의 어텐션을 병렬로 수행합니다. 이때 각 어텐션마다 결과로 얻어지는 어텐션 값 행렬을 어텐션 헤드(attention head)라고 합니다. 또 가중치 행렬 $W^Q, W^K, W^V$는 각 어텐션 헤드마다 다른 값을 갖습니다. 이를 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer17.PNG)

원 논문에서는 하이퍼파라미터 $d_{model} = 512$로, $num\\\_heads = 8$로 설정하였습니다. 즉 512차원을 갖는 각각의 단어 벡터들을 64차원을 갖는 $Q, K, V$ 벡터들로 바꾸고 8개의 어텐션을 병렬로 수행한 것입니다.

그러고 나서 $num\\\_heads$개의 어텐션으로부터 결과로 얻어지는 어텐션 헤드들을 모두 연결(concatenate)합니다. 어텐션 헤드를 모두 연결한 행렬의 차원은 $(seq\\\_len, \ d_{model})$이 됩니다.

![]({{site.url}}/assets/images/DL/NLP/transformer18_final.PNG)

여텐션 헤드를 모두 연결한 행렬에 가중치 행렬 $W^0$를 곱함으로써 최종 output을 얻을 수 있습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer19.PNG)

이때 최종 output의 차원 $(seq\\\_len, \ d_{model})$이 input의 차원 $(seq\\\_len, \ d_{model})$과 동일함을 확인할 수 있습니다.

---

**Multi-Head Attention으로 얻을 수 있는 효과** : Multi-Head Attention은 어텐션을 병렬로 수행하여 다양한 시각에서 정보를 수집할 수 있도록 합니다. 예를 들어 "The animal didn't cross the street because it was to tired."라는 문장이 있다고 가정해봅시다. 단어 "it"이 Query라고 한다면, "it"에 대한 $Q$ 벡터로부터 다른 단어들과의 연관도를 구할 수 있습니다. 이때 첫번째 어텐션 헤드는 "it"과 "animal"의 연관도를 높게 볼 수 있고, 두번째 어텐션 헤드는 "it"과 "tired"의 연관도를 높게 볼 수 있습니다. 각각의 어텐션 헤드가 모두 서로 다른 시각에서 보고 있기 때문입니다.

## Self-Attention

트랜스포머의 Self-Attention에서의 $Q, K, V$는 아래와 같습니다.

- $Q$ : input 문장의 모든 단어 벡터들
- $K$ : input 문장의 모든 단어 벡터들
- $V$ : input 문장의 모든 단어 벡터들

먼저, 각 단어 벡터에 가중치 행렬을 곱하여 $Q$ 벡터, $K$ 벡터, $V$ 벡터를 구합니다. 이때 각 단어 벡터들의 차원은 $d_{model}$이고 각 가중치 행렬들의 차원은 $(d_{model}, \ d_{model} / num\\\_heads)$이므로, $Q$ 벡터, $K$ 벡터, $V$ 벡터의 차원은 $d_{model} / num\\\_heads$입니다. 이때 $K$ 벡터의 차원을 $d_k$, $V$ 벡터의 차원을 $d_V$로 표기합니다. 가중치 행렬은 train 과정에서 학습합니다.

예를 들어, 단어 "student"에 대한 단어 벡터에 가중치 행렬 $W^Q, W^K, W^V$를 곱하여 $Q$ 벡터, $K$ 벡터, $V$ 벡터를 만드는 과정을 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer11.PNG)

- 단어 벡터의 차원 : $d_{model}$
- 가중치 행렬 $W^Q, \ W^K, \ W^V$의 차원 : $( d_{model}, \ d_{model} / num\\\_heads) = (512, \ 64)$
- $Q$ 벡터 즉 $Q_{student}$의 차원 : $d_{model} / {num\\\_heads} = 512 / 8 = 64$
- $K$ 벡터 즉 $K_{student}$의 차원 : $d_{model} / {num\\\_heads} = 512 / 8 = 64$
- $V$ 벡터 즉 $V_{student}$의 차원 : $d_{model} / {num\\\_heads} = 512 / 8 = 64$

---

다음으로, 어텐션 함수를 사용하여 어텐션 스코어(attention score)를 계산합니다. 이때 사용되는 어텐션 함수는 아래와 같습니다.

$score(q,k) = q \cdot k / \sqrt{n}$

- $q$ : $Q$ 벡터 (행 벡터) : $d_{model} / {num\\\_heads} = 512 / 8 = 64$ 차원
- $k$ : $K^T$ 벡터 (열 벡터) : $d_{model} / {num\\\_heads} = 512 / 8 = 64$ 차원
- $\sqrt{n} = \sqrt{d_k} = 8$ (사용자 설정)

이를 Scaled dot-product Attention이라고 합니다. 이때 $Q$ 벡터 즉 행 벡터와 $K$ 벡터의 transpose 즉 열 벡터의 내적이므로 결과값은 스칼라가 됩니다. 

예를 들어, 단어 "I"를 기준으로 어텐션 스코어를 계산하는 과정을 그림으로 나타내면 다음과 같습니다. 

![]({{site.url}}/assets/images/DL/NLP/transformer13.PNG)

---

그러고 나서 소프트맥스 함수를 사용하여 어텐션 분포(attention distribution)를 구하고, 이를 각 $V$벡터와 가중합(weighted sum)을 하여 어텐션 값(attention value)를 계산합니다.

예를 들어, 단어 "I"를 기준으로 어텐션 분포를 구하고 어텐션 값을 계산하는 과정을 그림으로 나타내면 다음과 같습니다. 

![]({{site.url}}/assets/images/DL/NLP/transformer14_final.PNG)

---

실제로 Self-Attention은 벡터 단위가 아닌 행렬 단위로 수행됩니다. 이에 대해 설명해보고자 합니다.

**먼저, 문장 행렬에 가중치 행렬을 곱하여 $Q$ 행렬, $K$ 행렬, $V$ 행렬을 구합니다.** $seq\\\_len$은 문장의 길이=단어의 개수=임베딩 벡터의 개수를 의미합니다.

![]({{site.url}}/assets/images/DL/NLP/transformer12.PNG)

- 문장 행렬의 차원 : $(seq\\\_len, \\ d_{model})$
- 가중치 행렬 $W^Q$의 차원 : $(d_{model}, \\ d_k) = (d_{model}, \\ d_{model} / {num\\\_heads})$
- 가중치 행렬 $W^K$의 차원 : $(d_{model}, \\ d_k) = (d_{model}, \\ d_{model} / {num\\\_heads})$
- 가중치 행렬 $W^V$의 차원 : $(d_{model}, \\ d_v) = (d_{model}, \\ d_{model} / {num\\\_heads})$
- $Q$ 행렬의 차원 : $(seq\\\_len, \\ d_k) = (seq\\\_len, \\ d_{model} / {num\\\_heads})$
- $K$ 행렬의 차원 : $(seq\\\_len, \\ d_k) = (seq\\\_len, \\ d_{model} / {num\\\_heads})$
- $V$ 행렬의 차원 : $(seq\\\_len, \\ d_v) = (seq\\\_len, \\ d_{model} / {num\\\_heads})$

**다음으로, 어텐션 함수를 사용하여 어텐션 스코어(attention score)를 계산합니다.** 즉 $Q$ 행렬과 $K$ 행렬의 transpose를 곱한 후 모든 값을 $\sqrt{n} = \sqrt{d_k}$로 나눕니다. 그 결과, \각 행과 열이 어텐션 스코어 값을 갖는 행렬 즉 어텐션 스코어 행렬이 됩니다. 

![]({{site.url}}/assets/images/DL/NLP/transformer15.PNG)

예를 들어 "I" 행과 "student" 열의 값은 단어 "I"의 $Q$ 벡터와 단어 "student"의 $K$ 벡터의 어텐션 스코어 값과 동일합니다.

**그러고 나서 소프트맥스 함수를 사용하여 어텐션 분포(attention distribution)를 구하고, 이를 $V$ 행렬과 곱하여 어텐션 값 행렬을 구합니다.**

![]({{site.url}}/assets/images/DL/NLP/transformer16.PNG)

이를 수식으로 표현하면 다음과 같습니다.

$Attention (Q,K,V) = softmax ( \frac{Q K^T}{\sqrt{d_k}} ) V$

- $Q$ 행렬의 차원 : $(seq\\\_len, \ d_k) = (seq\\\_len, \ d_{model} / {num\\\_heads})$
- $K$ 행렬의 차원 : $(seq\\\_len, \ d_k) = (seq\\\_len, \ d_{model} / {num\\\_heads})$
- $V$ 행렬의 차원 : $(seq\\\_len, \ d_v) = (seq\\\_len, \ d_{model} / {num\\\_heads})$
- 어텐션 값 행렬 $a$의 차원 : $(seq\\\_len, \ d_v)$

---

**Self-Attention으로 얻을 수 있는 효과** : 예를 들어 "The animal didn't cross the street because it was to tired."라는 문장이 있다고 가정해봅시다. 이 문장에서 "it"이 "animal"을 의미하는 것인지 "street"을 의미하는 것인지 불분명할 수 있습니다. 이때 Self-Attention은 input 문장 내의 단어들 간의 유사도를 구함으로써 "it"이 "animal"과 관련이 있을 확률이 높다는 것을 알 수 있습니다.

## Padding Mask

**트랜스포머의 인코더와 디코더의 어텐션 layer에서는 Padding Mask를 사용합니다.** Padding Masking은 input 문장에 \<\pad> 토큰이 존재할 경우 이를 어텐션에서 제외하기 위한 연산입니다.

예를 들어, <\pad> 토큰이 존재하는 문장을 input으로 하여 어텐션을 수행할 때 어텐션 스코어 행렬은 아래와 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-pad_masking11.PNG)

그런데 <\pad> 토큰의 경우에는 실질적인 의미를 가지고 있지 않습니다. 그래서 트랜스포머에서는 Key에 <\pad> 토큰이 존재할 경우 이에 대한 유사도는 구하지 않도록 이를 마스킹(masking) 합니다. 이는 어텐션 스코어 행렬에서 <\pad> 토큰의 위치에 매우 작은 음수 값을 할당함으로써 가능합니다. 다시 말해 어텐션 스코어 행렬에서 행은 Query이고 열은 Key이므로, Key에 <\pad> 토큰이 존재하는 경우에는 해당 열 전체에 매우 작은 음수 값을 할당합니다. 이를 그림으로 나타내면 아래와 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-pad_masking2.PNG)

위 그림은 소프트맥스 함수를 지나기 전 어텐션 스코어 행렬을 보여줍니다. 그러고 나서 이 어텐션 스코어 행렬은 소프트맥스 함수를 지나게 됩니다. 소프트맥스 함수를 지난 후 어텐션 스코어 행렬을 그림으로 나타내면 아래와 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-softmax.PNG)

즉 소프트맥스 함수를 지나면 <\pad> 토큰의 경우 값이 0이 되어 어떠한 유의미한 값도 가지고 있지 않게 됩니다.

## Look-ahead Mask

**트랜스포머의 디코더의 첫번째 sub-layer에서는 Look-ahead Mask을 사용합니다.** 단어를 매 시점(time step)마다 순차적으로 입력으로 받는 RNN 계열의 신경망과 달리, 트랜스포머는 문장 행렬 전체를 한번에 입력으로 받습니다. 따라서 단어를 예측할 때, 현재 시점 이전의 단어들만 참고할 수 있는 RNN과 달리, 트랜스포머는 문장 행렬 전체로부터 미래 시점의 단어들까지도 참고할 수 있다는 문제가 발생합니다. 이러한 문제를 해결하고자 트랜스포머는 디코더의 첫번째 sub-layer에서 look-ahead mask를 사용합니다. 다시 말해 어텐션 스코어 행렬에서 자기 자신보다 미래에 있는 단어들에 대하여 마스킹을 적용합니다.

예를 들어, 다음과 같이 어텐션을 수행하여 어텐션 스코어 행렬을 구한다고 가정해봅시다.

![]({{site.url}}/assets/images/DL/NLP/transformer-decoder_attention_score_matrix.PNG)

이제 현재 시점보다 미래 시점에 있는 단어들을 참고하지 못하도록 다음과 같이 마스킹(masking) 합니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-look_ahead_mask.PNG)

그 결과 어텐션 스코어 행렬의 각 행 즉 Query을 보면 현재 시점과 현재 시점 이전의 단어들만을 참고할 수 있게 됨을 확인할 수 있습니다.

## 어텐션 layer 종류

### Encoder의 첫번째 sub-layer : Multi-Head Self-Attention layer

- Self-Attention O
  - Query : 인코더의 이전 layer의 output
  - Key : 인코더의 이전 layer의 output
  - Value : 인코더의 이전 layer의 output
- Padding Mask 사용 O
- Look-ahead Mask 사용 X

### Decoder의 첫번째 sub-layer : Masked Multi-Head Self-Attention layer

- Self-Attention O
  - Query : 디코더의 이전 layer의 output
  - Key : 디코더의 이전 layer의 output
  - Value : 디코더의 이전 layer의 output
- Padding Mask 사용 O
- Look-ahead Mask 사용 O

### Decoder의 두번째 sub-layer : Multi-Head Attention layer : Encoder-Decoder Attention

- Self-Attention X
  - Query : 디코더의 이전 layer의 output
  - Key : 인코더의 output
  - Vale : 인코더의 output
- Padding Mask 사용 O
- Look-ahead Mask 사용 X

Query가 디코더 행렬이고 Key가 인코더 행렬일 때 어텐션 스코어 행렬을 구하는 과정은 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-decoder_sublayer_attention_score.PNG)

## Position-wise Feed-Forward Neural Networks ; FFNN

**트랜스포머의 인코더에서는 두번째 sub-layer로, 디코더에서는 세번째 sub-layer로 Position-wise Feed-Forward Neural Networks를 갖습니다.** FFNN은 fully-connected feed-forward network로, 각각의 position에 개별적으로 그리고 동일하게 적용됩니다. FFNN은 선형 변환, ReLU 활성화 함수, 선형 변환으로 구성됩니다. 이를 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-positionwiseffnn.PNG)

이를 수식으로 나타내면 다음과 같습니다.

$ FFNN(x) = \max (0, \ x W_1 + b_1 ) W_2 + b_2 $

- $x$ : 이전의 sub-layer의 output 행렬
- $x$의 차원 : $(seq\\\_len, \ d_{model})$
- 가중치 행렬 $W_1$의 차원 : $(d_{model}, \ d_{ff})$
- 가중치 행렬 $W_2$의 차원 : $(d_{ff}, \ d_{model})$

이때 $W_1, b_1, W_2, b_2$는 같은 인코더 내에서는 position 즉 다른 문장과 다른 단어에 관계없이 서로 같은 값을 갖지만, 다른 인코더 간에는 서로 다른 값을 갖습니다. 또 원 논문에서 input과 output의 차원은 $d_{model} = 512$이고, hidden layer의 차원은 $d_{ff} = 2048$입니다.

## Add & Norm : 잔차 연결(Residual connection) & 층 정규화(Layer Normalization)

**트랜스포머의 인코더와 디코더에서는 각각의 sub-layer에 추가적으로 Add & Norm을 사용합니다.** 구체적으로, Add는 잔차 연결(Residual connection)을, Norm은 층 정규화(Layer Normalization)를 의미합니다.

### 잔차 연결 (Residual Connection)

잔차 연결(Residual connection)은 input $x$와 output $F(x)$를 더하는 것을 말합니다. 이를 그림으로 나타내면 다음과 같습니다.

![]({{site.url}}/assets/images/DL/NLP/transformer22.PNG)

즉, 트랜스포머의 인코더와 디코더에서는 각 sub-layer마다 sub-layer의 input $x$와 sub-layer의 output $Sublayer(x)$를 더하는 연산을 수행합니다. 이를 수식으로 나타내면 다음과 같습니다.

$x + Sublayer(x)$

### 층 정규화 (Layer Normalization)

층 정규화는 크게 두가지 단계로 나누어집니다. 먼저 평균과 분산을 이용하여 정규화를 수행합니다. 그러고 나서 감마와 베타를 이용하여 정규화를 수행합니다.

우선, 평균과 분산을 이용한 정규화에 대해 살펴보겠습니다. 아래의 그림과 같은 화살표 방향의 벡터를 각각 $x_i$로 표기합니다. 그리고 화살표 방향으로 각각 평균 $\mu_i$과 분산 $\sigma_i^2$을 구합니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-layer_norm_new_2_final.PNG)

이 벡터 $x_i$를 평균 $mu_i$와 분산 $\sigma_i^2$을 이용하여 정규화 합니다. 

$ \hat{x}\_{i,k} = \frac{x_{i,k} - \mu_i}{\sqrt{\sigma_i^2 + \epsilon}} $

- $i$ : 행렬 내에서 행 인덱스 : 벡터의 인덱스
- $k$ : 행렬 내에서 열 인덱스 : 벡터 내 차원의 인덱스
- $\mu_i$ : 평균 (스칼라)
- $\sigma_i^2$ : 분산 (스칼라)
- $\epsilon$ : 분모가 0이 되는 것을 방지하기 위한 값

즉 $i$번째 벡터의 $k$번째 차원의 값인 $x_{i, k}$ 각각이 $\hat{x}\_{i,k}$로 정규화 되는 것입니다.

다음으로, 감마와 벡터를 이용한 정규화에 대해 살펴보겠습니다. 이를 수식으로 나타내면 다음과 같습니다.

$ln_i = \gamma \hat{x}_i + \beta = LayerNorm(x_i)$

이때 $\gamma$는 1을 초기값으로 갖는 벡터이고 $\beta$는 0을 초기값을 갖는 벡터입니다. $\gamma$와 $\beta$는 학습 가능한 파라미터입니다.

![]({{site.url}}/assets/images/DL/NLP/transformer-gamma_beta.PNG)

최종적으로 잔차 연결(residual connection)과 층 정규화(layer normalization)을 거친 output은 다음과 같습니다.

$LayerNorm(x + Sublayer(x))$

## 참고자료

[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)  
[딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/book/2155)
