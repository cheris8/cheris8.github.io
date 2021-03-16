---
title: '[ISL] Ch.5 Resampling Method'

categories:
  - Machine Learning
tags:
  - Machine Learning
  - ISL

last_modified_at: 2020-07-31T08:06:00-05:00

classes: wide
use_math: true
---

# 5. Resampling Method

## 5.0 Overview

- Resampling methods : training set으로부터 반복적으로 sample들을 추출하고 각 sample에 대하여 모델을 refitting하는 일련의 작업  
  - Cross-Validation (교차 검증) : 모델의 성능을 평가하기 위해 (model assessment) 혹은 모델의 유연성을 적절한 수준으로 선택하기 위해 (model selection) **주어진 모델과 관련하여 test error를 추정**하는 데에 사용   
  - Bootstrap (부트스트랩) : **매개변수 추정치의 정확도 혹은 주어진 모델의 정확도에 대한 척도를 제공**하는 데에 사용

## 5.1 Cross-Validation

test error는 지정된 test set이 있는 경우 쉽게 계산할 수 있지만, 불행히도 우리는 일반적으로 test set을 사용할 수 없는 경우가 많습니다. 반면, training error는 training set에 모델을 fitting함으로써 쉽게 계산할 수 있습니다. 하지만 training error는 test error와 상당히 다른 경우가 많아 문제의 소지가 있습니다.

지정된 test set이 없는 경우 training set을 사용하여 test error를 추정하는 많은 기법들이 존재합니다. 한가지 방법으로 *training error를 수학적으로 조정하여 test error를 추정하는 방법*이 있습니다. (Ch.6 : $C_p$, AIC, BIC, adjusted $R^2$) 또다른 방법으로는 *training set의 하위 집합을 선택한 다음 이 하위 집합에 모델을 fitting하여 test error를 추정하는 방법*이 있습니다. (Ch.5 : The Validation Set Approach, LOOCV, k-Fold CV)

### 5.1.1 The Validation Set Approach

**개념**

![]({{site.url}}/assets/images/fig5-1.png)

Validation Set Approach에서는 데이터셋을 무작위로 training set와 validation set 두 부분으로 나눕니다. 모델은 training set에 fitting하고, fitting한 모델은 validation set의 반응변수를 예측하는 데에 사용됩니다. 그 결과로 나오는 validation set error를 test error의 추정치로 사용합니다.

**문제점**

![]({{site.url}}/assets/images/fig5-2.png)

1. validation set error는 training set과 validation set에 어떤 관측치가 포함되는지에 따라 매우 variable 할 수 있습니다. (오른쪽 그림)
2. 일반적으로 모델을 fitting할 때 사용하는 데이터셋의 크기가 작을수록 모델의 성능은 낮아집니다. Validation Set Approach에서는 전체 데이터셋 중 일부만을 모델을 fitting하는 데에 사용하기 때문에, 이는 validation set error가 전체 데이터셋에 fitting한 모델에 대한 test error를 과대평가하는 경향이 있음을 시사합니다.

### 5.1.2 Leave-One-Out Cross-Validation (LOOCV)

**개념**

![]({{site.url}}/assets/images/fig5-3.png)

LOOCV는 Validation Set Approach의 단점을 극복하기 위해 만들어진 방법입니다. 데이터셋을 비슷한 크기의 두 부분으로 나누는 Validation Set Approach와 달리, LOOCV에서는 단 하나의 관측치 $\left( x_1, y_1 \right)$ 만을 validation set으로 사용하고, 나머지 관측치들 $\{ \left( x_2, y_2 \right), ..., \left( x_n, y_n \right) \}$을 training set으로 사용합니다. 즉 LOOCV에서는 $n$개의 관측치를 가지고 있는 전체 데이터셋을 $n - 1$개의 관측치를 가지고 있는 training set과 1개의 관측치를 가지고 있는 validation set 두 부분으로 나눕니다. 모델은 $n - 1$개의 관측치를 가지고 있는 training set에 fitting하고, training set에 포함되지 않은 관측치인 $x_1$을 사용하여 $\hat{y}_1$을 만듭니다. $MSE_1 = (y_1-\hat{y}_1)$은 $\left( x_1, y_1 \right)$이 fitting에 사용되지 않았기 때문에 test error에 대해 bias가 없는 추정치를 제공합니다. 하지만 비록 $MSE_1$이 test error에 대해 bias가 없다 하더라도, $MSE_1$은 단 하나의 관측치 $\left( x_1, y_1 \right)$에 기반한 값이기 때문에 variability가 높아 좋은 추정치라고 볼 수는 없습니다. 따라서 우리는 validation set을 $\left( x_1, y_1 \right)$부터 $\left( x_n, y_n \right)$까지 반복적으로 선택하여 위 과정을 $n$번 반복합니다. 그 결과 $MSE_1, ..., MSE_n$을 얻을 수 있고, LOOCV에서는 이러한 $n$개의 test error의 추정치에 대한 평균을 test error의 추정치로 사용합니다. 

$CV_{\left( n \right)} = \frac{1}{n} \sum_{i=1}^n MSE_i$

**장점**

1. LOOCV는 Validation Set Approach에 비해 훨씬 더 낮은 bias를 갖습니다.
2. training set과 validation set을 나누는 과정에서 랜덤성이 있어 매번 다른 결과를 산출해내는 Validation Set Approach와는 달리, LOOCV는 training set과 validation set을 나누는 과정에서 랜덤성이 없어 항상 같은 결과를 산출해냅니다.

### 5.1.3 k-Fold Cross-Validation (k-Fold CV)

**개념**

![]({{site.url}}/assets/images/fig5-5.png)

k-Fold CV는 LOOCV에 대한 대안으로 등장했습니다. k-Fold CV에서는 전체 데이터셋을 거의 같은 크기의 $k$개의 fold로 무작위로 나눕니다. 먼저 첫번째 fold를 validation set으로 나머지 $k-1$개의 fold를 training set으로 사용합니다. 즉 모델은 training set에 fitting하고, 첫번째 fold인 validation set을 사용하여 $MSE_1$을 계산합니다. 다음으로 두번째 fold를 validation set으로 나머지 $k-1$개의 fold를 training set으로 사용합니다. 즉 모델은 training set에 fitting하고, 두번째 fold인 validation set을 사용하여 $MSE_2$을 계산합니다. 이러한 과정을 $k$번 반복합니다. 그 결과 $MSE_1, ..., MSE_k$를 얻을 수 있고, k-Fold CV에서는 이러한 $k$개의 test error의 추정치에 대한 평균을 test error의 추정치로 사용합니다.

$CV_k = \frac{1}{k} \sum_{i=1}^k MSE_i$

**LOOCV vs. k-Fold CV**

LOOCV는 k-Fold CV의 하나의 특별한 케이스에 해당된다는 것을 볼 수 있습니다. 즉 LOOCV는 $k = n$일 때의 k-Fold CV와 같습니다.

![]({{site.url}}/assets/images/fig5-6.png)

- blue line : true test MSE
- black line : LOOCV test MSE 추정치
- orange line : 10-fold CV test MSE 추정치

**장점**

1. k-Fold CV는 LOOCV에 비해 computational합니다.
2. k-Fold CV는 LOOCV에 비해 더 정확한 test error의 추정치를 제공합니다.

## 5.2 The Bootstrap

Bootstrap은 주어진 추정치 또는 모델과 관련된 불확실성을 계량화하는 데에 사용됩니다. 예를 들어, Bootstrap을 선형 회귀 계수의 standard error를 추정하는 데에 사용할 수 있습니다.

우리가 고정된 자금을 두가지 금융자산에 투자하려고 한다고 가정해봅시다. 우리는 돈의 $\alpha$만큼을  $X$에, 나머지 $1 - \alpha$만큼을 $Y$에 투자할 것입니다. 우리는 투자의 리스크, 즉 variance를 최소화하는 $\alpha$를 선택하고자 할 것입니다. 다시 말해 우리는 $Var(\alpha X + \left( 1 - \alpha \right) Y)$를 최소화하기를 원합니다. 이를 최소화하는 $\alpha$는 다음과 같습니다.

$\alpha = \frac{\sigma_Y^2 - \sigma_{XY}}{\sigma_X^2 + \sigma_Y^2 - 2 \sigma_{XY}}$

- $\sigma_X^2$ : $Var(X)$  
- $\sigma_Y^2$ : $Var(Y)$  
- $\sigma_{XY}$ : $Cov(X,Y)$  

하지만 현실에서 $\sigma_X^2$, $\sigma_Y^2$, $\sigma_{XY}$는 알려지지 않은 값이므로 우리는 데이터셋을 사용하여 이들의 추정치인 $\hat{\sigma_X^2}$, $\hat{\sigma_Y^2}$, $\hat{\sigma}_{XY}$를 계산합니다. 그러면 우리는 다음과 같이 우리의 투자의 variance를 최소화하는 $\alpha$값을 추정할 수 있을 것입니다.

$\hat{\alpha} = \frac{\hat{\sigma}\_Y^2 - \hat{\sigma}\_{XY}}{\hat{\sigma}\_X^2 + \hat{\sigma}\_Y^2 - 2 \hat{\sigma}\_{XY}}$

_(참고용 시작)_

![]({{site.url}}/assets/images/fig5-9.png)

위 그림에서 각각의 그래프는 simulated 데이터셋, 즉 시뮬레이션을 통해 생성된 100쌍의 $X$, $Y$ 관측치를 나타냅니다. 먼저 이 100쌍의 $X$, $Y$ 관측치를 사용하여 $\sigma_X^2$, $\sigma_Y^2$, $\sigma_{XY}$를 추정하고 ($\hat{\sigma_X^2}$, $\hat{\sigma_Y^2}$, $\hat{\sigma}_{XY}$를 계산하고), 위의 식을 통해 $\alpha$를 추정합니다 ($\hat{\alpha}$를 계산합니다). 각각의 simulated 데이터셋에서 결과로 나온 $\hat{\alpha}$값은 0.532 ~ 0.657 범위에 존재합니다.

이제 $\alpha$의 추정치 즉 $\hat{\alpha}$의 정확도에 대해 살펴보고자 합니다. $\hat{\alpha}$의 standard deviation를 추정하기 위해, 시뮬레이션을 통해 100쌍의 $X$, $Y$ 관측치를 생성하는 위 과정을 1000번 반복합니다. 그 결과 1000개의 $\alpha$에 대한 추정치, 즉 $\hat{\alpha}\_{1}, \hat{\alpha}\_{2}, ..., \hat{\alpha}\_{1000}$을 얻을 수 있습니다. 

![]({{site.url}}/assets/images/fig5-10.png)

- 왼쪽 그림 : A histogram of the estimates of α obtained by generating 1,000 simulated data sets from the true population
- 가운데 그림 : A histogram of the estimates of α obtained from 1,000 bootstrap samples from a single data set
- 오른쪽 그림 : The estimates of α displayed in the left and center panels are shown as boxplots

왼쪽 그림은 결과로 나온 $\alpha$에 대한 추정치 1000개를 히스토그램으로 표현한 것입니다. 이 시뮬레이션에서 매개변수는 $\sigma_{X}^2 = 1$, $\sigma_{Y}^2 = 1.25$, $\sigma_{XY} = 0.5$로 설정되었는데, 이를 통해 $\alpha$의 실제값은 0.6임을 알 수 있습니다. 이 값은 위 그림에서 분홍색 선으로 표시되어 있습니다.

$\alpha$에 대한 추정치 1000개의 평균은 다음과 같습니다.

$\bar{\alpha} = \frac{1}{1000} \sum_{r=1}^{1000} \hat{\alpha}_r = 0.5996$

이때 $\bar{\alpha}$의 값이 $\alpha$의 실제값인 0.6에 매우 가깝다는 것을 확인할 수 있습니다. 

추정치의 standard deviation은 다음과 같습니다.

$\sqrt{\frac{1}{1000-1} \sum_{r=1}^{1000} \left( \hat{\alpha}_r - \bar{\alpha} \right) ^ 2}$

$SE \left( \hat{\alpha} \right) \approx 0.083$로, 이를 통해 $\hat{\alpha}$의 정확도를 알 수 있습니다. 대략적으로, 모집단으로부터 무작위로 추출한 표본에 대하여, 평균적으로 $\hat{\alpha}$은 $\alpha$와 약 0.08만큼 차이가 날 것이라고 생각해볼 수 있습니다.

_(참고용 끝)_

**개념**

그러나 실제 데이터의 경우 모집단으로부터 새로운 표본을 생성할 수 없기 때문에 앞서 설명한 $SE \left( \hat{\alpha} \right)$에 대해 추정하는 과정은 현실에서는 적용될 수 없습니다. <U>이때 Bootstrap은 추가적으로 관측치를 생성하지 않아도 $\hat{\alpha}$의 variability를 추정할 수 있도록 합니다.</U> 다시 말해, Bootstrap은 모집단으로부터 반복적으로 독립적인 데이터셋을 얻는 대신 <U>오리지널 데이터셋으로부터 반복적으로 관측치를 추출하여 데이터셋을 얻습니다.</U>

![]({{site.url}}/assets/images/fig5-11.png)

오리지널 데이터셋 $Z$는 3개의 관측치를 가진 데이터셋입니다. 먼저 오리지널 데이터셋 $Z$로부터 무작위로 $n$개의 관측치를 선택하여 (복원추출) 부트스트랩 데이터셋 $Z^{\*1}$을 생성합니다. $Z^{\*1}$은 세번째 관측치를 2번, 첫번째 관측치를 1번, 두번째 관측치를 0번 포함합니다. $Z^{\*1}$을 사용하여 $\alpha$에 대한 새로운 부트스트랩 추정치, 즉 $\hat{\alpha}^{\*1}$를 생성합니다. 이 과정을 $B번$ 반복하여 $B$개의 서로 다른 부트스트랩 데이터셋 $Z^{\*1}, Z^{\*2}, ..., Z^{\*B}$를 생성하고, 이를 사용하여 $B$개의 부트스트랩 추정치 $\hat{\alpha}^{\*1}, \hat{\alpha}^{\*2}, ..., \hat{\alpha}^{\*B}$를 생성합니다. 이러한 부트스트랩 추정치에 대한 standard error는 다음과 같습니다.

$
SE_B \left( \hat{\alpha} \right) = \sqrt{\frac{1}{B-1} \sum_{r=1}^B \left(\hat{\alpha}^{\*r} - \frac{1}{B} \sum_{r=1}^B \hat{\alpha}^{\*r'} \right)^2}
$

이 $SE_B \left( \hat{\alpha} \right)$를 오리지널 데이터셋으로부터 추정된 $\hat{\alpha}$의 standard error에 대한 추정치로 사용합니다.
