---
title: '[머신러닝] 하이퍼파라미터 튜닝 (Hyperparameter Tuning)'

categories:
  - Machine Learning
tags:
  - 

last_modified_at: 2020-10-06T08:06:00-05:00

classes: wide
use_math: true
---

## Hyperparameter Tuning 종류

1. Manual Search
2. Grid Search
3. Random Search
4. Bayesian Optimization
5. Early Stopping Based
6. Gradient Based Optimization 7. Evolutionary Optimization

## 1. Manual Search

- 개념
    - 경험 또는 감에 의존해서 hyperparameter를 설정하는 것
- 단점
    - 각 hyperparameter 조합에 대한 제대로 된 비교가 어려움
- 활용
    - 성능을 수치로 확인하기 어려운 경우 혹은 성능 비교가 어려운 경우에 사용
    - 예) DL에서 Layer 수 조정 / Word2vec에서 dimension, window 수 조정

## 2. Grid Search

- 개념
    - 각 Hyperparameter 자리에 들어갈 값들을 미리 정해 놓고 모든 조합을 실행하여 최적의 조합을 찾는 것
- 간단하고 모든 가능성을 탐색할 수 있으나 시간이 오래 걸림
- 데이터가 크거나 연산량이 많은 모델(boosting 등)에는 부적합

## 3. Random Search

각 Hyperparameter의 후보 값을 정해놓고 후보 내에서 무작위 값 을 반복추출하여 최적의 조합을 찾는 것
• 간단하며, Grid search에 비해 시간대비 성능이 훨씬 좋음
• 매번 성능이 달라지며 최적의 성능을 찾기는 사실상 어려움

## 4. Bayesian Optimization

Bayesian Optimization (2010) (Sequential Model-Based Optimization)
각 Hyperparameter 값에 대한 성능을 학습하여, 성능을 최대화 할 수 있는 Hyperparameter 조합을 찾아내는 알고리즘
• 앞의 방법들보다 빠르고 정확하게 최적 hyperparameter를 찾을 수 있음
• 연속적인 탐색이므로 병렬처리가 불가

목적함수를 추정하는 모델 (Surrogate Model)
+
다음 입력 데이터로 어떤 것이 좋을지 추천하는 함수 (Acquisition Function)
Surrogate Model의 종류에 따라 종류를 구분하기도 함

## 5. Early Stopping Based Model

제한된 시간, 자원 내에 최대한의 성능을 내는 hyperparameter를 찾기 위한 방법
• SHA(2015), Hyperband(2016) 등
• hyperparameter가 많고, 범위가 매우 넓은 경우에 적절
• 시간이 매우 적거나, computing power가 매우 한정적인 경우에 적절

SHA(2015)
o 총 탐색에 소요되는 budget (B) 설정
o n 개의 hyperparameter 설정 랜덤 추출
o n 개의 모델에 budget 할당하여 학습
o 중간loss추출
o 중간loss보다성능이안좋은설정을반만큼
버림
o 나머지 설정값들로 위 과정을 반복하여, 하나의
설정값이 남도록 진행
• Random Search보다 훨씬 빠르고 좋은 성능
• hyperparameter 튜닝에 또 다른
hyperparameter (B, n) 이 존재하고 이에 따 른성능격차큼


Hyperband (2016)
o 하나의 탐색에 최대로 할당할 budget (R) 설정
o 이후중간loss보다성능이안좋은설정값을특정
비율만큼버림
• 다른 기법들과 비교했을 때 최적 성능에 도달하는 시간이 매우 빠르며, 성능도 비슷하게 좋고 variation도 적음
• 2018년 Bayesian Optimization의 성능과 Hyperband의 속도를 결합한 BOHB 모델이 나옴
 
## 6. Gradient Based Model


Hyperparameter에 따른 성능 변화를 함수화 한 후, 기울기 값을 추정하여 최적 hyperparameter를 탐색 하는 방법
• 기울기 값을 추정하는 방식에 따라 다양한 모델이 있으며, 아직 활발히 연구중
• HOAG(2016), FAR-HO(2018), RFHO(2018)
• HOAG 모델을 사용하였을 때, 시간을 충분히 들이
면 Bayesian Optimization보다도 좋은 성능을 내 는것으로나옴



