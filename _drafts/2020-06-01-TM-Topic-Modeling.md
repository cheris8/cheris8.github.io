---
title: '[텍스트 마이닝] 토픽 모델링 (Topic Modeling)'

categories:
  - Text Mining
tags:

last_modified_at: 2020-07-31T08:06:00-05:00

use_math: true
---

# 1. 토픽모델

기계 학습 및 자연언어 처리 분야에서 토픽 모델(Topic model)이란 문서 집합의 추상적인 "주제"를 발견하기 위한 통계적 모델 중 하나로, 텍스트 본문의 숨겨진 의미구조를 발견하기 위해 사용되는 텍스트 마이닝 기법 중 하나입다. 특정 주제에 관한 문헌에서는 그 주제에 관한 단어가 다른 단어들에 비해 더 자주 등장할 것이다. 예를 들어 개에 대한 문서에서는 "개"와 "뼈다귀"라는 단어가 더 자주 등장하는 반면, 고양이에 대한 문서에서는 "고양이"와 "야옹"이 더 자주 등장할 것이고, "그", "~이다"와 같은 단어는 양쪽 모두에서 자주 등장할 것이다. 이렇게 함께 자주 등장하는 단어들은 대게 유사한 의미를 지니게 되는데 이를 잠재적인 "주제"로 정의할 수 있다. 즉, "개"와 "뼈다귀"를 하나의 주제로 묶고, "고양이"와 "야옹"을 또 다른 주제로 묶는 모형을 구상할 수 있는데 바로 이것이 토픽 모델의 개략적인 개념이다. 실제로 문헌 내에 어떤 주제가 들어있고, 주제 간의 비중이 어떤지는 문헌 집합 내의 단어 통계를 수학적으로 분석함으로써 알아 낼 수 있다.

그렇기에 토픽 모델은 또한 확률적 토픽 모델이라고도 불리는데, 이는 광범위한 텍스트 본문의 잠재적 의미 구조를 발견하기 위한 통계적 알고리즘을 가리키는 의미로도 쓰인다. 정보화 시대가 도래하면서 매일 생성되는 텍스트는 인간이 직접 처리할 수 있는 양을 크게 넘어서는데, 토픽 모델은 자동적으로 비정형 텍스트의 집합을 이해하기 쉽도록 조직하고 정리하는 데에 쓰일 수 있다. 또한 토픽 모델은 원래 개발된 목적인 텍스트 마이닝 분야 이외에도 유전자 정보, 이미지, 네트워크와 같은 자료에서 유의미한 구조를 발견하는데에도 유용하게 사용되고 있다. 또한 생물정보학과 같은 응용분야에서도 널리 사용되고 있다.[1]

# 토픽 모델링의 정의

토픽 모델링 (Topic Modeling)
구조화되지 않은 방대한 문헌집단에서 주제를 찾아내기 위한 알고리즘
주제: 같은 맥락에서 나타날 가능성이 있는 단어들을 그룹화 함
맥락과 관련된 단서들을 이용하여 유사한 의미를 가진 단어들을 클러스터링하는 방식으로 주제를 추론하는 모델




토픽 모델링 (Topic Modeling)
문헌 모델링과 문헌집단 모델링을 위한 기법
Generative Model 중의 일종
Generative Model: 어떤 확률분포와 그 파라미터가 있다고 할 때, 그로부터 랜덤 프로세스에 따라 데이터를 생성하는 관점의 모델




토픽 모델링 (Topic Modeling)
문헌 생성 모델



토픽 모델링의 문제점
문헌 내에서의 용어 분포는 알 수 있지만 주제들(Topic 1, Topic 2)의 용어분포는 사전에 알 수 없음

따라서 직접 관찰할 수 있는 문헌집단 내 각 문헌들의 용어분포들로부터 주제의 용어분포를 추정하는 과정 필요
잠재 디리클레 할당(Latent Dirichlet Allocatoin : LDA) 기법 제안



# 잠재 디리클레 할당 (Latent Dirichlet Allocation ; LDA)

LDA는 토픽 모델링 기법 중 텍스트 마이닝 분석에서 가장 많이 활용되고 있는 문헌 생성 모델(generative probabilistic model)입니다. LDA를 이루는 이론적 기초는 베이즈 추론이고, 베이즈 추론에 사용되는 분포 중 하나인 디리클레 분포이며, LDA에 쓰이는 확률 식을 추정하기 위해 깁스 샘플링라는 방법 사용합니다.


이번 글에서는 코퍼스로부터 토픽을 추출하는 토픽 모델링 (Topic Modeling) 기법 가운데 하나인 잠재 디리클레 할당 (Latent Dirichlet Allocation ; LDA)에 대해 살펴보도록 하겠습니다.


## 모델 개요

LDA란 주어진 문헌에 대하여 각 문서에 어떤 주제들이 존재하는지에 대한 확률모형입니다. LDA는 토픽 별 단어의 분포, 문헌 별 토픽의 분포를 모두 추정해 냅니다. LDA의 개략적인 도식은 다음과 같습니다.


우선 LDA는 특정 토픽에 특정 단어가 나타날 확률을 내어줍니다. 예컨대 위 그림에서 노란색 토픽엔 gene이라는 단어가 등장할 확률이 0.04, dna는 0.02, genetic은 0.01입니다. 이 노란색 토픽은 대략 ‘유전자’ 관련 주제라는 걸 알 수 있네요.

이번엔 문서를 보겠습니다. 주어진 문서를 보면 파란색, 빨간색 토픽에 해당하는 단어보다는 노란색 토픽에 해당하는 단어들이 많네요. 따라서 위 문서의 메인 주제는 노란색 토픽(유전자 관련)일 가능성이 큽니다. 이렇듯 문서의 토픽 비중 또한 LDA의 산출 결과물입니다.

위 그림 우측에 있는 ‘Topic proportions & assignments’가 LDA의 핵심 프로세스입니다. LDA는 문서가 생성되는 과정을 확률모형으로 모델링한 것이기 때문인데요.

LDA도 마찬가지입니다. 우선 말뭉치로부터 얻은 토픽 분포로부터 토픽을 뽑습니다. 이후 해당 토픽에 해당하는 단어들을 뽑습니다. 이것이 LDA가 가정하는 문헌 생성 과정입니다.

이제 반대 방향으로 생각해보겠습니다. 현재 문헌에 등장한 단어들은 어떤 토픽에서 뽑힌 단어들일까요? 이건 명시적으로 알기는 어렵습니다. 말뭉치에 등장하는 단어들 각각에 꼬리표가 달려있는 건 아니니까요.

그런데 LDA는 이렇게 말뭉치 이면에 존재하는 정보를 추론해낼 수 있습니다. LDA에 잠재(Latent)라는 이름이 붙은 이유입니다. LDA의 학습은 바로 이러한 잠재정보를 알아내는 과정입니다.

## LDA and probabilist models

$$p( )$$

$\beta_1:K$ : topics
$\beta_k$ : a distribution over the vocabulary (the distributions over words at left in Figure 1).

$\theta_d$ : the topic proportions for the dth document
$\theta_d,k$ : the topic proportion for topic k in document d (the car- toon histogram in Figure 1).

$z_d$ : the topic assignments for the dth document
$z_d,n$ : the topic assignment for the nth word in document d (the colored coin in Figure 1).

$w_d$ : the observed words for document d
$w_d,n$ : the nth word in document d, which is an element from the fixed vocabulary.


## The graphical model for LDA


each node is a random variable and is labeled according to its role in the generative process (see figure 1).

the hidden nodes—the topic proportions, assignments, and topics—are unshaded.

the observed nodes—the words of the documents—are shaded.

the rectangles are “plate” notation, which denotes replication.

$N$ plate : the collection words within documents

$D$ plate : the collection of documents within the collection.





## 모델 아키텍처



LDA의 아키텍처, 즉 LDA가 가정하는 문헌 생성 과정은 다음과 같습니다.
$D$ : 말뭉치 전체 문헌 개수,
$K$ : 전체 토픽 수 (하이퍼 파라미터),
$N$ : $d$번째 문서의 단어 수를 의미합니다.
네모칸은 해당 횟수만큼 반복하라는 의미이며 동그라미는 변수를 가리킵니다.
화살표가 시작되는 변수는 조건, 화살표가 향하는 변수는 결과에 해당하는 변수입니다.

우리가 관찰 가능한 변수는 $d$번째 문서에 등장한 $n$번째 단어 $w_{d,n}$
이 유일합니다(음영 표시). 우리는 이 정보만을 가지고 하이퍼파라메터(사용자 지정) α,β
를 제외한 모든 잠재 변수를 추정해야 합니다. 앞으로 이 글에서는 이 그림을 기준으로 설명할 예정이기 때문에 잘 기억해두시면 좋을 것 같습니다.


LDA의 문서생성과정은 다음과 같습니다. 이는 저도 정리 용도로 남겨두는 것이니 스킵하셔도 무방합니다.

(1) Draw each per-corpus topic distributions $ϕk~Dir(β) for k∈{1,2,…K}$

(2) For each document, Draw per-document topic proportions $θd~Dir(α)$

(3) For each document and each word, Draw per-word topic assignment $zd,n
~Multi(θd)$

(4) For each document and each word, Draw observed word $wd,n
~Multi(ϕzd,n,n)$


## LDA 모델의 변수


우선 각 변수를 설명하겠습니다. ϕk
는 k
번째 토픽에 해당하는 벡터입니다. 말뭉치 전체의 단어 개수만큼의 길이를 가졌습니다. 예컨대 ϕ1
은 아래 표에서 첫번째 열입니다. 마찬가지로 ϕ2
는 두번째, ϕ3
은 세번째 열벡터입니다. ϕk
의 각 요소값은 해당 단어가 k
번째 토픽에서 차지하는 비중을 나타냅니다. ϕk
의 각 요소는 확률이므로 모든 요소의 합은 1이 됩니다(아래 표 기준으로는 열의 합이 1).


## LDA의 infernece

지금까지 LDA가 가정하는 문서생성과정과 잠재변수들이 어떤 역할을 하는지 설명했습니다. 이제는 $w_{d,n}$를 가지고 잠재변수를 역으로 추정하는 inference 과정을 살펴보겠습니다. 다시 말해 LDA는 토픽의 단어분포와 문서의 토픽분포의 결합으로 문서 내 단어들이 생성된다고 가정합니다. LDA의 inference는 실제 관찰가능한 문서 내 단어를 가지고 우리가 알고 싶은 토픽의 단어분포, 문서의 토픽분포를 추정하는 과정입니다.

여기에서 LDA가 가정하는 문서생성과정이 합리적이라면 해당 확률과정이 우리가 갖고 있는 말뭉치를 제대로 설명할 수 있을 것입니다. 바꿔 말해 토픽의 단어분포와 문서의 토픽분포의 결합확률이 커지도록 해야 한다는 이야기입니다. 확률과정과 결합확률을 각각 그림과 수식으로 나타내면 다음과 같습니다.
($z_{d,n}$ : per-word topic assignment,
$\theta_d$ : per-document topic proportions,
$\phi_k$ : per-corpus topic distributions)


위 수식에서 사용자가 지정한 하이퍼파라메터 α,β
와 우리가 말뭉치로부터 관찰가능한 wd,n
을 제외한 모든 변수가 미지수가 됩니다. 따라서 우리는 사후확률(posterior) p(z,ϕ,θ
|w)
를 최대로 만드는 z,ϕ,θ
를 찾아야 합니다. 이것이 LDA의 inference입니다. 그런데 사후확률을 계산하려면 분모에 해당하는 p(w)
를 반드시 구해야 합니다. 이는 베이즈 정리에서 evidence로 불리는 것으로, p(w)
는 잠재변수 z,ϕ,θ
의 모든 경우의 수를 고려한 각 단어(w
)의 등장 확률을 가리킵니다. 그러나 z,ϕ,θ
는 우리가 직접 관찰하는 게 불가능할 뿐더러, p(w)
를 구할 때 z,ϕ,θ
의 모든 경우를 감안해야 하기 때문에, 결과적으로 p(w)
를 단번에 계산하는 것이 어렵습니다. 이 때문에 깁스 샘플링 같은 기법을 사용하게 됩니다. 깁스 샘플링 관련 자세한 내용은 이곳을 참고하시면 좋을 것 같습니



## 베이즈 추론

베이즈 추론의 순서는 다음과 같습니다.
1. 어떤 사건이 발생할 확률을 가정한다.
2. 그리고 추가적인 관측이 발생하면
3. 관측된 사건을 통해 그 사건이 발생할 확률을 더 정확하게 추론한다.

사전 확률 (prior probability, 선험적 확률): 관측에 의거하지 않고 가정하는 특정 사건이 일어날 확률.
관측: 사건이 실제로 발생한 것.
사후 확률 (posterior probability): 실제 관측된 사건을 가지고 해당 사건이 일어날 확률을 더 정확하게 계산한것.
사후 확률 ∝ 가능도 (likelihood) × 사전 확률


## 켤레 사전 분포 (Conjugate Prior)

특정 사건과 짝을 이루는 사전 확률 분포

LDA (Latent Dirichlet Allocation)
이미 관찰된 변수(observed variable)를 통해 각각의 확률을 계산하여 토픽을  생성하는 사후 추론방법

각 문서는 주제가 무작위로 혼합되어  있으며 각 단어는 해당 주제 중 하나에서 나옴

포아송분포로부터 임의의 문헌 길이 N을 선택

𝜶를 매개변수로 한 디리클레분포로부터 주제분포 𝜽를 선택
𝜶: 문헌집단 내에서 정해지는 변수로,       학습을 통해 추정됨

하나의 문헌이 생성되는 과정
(i) 어떤 문헌에 대해 주제 벡터인 𝜽가 매개변수일 때, 앞에서부터 단어를 하나씩 채울 때마다 𝜽로부터 하나의 주제(𝒛_𝒏)를 선택
(ii) 다시 그 주제로부터 단어를 선택



𝜶: 문헌 내 주제분포 𝜽를 추정하기 위한 매개변수
문헌 내에서 특정 주제가 할당될 사전 확률(prior probability)

𝜷: 각 용어가 특정 주제에 할당될 사전 확률


이미 관찰된 변수(observed variable)를 통해 각각의 확률을 계산하여 토픽을  생성하는 사후 추론방법


## Gibbs Sampling
LDA에서 주제 분포와 주제-단어 분포를 산출하기 위한 또 하나의 기법
마르코프체인 몬테카를로(Markov chain Monte Carlo)의 한 종류
여러 분포 혹은 고차원의 분포로부터 특정값들을 무작위 추출을 통해 추정값을 산출하기 위한       일련의 방법론 (Gilks, Richardson & Spiegelhalter, 1996)


저차원 분포로부터 반복적인 샘플링을 통해 고차원 분포와 유사한 수치들의 집합을 찾아냄

----


# Latent Dirichlet Allocation

우리는 먼저 가장 단순한 주제 모델인 잠재 Dirichletallocation(LDA) 뒤에 숨겨진 기본 아이디어를 설명한다.8 LDA의 직관은 문서가 여러 주제를 나타낸다는 것이다. 예를 들어, 그림 1의 기사를 고려하십시오. '생명의 맨몸(유전자) 필수품 찾기'라는 제목의 이 글은 데이터 분석을 통해 유기체가 생존해야 할 유전자의 수(진화적 의미)를 파악하는 내용이다.

손으로 우리는 기사에 사용되는 다른 단어들을 강조하였다. "컴퓨터"와 "사전화"와 같은 데이터 분석에 대한 단어는 파란색으로 강조 표시되고, "생명체"와 "조직체"와 같은 진화 생물학에 대한 단어는 분홍색으로 강조 표시된다. "순서화"와 "순서화"와 같은 유전학에 관한 단어들은 노란색으로 강조되어 있다. 만약 우리가 기사의 모든 단어를 강조하기 위해 시간을 할애한다면, 당신은 이 기사가 유전학, 데이터 분석, 진화 생물학을 다른 비율로 혼합하는 것을 볼 수 있을 것이다. ("and" but" 또는 "if"와 같이 주제적인 내용이 거의 없는 단어는 제외한다.) 게다가, 이 논문이 그러한 주제들을 혼합한다는 것을 아는 것은 여러분이 그것을 과학 논문의 모음 속에 넣는 데 도움이 될 것이다.
LDA는 이러한 내용을 포착하려는 문서 모음의 통계 모델이다. 그것은 쉽게 그 생성 과정, 즉 모델이 문서가 생성되었다고 가정하는 가상의 무작위 과정으로 설명된다. (확률론적 모델로서의 LDA의 해석은 나중에 구체화된다.)


예시 기사에서 주제에 대한 논쟁은 유전학, 데이터 분석, 진화 생물학에 대한 조사 능력을 배치하고 각각의 단어는 그 세 개의 상위 icc 중 하나에서 도출된다. 이 컬렉션의 다음 기사는 데이터 분석과 신경 과학에 관한 것일 수 있다. 주제에 대한 논쟁은 이 두 가지 주제에 대한 조사 능력을 제공할 것이다. 이것은 잠재적 디리클레 할당의 구별되는 특징이다. 컬렉션의 모든 문서는 동일한 주제 집합을 공유하지만, 각 문서는 그러한 주제들을 서로 다른 비율로 보여준다.
주제 모델링을 위한 중앙 연산 문제는 관찰된 문서를 사용하여 숨겨진 주제 구조를 유추하는 것이다. 이것은 생성 과정을 "반복"하는 것으로 생각할 수 있다. 관찰된 수집을 생성했을 가능성이 있는 숨겨진 구조는 무엇인가?
---

LDA의 직관은 문서들이 multiple 다수의(여러) 토픽들을 나타낸다는 것입니다. 

예를 들어, 그림1의 아티클을 생각햅ㅂ시다. 이 아티클은 "Seeking Life's BAre (Genetic Neccessities)"이라는 제목의 이 아티클을 데이터 분석을 통해 유기체가 생존하는 데에 필요료 하는 유전자의 수를 결정하는 것에 대한 내용입니다.

"computer"와 "prediction"과 같이 데이터 분석에 관한 단어들은 파란색으로,  "life"와 "organism"과 같이 진화 생물학(evolutionary biology)에 대한 단어는 분홍색으로,  "sequenced"와 "genes"와 같이 유전학에 관한 단어들은 노란색으로 강조되어 있다.

 당신은 이 아티클에 유전학, 데이터 분석, 진화 생물학이 다른 비율들로 혼합되어 있는 것을 알 수 있습니다. 더 나아가, 
 
LDA는 이러한 내용을 포착하려는 문서 컬렉션에 대한 통계 모델입니다. LDA는 LDA의 generative process에 의해 쉽게 기술됩니다.

토픽 : 고정된 단어 집합에 대한 분포


우리는 어떠한 데이터가 생성되기 전에 이러한 토픽들이 구체화되어 있다고 가정합니다.
이제 컬렉션 내 각각의 문서에 대하여, 우리는 2단계 프로세스로 단어들을 생성합니다.
1. 토픽에 대한 분포를 랜덤하게 선택합니다.
2. 문서 내 각각의 단어에 대하여
    1. step1에서의 토픽에 대한 분포로부터 토픽을 랜덤하게 선택합니다.
    2. 상응하는 단어의 분포로부터 단어를 랜덤하게 선택합니다.

이러한 통계적 모델은 문서가 다수의 토픽들을 내포하고 있다는 직관을 반영합니다. 각각의 문서는 다른 비율로 그것의 토픽들을 내포하고 (스텝1), 각 문서의 각각의 단어는 토픽들 중 하나로부터 도출됩니다. (step2b), 선택된 토픽은 토픽에 대한 문서 당 분포로부터 선택됩니다.

예시의 아티클에서, 문서에 대한 분포는 유전학, 데이터 분석, 진화 생물학에 대한 확률로 위치하고, 각가의 단어는 이 세개의 토픽들로부터 도출됩니다. 컬렉션 내 다음 아티클이 데이터 분석과 신경 과학에 관한 것이라고 가정해 봅시다. 이 아티클의 토픽의 분포는 이 두개의 토픽들에 대한 분포로 위치할 것입니다. 이것이 LDADML 구별되는 특징입니다. 즉 컬렉션 내 모든 문서들은 동일한 토픽들의 집합을 공유하지만, 각각의 문서들은 다른 비율로 이러한 토픽들을 보입니다.

토픽 모델링의 목표는 문서들의 컬렉션으로부터 자동적으로 토픽들을 발견하는 것입니다.
문서들은 그자체로 관찰되는 바념ㄴ, 토픽 구조 - topics, per-document topic distribution, per-document per-word topic assignments-는 hidden structure입니다. 토픽모델링의 중요한 연산 문제는 숨겨진 토픽 구조를 추론하기 위해 관찰된 문서를 사용하는 것입니다. 이것은 generative process를 뒤집는 것 역행하는 것 으로 생각할 수 있습니다. 즉 관찰된 컬렉션에서 생성될 가능성이 있는 숨겨진 구조는 무엇인가?



주제 모델링을 위한 중앙 연산 문제는 관찰된 문서를 사용하여 숨겨진 주제 구조를 유추하는 것이다. 이것은 생성 과정을 "반복"하는 것으로 생각할 수 있다. 관찰된 수집을 생성했을 가능성이 있는 숨겨진 구조는 무엇인가?

# LDA and Probabilistic Models

LDA와 다른 토픽 모델들은 Probabilistic Modeling이라는 더 넓은 분야의 일부분입니다.
generative probabilistic modeling에서, 우리는 우리의 데이터를 hidden variable을 포함하는 generative porcess로부터 발생한 것으로 간주합니다. 이러한 generative process는 observed and hidden random varialrs 둘다에 대하여 joint probability distribution을 정의합니다. 우리는 관찰 변수가 주어졋을 때 히든 변수의 conditional distribution을 계산하기 위해 이 joint distribution을 사용함으로써 데이터 분석을 수행합니다. 이러한 conditional distribution은 posterior distribution이라고도 불립니다.

LDA는 정확하게 이 프레임 활용
observed variabls = the words of the documents = 문서 내 단어들
hidden variables = topic structure
generative process는 다음과 같습니다.
문서로부터 숨겨진 토픽 구조를 추론하는 것의 연산 문제는 posterior distribution - 문서가 주어질 때 히든 변수의 conditinal distribution-을 계산하는 것의 문제입니다.

우리는 LDA를 다음과 같은 노테이션

$$p( )$$

$ \beta_{1:K} $ : topics = 토픽들
$ \beta_k $ : a distribution over the vocabulary (the distributions over words at left in Figure 1). = 단어의 분포

$ \theta_d $ : the topic proportions for the dth document = d번째 문서에 대한 토픽의 비율
$\theta_{d,k}$ : the topic proportion for topic k in document d (the cartoon histogram in Figure 1). = 문서 d의 토픽 k에 대한 토픽 분포
$z_d$ : the topic assignments for the dth document = d번째 문서에 대한 토픽 어사인
$z_{d,n}$ : the topic assignment for the nth word in document d (the colored coin in Figure 1). 문서d의 n번째 단어에 대한 토픽 어사인

$w_d$ : the observed words for document d = 문서d의 관찰된 단어들
$w_{d,n}$ : the nth word in document d, which is an element from the fixed vocabulary. = 문서 d의 n번째 단어

LDA의 generative process는 다음의 observed and hidden vairabl의 joint distiribution에 상응합니다.

$
p \left( \beta_{1:K}, \theta_{1:D}, z_{1:D}, w_{1:D} \right) 
= 
$
