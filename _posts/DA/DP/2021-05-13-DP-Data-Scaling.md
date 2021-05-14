---
title: '[데이터 전처리] 데이터 스케일링 (Data Scaling)'

categories:
  - Data Analysis
tags:
  - Data Preprocessing

last_modified_at: 2021-01-20T08:06:00-05:00

classes: wide
---

이 글은 데이터 스케일링에 관한 기록입니다.

## 스케일링 개념


데이터를 모델링하기 전에는 반드시 스케일링 과정을 거쳐야 한다. 스케일링을 통해 다차원의 값들을 비교 분석하기 쉽게 만들어주며, 자료의 오버플로우(overflow)나 언더플로우(underflow)를 방지 하고, 독립 변수의 공분산 행렬의 조건수(condition number)를 감소시켜 최적화 과정에서의 안정성 및 수렴 속도를 향상 시킨다.

특히 k-means 등 거리 기반의 모델에서는 스케일링이 매우 중요하다.

회귀분석시 조건수라는 개념이 있는데 그 내용은 아래와 같다.

복잡한 얘기는 생략하고 핵심만 파악해보자.

함수의 조건수(condition number)는 argument에서의 작은 변화의 비율에 대해 함수가 얼마나 변화할 수 있는지 에 대한 argument measure이다.

조건수가 크면 약간의 오차만 있어도 해가 전혀 다른 값을 가진다. 따라서 조건수가 크면 회귀분석을 사용한 예측값도 오차가 커지게 된다.

회귀분석에서 조건수가 커지는 경우는 크게 두 가지가 있다.

1) 변수들의 단위 차이로 인해 숫자의 스케일이 크게 달라지는 경우. 이 경우에는 스케일링(scaling)으로 해결한다.

2) 다중 공선성 즉, 상관관계가 큰 독립 변수들이 있는 경우, 이 경우에는 변수 선택이나 PCA를 사용한 차원 축소 등으로 해결한다.

그리고 다음과 같은 경우에는 로그 함수 혹은 제곱근 함수 등을 사용하여 변환된 변수를 사용하면 회귀 성능이 향상될 수도 있다.

독립 변수나 종속 변수가 심하게 한쪽으로 치우친 분포를 보이는 경우 독립 변수와 종속 변수간의 관계가 곱셈 혹은 나눗셉으로 연결된 경우 종속 변수와 예측치가 비선형 관계를 보이는 경우
대부분 우리가 다루는 데이터 중 금액처럼 큰 수치 데이터에 로그를 취하게 되는 이유 이기도 하다.

보통 이런 데이터는 선별적으로 로그를 취한 후 모델링 전 전반적으로 스케일링을 적용한다.(in my opinion)

`scikit-learn`에서는 스케일링을 수행하는 다양한 스케일러를 제공합니다. 이때 모든 스케일러는 다음과 같은 메서드를 갖습니다.

- `SCALER.fit()` : 데이터 변환을 학습, train셋에 대해서만 적용
- `SCALER.transform()` : 실제로 데이터 변환을 수행, train셋과 test셋 모두에 대해서 적용

## 스케일링 종류

스케일링을 위해 `scikit-learn`에서는 `MinMaxScaler`, `MaxAbsScaler`, `StandardScaler`, `RobustScaler`을 제공합니다.

- `MinMaxScaler` : 데이터가 0과 1 사이에 위치하도록 스케일링
- `MaxAbsScaler` : 데이터가 -1과 1 사이에 위치하도록 스케일링
- `StandardScaler` : 데이터의 평균 = 0, 분산 = 1이 되도록 스케일링
- `RobustScaler` : 데이터의 중앙값 = 0, IQE = 1이 되도록 스케일링

데이터의 분포 별로 각각의 스케일링 결과가 어떻게 다른지 살펴보면 다음과 같습니다.

![]({{site.url}}/assets/images/DA/DP/scaling-ex1.PNG)
![]({{site.url}}/assets/images/DA/DP/scaling-ex2.PNG)
![]({{site.url}}/assets/images/DA/DP/scaling-ex3.PNG)
![]({{site.url}}/assets/images/DA/DP/scaling-ex4.PNG)
![]({{site.url}}/assets/images/DA/DP/scaling-ex5.PNG)

### MinMaxScaler

MinMaxScaler 최대/최소값이 각각 1, 0이 되도록 스케일링

- Transform features by scaling each feature to a given range.
- 주어진 range로 각각의 feature들을 스케일링 함으로써 feature들을 transform
- default는 0, 1입니다.

- 모든 feature들의 값이 0과 1 사이에 있도록 데이터를 변환합니다.
- 모든 feature들을 최소값=0, 최대값은 1이 되도록 스케일링 합니다.

- 데이터를 0-1사이의 값으로 변환
- (x - x의 최소값) / (x의 최대값 - x의 최소값)
- 데이터의 최소값과 최대값을 알 때 사용합니다.

- 다만 이상치가 있는 경우 변환된 값이 매우 좁은 범위로 압축될 수 있다. 즉, MinMaxScaler 역시 아웃라이어의 존재에 매우 민감하다.
- 이상치가 있는 경우 변환된 값이 매우 좁은 범위로 압축될 수 있습니다. MinMax 역시 이상치(Outlier)의 존재에 민감합니다.

MaxAbs Scaler라는 것도 있는데, 최대 절대값과 0이 각 1, 0이 되도록 하여 양수 데이터로만 구성되게 스케일링 하는 기법입니다.

```python
from sklearn.preprocessing import MinMaxScaler

# standard scaler 선언 및 학습
minmaxScaler = MinMaxScaler().fit(X_train)

# train feature를 standard scaling
X_train_minmax = minmaxScaler.transform(X_train) 

# standard scaling 변환
X_test_minmax = minmaxScaler.transform(X_test) 
```

### 3. MaxAbsScaler

- 
- Scale each feature by its maximum absolute value.
- 각 feature의 쵀대값의 절대값으로 스케일링


- 절대값이 0~1사이에 매핑되도록 한다. 즉 -1~1 사이로 재조정한다.
- 최대절대값과 0이 각각 1, 0이 되도록 스케일링


양수 데이터로만 구성된 특징 데이터셋에서는 MinMaxScaler와 유사하게 동작하며, 
- 큰 이상치에 민감할 수 있습니다.


```python
from sklearn.preprocessing import MaxAbsScaler

# MaxAbsScaler 선언 및 학습
maxabsScaler = MaxAbsScaler().fit(X_train)

# MaxAbsScaler를 사용하여 데이터 변환
X_train_maxabs = maxabsScaler.transform(X_train)

# MaxAbsScaler를 사용하여 데이터 변환
X_test_maxabs = maxabsScaler.transform(X_test)
```

## `StandardScaler`

StandardScaler 기본 스케일. 평균과 표준편차 사용
- 데이터가 표준 정규 분포(standard normal distribution)를 따르도록 스케일링
- 
- Standardize features by removing the mean and scaling to unit variance
- `StandardScaler()`는 평균을 제거하고 단위 분산(unit variance)으로 스케일링 함으로써 feature를 standardize합니다.

- 기존 변수에 범위를 정규 분포로 변환
- (x - x의 평균값) / (x의 표준편차)
- 데이터의 최소, 최대 값을 모를 경우 사용
 
- 각 피처의 평균을 0, 분산을 1로 변경합니다.
- 모든 특성들이 같은 스케일을 갖게 됩니다.

- 그러나 이상치가 있다면 평균과 표준편차에 영향을 미쳐 변환된 데이터의 확산은 매우 달라지게 된다. 따라서 이상치가 있는 경우 균형 잡힌 척도를 보장할 수 없다.
- 그러나 이상치가 있다면 평균과 표준편차에 영향을 미쳐 변환된 데이터의 확산은 매우 달라지게 됩니다. 따라서 이상치(Outlier)가 있는 경우 균형 잡힌 척도를 보장할 수 없게 됩니다.

```python
from sklearn.preprocessing import StandardScaler

# standard scaler 선언 및 학습
standardScaler = StandardScaler().fit(X_train)

# standard scaling 변환
X_train_standard = standardScaler.transform(X_train)

# standard scaling 변환
X_test_standard = standardScaler.transform(X_test) 

# 스케일링 된 결과 값으로 본래 값을 구할 수도 있다.
X_origin = std_scaler.inverse_transform(X_train_scaled)
```

### 4. RobustScaler
- 중앙값(median)과 IQR(interquartile range)을 활용하여 스케일링
- 각 특성들의 평균과 분산 대신 특성들의 중앙값을 0, IQE을 1로 스케일링한다
- 
아웃라이어의 영향을 최소화한 기법이다. 중앙값(median)과 IQR(interquartile range)을 사용하기 때문에 StandardScaler와 비교해보면 표준화 후 동일한 값을 더 넓게 분포 시키고 있음을 확인 할 수 있다.

IQR = Q3 - Q1 : 즉, 25퍼센타일과 75퍼센타일의 값들을 다룬다.

아웃라이어를 포함하는 데이터의 표준화 결과는 아래와 같다. 

- StandardScaler에 의한 표준화보다 동일한 값을 더 넓게 분포
- 이상치(outlier)를 포함하는 데이터를 표준화하는 경우
 
모든 특성들이 같은 크기를 갖는다는 점에서 Standard Scaler와 비슷하지만, 평균과 분산 대신 Median과 IQR(interquartile range)을 사용하며, 이상치(Outlier)의 영향을 최소화 합니다. StandardScaler와 비교해보면 표준화 후 동일한 값을 더 넓게 분포 시키고 있음을 확인 할 수 있습니다.
(IQR = Q3 - Q1 : 25% ~ 75% 타일의 값을 다룬다.)

```python
from sklearn.preprocessing import RobustScaler

# RobustScaler 선언 및 학습
robustScaler = RobustScaler().fit(X_train)

# RobustScaler를 사용하여 train셋 features 스케일링
X_train_robust = robustScaler.transform(X_train)

# RobustScaler를 사용하여 test셋 features 스케일링
X_test_robust = robustScaler.transform(X_test)
```

결론적으로 모든 스케일러 처리 전에는 아웃라이어 제거가 선행되어야 한다. 또한 데이터의 분포 특징에 따라 적절한 스케일러를 적용해주는 것이 좋다.

## 참고자료

https://scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing

https://mkjjo.github.io/python/2019/01/10/scaler.html

https://ebbnflow.tistory.com/137