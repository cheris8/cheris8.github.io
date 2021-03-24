---
title: '[ISL] Ch.6 Linear Model Selection and Regularization'

categories:
  - Machine Learning
tags:
  - Machine Learning

last_modified_at: 2020-08-01T08:06:00-05:00

classes: wide
use_math: true
---

# 6. Linear Model Selection and Regularization

## 6.0 Introduction

반응변수 $Y$와 예측변수 $X_1, X_2, ..., X_p$ 사이의 관계를 기술하는 데에 일반적으로 사용되는 linear model은 다음과 같습니다. 

$ Y = \beta_0 + \beta_1 X_1 + \cdots + \beta_p X_p + \epsilon $

6장에서는 이 linear model의 프레임워크를 확장하는 몇몇 접근 방법들에 대해 살펴보고자 합니다. 특히 linear model의 최소제곱법을 대안적인 fitting 절차들로 대체함으로써 linear model을 개선할 수 있는 몇가지 방법들에 대해 제시합니다. 이를 통해 우리는 더 나은 prediction accuracy와 model interpretability를 얻을 수 있습니다.

**Prediction Accuracy**
- 관측치의 개수가 변수의 개수보다 많은 경우 : least square estimates 낮은 bias & 낮은 variance  
- 관측치의 개수가 변수의 개수보다 많지 않은 경우 : 높은 variability -> overfitting -> poor predictions  
- 관측치의 개수가 변수의 개수보다 적은 경우 : variance $\rightarrow \infty$

따라서 우리는 추정된 계수를 constraing 혹은 shrinking 함으로써 variance를 줄여 accuracy를 향상시키고자 합니다.

**Model Interpretability**  
- 반응변수와 관련 없는 예측변수들이 모델에 포함 $\rightarrow$ 불필요하게 모델의 복잡성 증가
- 반응변수와 관련 없는 예측변수들을 제거 (coefficient estimates = 0 설정) $\rightarrow$ 모델의 interpretability 향상

따라서 우리는 자동적으로 feature selecton 혹은 variable selection, 즉 multiple regression model로부터 관련없는 변수들을 제외하는 몇몇 접근들에 대해 알아봅니다.

## 6.1 Subset Selection

### 6.1.1 Best Subset Selection

Best Subset Selection에서는 $p$개의 예측변수의 각각의 가능한 조합에 대해 별도의 least suqare regression을 fitting합니다. 즉 1개의 예측변수를 포함하는 $p$개의 model,  2개의 예측변수를 포함하는 $\frac{p(p-1)}{2}$ 개의 모델, ..., $p$개의 예측변수를 포함하는 1개의 모델 모두를 fitting합니다.

**Algorithm : Best subset selection**
1. Let $M_0$ denote the null model, which contains no predictors. This model simply predicts the sample mean for each observation.
2. For $k = 1, 2 ,... p$:  
    (a) Fit all ${p \choose k}$ models that contain exactly $k$ predictors.  
    (b) Pick the best among these ${p \choose k}$ models, and call it $M_k$. Here best 2 is defined as having the smallest RSS, or equivalently largest $R^2$. 
3. Select a single best model from among $M_0, ..., M_p$ using cross-validated prediction error, $C_p$, (AIC), BIC, or adjusted $R^2$.

### 6.1.2 Stepwise Selection

**Algorithm : Forward stepwise selection**

1. Let $M_0$ denote the null model, which contains no predictors.
2. For $k = 0, ..., p−1$:    
    (a) Consider all $p − k$ models that augment the predictors in $M_k$ with one additional predictor.  
    (b) Choose the best among these $p − k$ models, and call it $M_{k+1}$. Here best is defined as having smallest RSS or highest $R^2$.
3. Select a single best model from among $M_0, ..., M_p$ using cross-validated prediction error, $C_p$, (AIC), BIC, or adjusted $R^2$.

forward stepwise selection은 예측변수를 포함하지 않는 모델에서 시작하여 모든 예측변수가 모형에 포함될 때까지 한번에 하나씩 각 단계에서 fitting을 가장 크게 향상시키는 예측변수를 모델에 추가합니다.

**Algorithm : Backward stepwise selection**

1. Let $M_p$ denote the full model, which contains all $p$ predictors.
2. For $k = p, p−1, ..., 1$:  
    (a) Consider all $k$ models that contain all but one of the predictors in $M_k$, for a total of $k − 1$ predictors.  
    (b) Choose the best among these $k$ models, and call it $M_{k−1}$. Here best is defined as having smallest RSS or highest $R^2$.
3. Select a single best model from among $M_0, ..., M_p$ using cross-validated prediction error, $C_p$ (AIC), BIC, or adjusted $R^2$.

backward stepwise selection은 예측변수를 모두 포함하는 모델에서 시작하여 가장 유용하지 않은 예측변수를 한번에 하나씩 반복적으로 제거합니다.

### 6.1.3 Choosing the Optimal Model

앞서 살펴본 여러 Algorithm의 Step 3을 보면, 우리는 이러한 모델들 중 무엇이 최적의 모델인지 결정해야 함을 알 수 있습니다. 즉 우리는 test error를 추정해야 합니다. 이렇게 test error를 추정하는 방법은 크게 두가지로 나눠볼 수 있습니다.

- indirectly estimate test error : $C_p$, AIC, BIC, adjusted $R^2$
- directly estimate test error : validation set approach,  cross-validation approach (Ch.5)

**indirectly estimate test error**  
일반적으로 더 많은 변수들이 모델에 포함됨에 따라 training error는 감소할 것이지만 test error는 그렇지 않을 수 있습니다. 그러므로 training set RSS와 training set $R^2$은 변수의 개수가 다른 일련의 모델들 중에서 선택하는 데에 사용될 수 없습니다. 이에 대한 대안으로 모델의 사이즈 즉 모델에 포함된 변수의 개수에 대하여 training error를 조정하는 여러 방법들이 있습니다.
- $C_p$
- Akaike Information Criterion ; AIC
- Bayesian Information Criterion ; BIC
- adjusted $R^2$

**$C_p$**

$C_p = \frac{1}{n} \left( RSS + 2 d \hat{\sigma}^2 \right)$
- n : 관측치 개수
- d : 예측변수 개수
- $\hat{\sigma}^2$ : $\epsilon$의 분산의 추정치

$C_p$는 낮은 test error를 갖는 모델에 대해 작은 값을 갖는 경향이 있어 일반적으로 가장 낮은 $C_p$값을 갖는 모델을 선택합니다. $C_p$는 training RSS에 $2 d \hat{\sigma}^2$ 라는 패널티를 더해줍니다. 이 패널티는 training error가 test error를 과소평가 하는 경향이 있다는 사실을 조정하기 위한 것으로, 모델에 있는 예측변수의 개수가 증가함에 따라 증가합니다.

**AIC**

$AIC = \frac{1}{n\hat{\sigma}^2} \left( RSS + 2 d \hat{\sigma}^2 \right)$

**BIC**

$BIC = \frac{1}{n\hat{\sigma}^2} \left( RSS + \log\left(n\right) d \hat{\sigma}^2 \right)$

BIC는 $C_p$와 마찬가지로 낮은 test error를 갖는 모델에 대해 작은 값을 갖는 경향이 있어 일반적으로 가장 낮은 BIC값을 갖는 모델을 선택합니다. BIC는 $C_p$에서 사용하는 패널티인 $2 d \hat{\sigma}^2$ 대신 $\log \left( n \right) d \hat{\sigma}^2$를 패널티로 사용하는 것을 볼 수 있습니다. 이때 $n > 7$에 대하여 $\log n > 2$이므로 BIC는 일반적으로 변수가 많은 모델에 대해 $C_p$보다 더 무거운 패널티를 적용합니다. 따라서 $C_p$를 기준으로 사용할 때보다 BIC를 기준으로 사용할 때 예측변수의 개수가 더 적은 모델을 선택하게 됩니다.

**adjusted $R^2$**

$Adjusted \; R^2 = 1 - \frac{RSS / \left( n - d - 1 \right)}{TSS / \left( n - 1 \right)}$

adjusted $R^2$는 앞서 살펴본 criterion들과는 달리 낮은 test error를 갖는 모델에 대해 큰 값을 갖는 경향이 있어 일반적으로 가장 높은 adjusted $R^2$값을 갖는 모델을 선택합니다. adjusted $R^2$에서는, 일단 모든 정확한 변수들이 모델에 포함되었다면 추가적인 noise 변수들을 추가하는 것은 단지 RSS에서의 매우 작은 감소만으로 이어질 뿐이라고 여겨집니다. 그러므로, 이론적으로, 가장 큰 dsjuted $R^2$를 갖는 모델은 오직 정확한 변수들만을 갖고 noise variable은 갖지 않을 것입니다.

**비교**

![]({{site.url}}/assets/images/fig6-3.png)

## 6.2 Shrinkage Methods

### 6.2.1 Ridge Regression

Least Square는 다음의 식을 최소화하는 값으로 $\beta_0, \beta_1, ..., \beta_p$를 추정합니다.

$RSS = \sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2$

Ridge Regression은 Least Square와 매우 유사하지만, 살짝 다른 양을 최소화하는 값으로 Ridge 회귀 계수를 추정한다는 점에서 차이가 있습니다. Ridge 회귀 계수 추정치 $\hat{\beta}^R$는 다음의 식을 최소화하는 값입니다. 

$\sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2 + \lambda \sum_{j=1}^p \beta_j^2 = RSS + \lambda \sum_{j=1}^p \beta_j^2$

이는 다음과 같이 표현할 수도 있습니다.

$\min_p \left\\{ \sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2 \right\\} \; \mathrm{subject ; to} \; \sum_{j=1}^p \beta_j^2 \leq s$

Ridge 회귀에서의 패널티는 $\beta_j^2$로, Ridge 회귀는 $l_2$ 패널티를 사용한다고 할 수 있습니다.

**An Application to the Credit Data**

![]({{site.url}}/assets/images/fig6-4.png)

**Why Does Ridge Regression Improve Over Least Square?**

![]({{site.url}}/assets/images/fig6-5.png)

- black line = squared bias
- green line = variance
- purple line = test MSE

<U>Least Square에 비해 Ridge 회귀가 갖는 장점은 bias-variance trade-off에 기인합니다.</U> $\lambda$가 증가하면 Ridge 회귀의 유연성이 떨어져 variance은 감소하는 한편 bias은 증가합니다. 이는 예측변수의 개수인 $p$가 45이고, 관측치의 개수인 $n$이 50인 데이터셋을 사용하여 왼쪽 그림에 표현되어 있습니다. $\lambda$값이 0일 때, variance는 높은 한편 bias는 없습니다. 하지만 $\lambda$값이 증가함에 따라, Ridge 회귀 계수 추정치의 shrinkage는 bias를 약간 증가시키는 한편 variance를 상당히 감소시킵니다. $\lambda$값이 약 10일 때, bias는 거의 증가하지 않는 한편 variance는 급격히 감소하는 것을 볼 수 있습니다. 따라서 $\lambda$값이 0에서 10으로 증가함에 따라 MSE는 크게 감소합니다. $\lambda$값이 10보다 커지면, variance는 느리게 감소하는 한편 계수의 shrinkage로 인해 계수가 현저히 과소평가되기 때문에 bias는 크게 증가합니다. 최소 MSE는 $\lambda$값이 약 30일 때 나타나는 것을 확인할 수 있습니다.

일반적으로 반응변수와 예측변수의 관계가 선형에 가까운 상황에서 Least Square 추정치는 bias는 낮지만 variance가 높을 수 있습니다. 이는 trainig set에서의 작은 변화가 Least Square 계수 추정치에서의 큰 변화로 이어질 수 있다는 것을 의미합니다.

- $p = n$ : Least Square 추정치는 매우 높은 variance를 갖습니다.
- $p > n$ : Least Square는 솔루션조차 없는 반면 <U>Ridge 회귀는 bias의 작은 증가와 variance의 큰 감소를 교환함으로써 여전히 좋은 성능을 낼 수 있습니다.</U>

따라서 Ridge 회귀 분석은 Least Square 계수 추정치의 variance가 높은 상황에서 가장 효과적입니다.

### 6.2.2 The Lasso

Ridge 회귀는 한가지 분명한 단점을 가지고 있습니다. 바로 최종 모델에 $p$개의 모든 예측변수를 포함한다는 점입니다. 왜냐하면 Ridge 회귀에서의 패널티 $\lambda \sum \beta_j^2$는 모든 계수를 0을 향해 축소시킬 뿐 정확하게 0으로 설정하지는 않기 때문입니다. 이는 prediction accuracy 측면에서는 문제가 되지 않을 수 있지만, model interpretability 측면에서는 문제가 될 수 있습니다.

Lasso 회귀는 Ridge 회귀의 이러한 단점을 극복한 대안입니다. Lasso 회귀 계수 추정치 $\hat{\beta}_\lambda^L$는 다음의 식을 최소화하는 값입니다.

$\sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2 + \lambda \sum_{j=1}^p \left\vert \beta_j \right\vert = RSS + \lambda \sum_{j=1}^p \left\vert \beta_j \right\vert$

이는 다음과 같이 표현할 수도 있습니다.

$\min_p \left\\{ \sum_{i=1}^n \left( y_i - \beta_0 - \sum_{j=1}^p \beta_j x_{ij} \right)^2 \right\\} \; \mathrm{subject \; to} \; \sum_{j=1}^p \left\vert \beta_j \right\vert \leq s$

Lasso 회귀에서의 패널티는 $\left\vert \beta_j \right\vert$로, Lasso 회귀는 $l_1$ 패널티를 사용한다고 할 수 있습니다.

**The Variable Selection Property of the Lass**

![]({{site.url}}/assets/images/fig6-7.png)

<U>Lasso 회귀는 Ridge 회귀와 마찬가지로 계수 추정치를 0으로 축소합니다. 하지만 Lasso의 경우, $l_1$ 패널티를 통해 튜닝 파라미터 $\lambda$가 충분히 클 때 일부 계수 추정치를 정확하게 0으로 설정하도록 강제합니다. 따라서 Lasso는 variable selection을 수행한다고 볼 수 있습니다. 따라서 Lasso 회귀는 일반적으로 Ridge 회귀보다 해석하기에 훨씬 더 쉽습니다.</U>

### Ridge vs. Lasso 비교

![]({{site.url}}/assets/images/fig6-5.png)

![]({{site.url}}/assets/images/fig6-8.png)

![]({{site.url}}/assets/images/fig6-7.png)

![]({{site.url}}/assets/images/fig6-9.png)

![]({{site.url}}/assets/images/fig6-10.png)

## 6.3 Dimension Reduction Methods

목표: Y를 예측하기 위해 사용하는 predictor개수를 축소
1. Predictor transformation
2. 새로 변환한 변수를 사용하여 least sqares model 적용

기존 predictors:
- $X_1, X_2, ..., X_p$

Predictor transformation: **Linear combination (선형 결합)**
- $Z_1, Z_2, ..., Z_M$, where $M<p$
- $Z_m=\sum_{j=1}^{p}\phi_{jm}X_j$, for some $\phi_{1m},\phi_{2m},..., \phi_{pm}, m=1, ..., M$

회귀분석 모델 적용 (Least Squares):
- $y_i=\theta_0 + \sum_{m=1}^{M}\theta_mz_{im}+\epsilon_i$, $i=1, ..., n$

추정 계수:
- $\beta_0, \beta_1, ..., \beta_p$ (p+1개) $\Rightarrow$  $\theta_0, \theta_1, ..., \theta_M$ (M+1개)

<br>

Dimensions Reduction은 $\beta$에 대한 제약조건 하에 회귀분석하는 것 (기존 모델의 특수한 case)
- 기존 모델: 

    $Y=\beta_0+\beta_1X_1+...+\beta_pX_p+\epsilon$
        
        
- 위 차원축소된 회귀분석 모델을 $\beta$에 대해 정리하면: 
    
     $\sum_{m=1}^{M}\theta_mz_{im} = \sum_{m=1}^{M}\theta_m\sum_{j=1}^{p}\phi_{jm}x_{ij}=\sum_{j=1}^{p}\sum_{m=1}^{M}\theta_m\phi_{jm}x_{ij}=\sum_{j=1}^{p}\beta_{j}x_{ij}$, <p> where $\beta_{j}=\sum_{m=1}^{M}\theta_m\phi_{jm}$

Bias-Variance Tradeoff:
- 제약조건으로 인해 $\beta$ 계수 추정치에 bias가 생길 수 있다
- p가 n보다 큰 경우 dimension reduction($M<p$ 개의 predictor 선택)은 fitted된 계수의 variance를 낮출 수 있다 (M=p이며 $Z_m$이 선형 독립이면 기존 모델과 동일해짐)
                                      
$Z_m$, 즉 $\phi_{jm}$을 잘 선택하는 방법 2가지: 
1. principal components 
2. partial least squares

### 6.3.1 Principal Components Regression

Principal Components Analysis(PCA):
- **차원축소**: 수많은 X변수 중 low-dimensional feature 선택
- Unsupervised Learning (10장)

**An Overview of Principal Component Analysis**

**목표**: $X_{nxp}$의 차원축소

**용어:**
- **first principal component**: 1) observation의 분산을 가장 크게 만드는 data의 방향, 또는 2) data와의 직각 거리가 가장 작은 선 (sum of the squared perpendicular distances)
    - Figure 6.14의 녹색선이 first principal component
    - 100개의 observation을 가능한 모든 basis vector에 projection했을때 projected observation의 variance를 가장 크게 만들어주는 basis vector가 녹색선이다. 
    - 초록선 basis vector로 보고 
    - 수학적으로는<br>
        - $Max_{\phi_{11},\phi_{21}}Var[\phi_{11}*(pop-\bar{pop})+\phi_{21}*(ad-\bar{ad})]$ s.t $\phi_{11}^2+\phi_{21}^2=1$<br>
        - solving gives...
        - $Z_1=0.839*(pop-\bar{pop})+0.544*(ad-\bar{ad})$
        
- **principal component loadings**: 
    - $\phi_{11}=0.839$ <p> $\phi_{21}=0.544$

- **principal component scores**: $Z_1$의 원소 $z_{i1}, ..., z_{in}$
    - $Z_1$은 nx1 벡터이다
    - $z_{i1}=0.839*(pop-\bar{pop})+0.544*(ad_i-\bar{ad})$
    - Figure 6.15의 우측
    - x축 기준 (first principal component basis를 x축으로 위치) 0으로부터의 거리
    
**First principal component $Z_1$의 의미:**
- $z_{i1}<0$ 인 경우 observation(도시 i)의 population과 ad는 평균보다 작다 
- Why? First principal component는 pop와 ad predictor의 정보를 잘 capture한다. 
    1. population과 ad는 양의 상관관계를 가진다. 즉 population이 낮으면 ad도 낮을것 (Fig 6.14)
    2. first principal component와 population, ad 간에 뚜렷한 상관관계가 있다 (Fig. 6.16)
    
**용어:**
- **kth principal component (k>1)**: 두 가지 조건<br>
    - 1) k-1번째 principal component와 othogonal(uncorrelated), <br>2) projected observation의 variance를 가장 크게 만들어주는 vector
    - Figure 6.14의 점선
    - 최대 개수인 p개의 principal component는 모든 p개 predictor의 정보를 다 포함한다
    - 하지만 first principal component가 가장 많은 정보를 담고 있다 (variability가 클수록 더 많은 정보 포함) (Figure 6.15, 6.17)

    **The Principal Components Regression Approach**

**용어:**
- **principal components regression(PCR)**:
    - M개의 principal components $Z_{1}, ..., Z_{M}$을 predictor로 사용한 least squares linear regression 
    - Why good? $X_{1}, ... X_{p}$의 variation이 가장 크게 나타나는 방향 $Z_j$들이 대체로 $Y$와 상관관계가 가장 큰 components들이다. 
    - PCR 가정 하에 M개의 $Z_{1}, ..., Z_{M}$가 p개의 X predictors에 대한 정보를 거의 다 포함하면 overfitting을 완화해 X predictors를 모두 사용한 least squares model보다 성능이 더 좋을 수 있다. 
    - p개의 X predictors에 대한 variation/정보를 충분히(sufficiently) 설명하는 components의 개수 M이 작을수록 PCR의 성능은 좋아진다(Figure 6.18, 6.19) 
    - M=p인 경우 기본 모형과 같아진다 (least squares fit using all p predictors)
    - **주의** 각 $Z_{k}$는 모든 p개의 X predictors의 선형결합이기 때문에 X predictors를 모두 사용한다. 따라서 PCR은 변수를 제거하는 feature selection method와 다르다
    - ridge regression은 PCR의 continuous version이다 (lasso regression과 다르게 모든 변수 사용, ridge regression의 $\lambda$는 continuous, PCR의 M은 discrete)
        - $\lambda$가 증가할수록 MSE가 증가하고, M이 감소할수록 MSE증가한다. (Figure 6.20)

**최적의 PCR:**
- principal component를 구하기 전에 각 predictor에 대한 정규화도 필요하다. 이는 변수의 scale 단위가 클수록 variance가 클 것이며 이는 principal component 선택에 영향을 미칠 것이다. 
- cross-validation error가 최소가 되는 component 개수 M을 선택한다. (Figure 6.20)

**PCR의 단점:Unsupervised dimension reduction**
- principal component $Z_{k}$는 response Y와 관계없이 X만 잘 설명하도록 도출
- response Y가 principal component의 선택을 supervise하지 않는다

### 6.3.2 Partial Least Squares

**용어:**
- **Partial Least Squares (PLS): supervised dimension reduction**
    - dimensions reduction: 기존 feature의 linear combinzation M개의 $Z_k$ 도출하고 이 M개의 feature들에 least squares linear regression 수행
    - Supervised: X뿐만 아니라 Y의 정보도 잘 포착하는 component/direction/$Z_k$ 선택
- **first PLS direction (도출방법)**
    1. p개의 predictor X 정규화
    2. 각 $X_{j}$를 Y에 대해 simple linear regression 수행하여 j개의 coefficient $\rho_j$
    3. first PLS= $Z_{1}$ = $\sum_{j=1}^{p}\phi_{j1}X_j$, where $\phi_{j1}=\rho_j$
        - PLS는 response와 가장 상관관계가 높은 변수에 가장 높은 weight를 부여 (예시 Fig.6.21)
- **kth PLS direction (k>1) (도출방법)**:<br>
    1. 각 $X_{j}$를 $Z_{k-1}$에 대해 simple linear regression 수행하여 j개의 residuals $\epsilon_j$ 도출
    2. kth PLS= $Z_{k}$ = $\sum_{j=1}^{p}\phi_{jk}X_j$, where $\phi_{jk}=\epsilon_j$
    
**최적의 PLS:**
- 각 predictor에 대한 정규화도 필요하다. 
- cross-validation error가 최소가 되는 component 개수 M을 선택한다. 

**PLS의 단점:Unsupervised dimension reduction**
- ridge regression과 PCR에 비해 성능이 떨어진다 
- bias를 줄일 수 있지만 variance를 상승시킬 수 있다. 

## 6.4 Considerations in High Dimensions

### 6.4.1 High-Dimensional Data

- 최근 20년간 feature data의 개수가 폭발적으로 증가
- high-dimensional: $n<p$
    - feature와 response 간 상관관계 존재 여부와 무관하게 least squares 수행시 잔차가 0이 되어 perfect fit을 달성하는 계수를 구할 수 있음
    - 하지만 trainsing MSE가 0으로 수렴하더라도 test MSE는 증가 
    - high-dimensional인 경우 $\hat{\sigma}^2$=0이기 때문에 $C_p$, AIC, BIC 방법론 사용 불가하며, fitting을 less flexible하게 만드는 (overfitting 방지) forward stepwise selection, ridge regression, lasso, PCR을 사용한다. 
    - multicollinearity problem