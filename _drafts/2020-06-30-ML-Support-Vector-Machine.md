---
title:
permalink:

categories:
- Machine Learning

tags:

last_modified_at:
---

Hyperplane이란?

p차원 공간(X ∈ Rp×1)에서 Hyperplane은 전체
공간을 두 부분으로 나누는 평면(할!). 수학적으로 다음과 같이 정의한다.
y(X) = WT X + b = 0
SVM은 feature space를 이러한 hyperplane으로
두 공간으로 나누는 Classifier.
feature mapping을 어떻게 정의하냐에 따라 직선은 물론 곡선, 원 등 다양한 boundary가 가능하다!

Hyperplane의 특성
1 W: Hyperplane의 방향을 결정
􏰃 pf)y(XA)=y(XB)→WT(XA−XB)=0
2 b: 원점과 Hyperplane 간의 거리를 결정
􏰃 pf)projW(X)=WTW =− b ∥W∥ ∥W∥
3 임의의 벡터 A와 Hyperplane 간의 거리는 y(A) ∥W∥
 􏰃 pf)
A=A⊥+γ W ∥W∥
WTA+b=WTA⊥ +b+γWTW ∥W∥
y(A) = 0 + γ∥W ∥
γ = y(A) ∥W∥

Hyperplane과 Margin
yi ∈ {−1, 1} 의 예측에서, X의 Hyperplane을 이용한 다음의 classifier를 생각해보자.
hW,b(X) = g(WT X + b) where g(z) = ifelse(1,−1,z ≥ 0) Margin: γi를 임의의 관측치 Xi와 Hyperplane 사이의 거리는
WTXi +b WT b
γi := yi( ∥W∥ ) = yi(∥W∥Xi + ∥W∥)
Xi를 포함한 전체 데이터 셋과 Hyperplane 사이의 거리는 다음과 같이 정의하자. 즉 가장 가까운 놈을 기준으로 잰다는 것.
 WTb
γ := minyi( Xi + )
i ∥W∥ ∥W∥
이렇게 정의한 거리를 이용해 Optimal Margin Classifier를 구해보자.




Optimal Margin Classifier
전체 데이터 셋과 거리가 가장 먼 Hyperplane을 구하는 것은 다음과 같다. argmax 1 minyi(WTXi +b)
W,b ∥W∥ i 이때W와b의길이를쭉쭉늘려도γ:=miniyi(WT Xi+ b )의값은바뀌지않는다.때문에
∥W∥ ∥W∥
mini yi(WT Xi + b) = 1로 아예 고정해버리자. 그럼 위 식은 ∥W∥의 값을 최소화하는 것이므로,
아래와 같이 다시 쓸 수 있다.
􏰀 minW,b 21 ∥W ∥2
s.t. yi(WTXi +b)≥1 ∀i∈[N]
Quadratic Programming을 이용해 이걸 만족하는 W∗,b∗를 구할 수는 있지만 시간이 많이 걸린다.
좀 더 빠른 방법을 찾기 위해서는 Lagrange Duality에 대해 알아야 한다.
