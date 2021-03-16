---
title: '[ISL] Ch.8 Tree Based Methods'

categories:
  - Machine Learning
tags:
  - Machine Learning
  - ISL
  - Classification
  - Regression

last_modified_at: 2020-07-31T08:06:00-05:00

classes: wide
use_math: true
---

# 8. Tree-Based Methods

## 8.1 The Basics of Decision Trees

의사결정트리는 회귀와 분류 둘다에 적용될 수 있습니다. 회귀에 적용되는 것을 **Regression Trees**, 분류에 적용되는 것을 **Classification Trees**라고 합니다.

### 8.1.1 Regression Trees

**Predicting Baseball Players' Salaries Using Regression Trees**

![]({{site.url}}/assets/images/fig8-1.png)

![]({{site.url}}/assets/images/fig8-2.png)

**Prediction via Stratification of the Feature Space**

1. We divide the predictor space into $J$ distinct and non-overlapping regions, $R_1, R2, ..., R_J$.
2. For every observation that falls into the region $R_j$ , we make the same prediction, which is simply the mean of the response values for the training observations in $R_j$.

먼저 predictor space를 구별되는 그리고 겹치지 않는 $J$개의 영역 $R_1, R2, ..., R_J$로 나눕니다. 영역 $R_j$에 속하게 되는 모든 observation에 대하여, $R_j$에 속한 training observation에 대한 response의 평균으로 값을 예측합니다.

예를 들어, 2개의 영역 $R_1$과 $R_2$를 얻었다고 가정해봅시다. 이때 첫번째 영역 즉 $R_1$에 속한 training observation에 대한 response의 평균은 10, 두번째 영역 즉 $R_2$ 에 속한 training observation에 대한 response의 평균은 20입니다. 그러고 나서 주어진 관측치 $X = x$에 대하여, 만약 $x \in R_1$이라면 10으로 값을 예측할 것이고 $x \in R_2$라면 20으로 값을 예측할 것입니다.

그렇다면 이제 <U>Region $R_1, ..., R_J$를 구성하는 방법</U>에 대해 살펴보겠습니다. 앞서 살펴본 바와 같이, Region $R_1, ..., R_J$를 구성한다는 것은 predictor space를 나눈다는 것을 의미합니다. 구체적으로는 predictor space를 high-dimensional boxes (rectangles)로 나눕니다. 목표는 RSS를 최소화 하는 박스 $R_1, ..., R_J$를 찾는 것입니다.

$\sum_{j=1}^{J} \sum_{i \in R_j} \left( y_i - \hat{y}_{R_j} \right)^2$

- $\hat{y}_{R_j}$ : j번째 박스에 있는 training observation에 대한 response의 평균

이를 찾기 위해 **recursive binary splitting**을 사용합니다. 먼저 recursive binary splitting은 <U>top-down</U> 즉 하향식 접근법입니다. 왜냐하면 모든 관측치가 하나의 영역에 속하는 지점인 트리의 맨 상단에서 시작하여 predictor space를 연속적으로 분할하기 때문입니다. 각 분할은 트리의 더 아래쪽에 있는 두개의 새로운 가지를 통해 표시됩니다. 또한 recursive binary splitting은 <U>greedy</U>합니다. 이는 트리를 성장시킬 때 미래에 더 나은 트리로 이어질 분할을 미리 보고 고르는 것이 아니라 해당 단계에서의 최적의 분할을 이루어냄을 의미합니다.

recursive binary splitting을 수행하기 위해서는, 먼저, predictor $X_j$와 cutpoint $s$를 선택합니다. 이때 예측 변수 공간을 $\\{ X \mid X_j < s \\}$ 및 $\\{ X \mid X_j \geq s \\}$ 영역으로 분할하는 것이 RSS의 가장 큰 감소로 이어지도록 하는 $X_j$와 $s$를 선택합니다. 즉, 모든 predictor $X_1, ..., X_p$와 각 predictor에 대하여 가능한 모든 cutpoint $s$ 값을 고려한 다음, 결과로 나오는 트리가 가장 낮은 RSS 값을 갖도록 predictor와 cutpoint를 선택합니다.

$R_1 \left( j, s \right) = \\{ X \mid X_j < s \\} \quad and \quad R_2 \left( j, s \right) = \\{ X \mid X_j \geq s \\}$

$\sum_{i: x_i \in R_1(j,s)} \left( y_i - \hat{y}\_{R_1} \right)^2 + \sum_{i: x_i \in R_2(j,s)} \left( y_i - \hat{y}_{R_2} \right)^2$

다음으로, 결과로 나오는 각 영역 내에서 RSS를 최소화 할 수 있도록 데이터를 더 분할하기 위해 최적의 predictor와 최적의 cutpoint를 찾아 이 과정을 반복합니다. 이때에는 전체 predictor space를 분할하는 것이 아니라 이전에 분할한 영역 중 하나를 분할합니다. 즉 이전에 분할한 2개의 영역 중 하나를 분할하여 3개의 영역을 만들어냅니다. RSS를 최소화하기 위해 다시 이 3개의 영역 중 하나를 분할합니다. 이러한 과정을 정지규칙에 도달할 때까지 반복합니다.

일단 영역 $R_1, ..., R_J$가 생성되면, 주어진 test observation이 속한 영역의 training observation에 대한 response의 평균을 사용하여 해당 test observation에 대한 response을 예측합니다.

![]({{site.url}}/assets/images/fig8-3.png)

**Tree Pruning**

위 과정은 training set에 대해서는 좋은 예측을 산출해낼 수도 있지만, test set에 대해서는 오버피팅으로 인해 좋은 예측을 산출해내지 못할 가능성이 있습니다. 이는 결과로 나오는 트리가 너무 복잡할 수 있다는 점에 기인합니다. 따라서 더 적은 분할 즉 더 적은 영역 $R_1, ..., R_J$을 갖는 작은 트리를 통해 더 낮은 variance와 더 나은 해석을 얻을 수 있습니다.

이에 대한 가장 좋은 대안은 아주 큰 트리 $T_0$를 성장시키고, 그러고 나서 가지치기를 하여 하위 트리를 얻는 것입니다. 즉 목표는 가장 낮은 test error rate로 이어지는 하위 트리를 선택하는 것입니다. 하위 트리가 주어지면, CV 등을 통해 test error를 추정할 수 있습니다. 그러나 모든 가능한 하위 트리에 대해 CV error를 추정할 수는 없으므로, 하위 트리의 작은 집합을 선택하는 방법 즉 Cost complexity pruning = Weakest link pruning을 사용합니다. 이는 모든 가능한 하위트리를 고려하는 것 대신, 튜닝 파라미터 $\alpha$에 의해 인덱싱된 트리들의 순서를 고려합니다. 

각각의 $\alpha$값에 대하여 대응되는 다음의 식을 가능한 한 작게 만드는 하위 트리 $T \subset T_0$가 존재합니다.

$\sum_{m=1}^{\left\vert T \right\vert} \sum_{x_i \in R_m} \left( y_i - \hat{y}_{R_m} \right)^2 + \alpha \left\vert T \right\vert$

- $\left\vert T \right\vert$ : 트리 T의 터미널 노드의 숫자
- $R_m$ : $m$번째 노드에 
- $\hat{y}_{R_m}$ : predicted response

튜닝 파라미터 $\alpha$는 하위 트리의 복잡성과 training data에 대한 적합 사이의 trade-off를 제어합니다. $\alpha = 0$이면, 위 식은 training error를 측정하는 식이 되기 때문에 하위 트리 $T$는 단순히 $T_0$가 됩니다. $\alpha$가 증가함에 따라, 많은 터미널 노드가 있는 트리를 갖는 것에 대하여 지불해야 하는 대가가 있으므로, 위 식은 더 작은 하위 트리에 대해 최소화 되는 경향이 있을 것입니다.

위 식에서 $\alpha$를 0에서부터 증가시킴에 따라, 트리로부터 가지치기가 수행되므로 $\alpha$에 대한 함수로서 하위 트리들의 전체 순서를 얻는 것이 쉽습니다. CV 등을 사용하여 $\alpha$의 값을 선택할 수 있고, 그러고 나서 전체 데이터셋으로 돌아가 $\alpha$에 해당되는 하위 트리를 얻습니다. 

1. Use recursive binary splitting to grow a large tree on the training data, stopping only when each terminal node has fewer than some minimum number of observations.
2. Apply cost complexity pruning to the large tree in order to obtain a sequence of best subtrees, as a function of $\alpha$.
3. Use K-fold cross-validation to choose $\alpha$. That is, divide the training observations into $K$ folds. For each $k = 1, ..., K$:  
    (a) Repeat Steps 1 and 2 on all but the $k$th fold of the training data.   
    (b) Evaluate the mean squared prediction error on the data in the left-out kth fold, as a function of $\alpha$.  
    Average the results for each value of $\alpha$, and pick $\alpha$ to minimize the average error.
4. Return the subtree from Step 2 that corresponds to the chosen value of $\alpha$.

### 8.1.2 Classification Trees

앞서 살펴본 회귀 트리는 양적 response를 예측하는 데에 사용합니다. 대조적으로, <U>분류 트리는 질적 response를 예측하는 데에 사용합니다.</U> 회귀 트리에서는, 같은 터미널 노드에 속하는 training observation의 response의 평균으로 observation에 대한 response를 예측합니다. 반면 분류 트리에서는, <U>같은 터미널 노드에 속하는 training observation의 response의 클래스 중 가장 흔하게 발생하는 클래스로 observation에 대한 response를 예측합니다.</U> 때때로 특정 터미널 노드 영역에 해당하는 클래스 예측 뿐만 아니라, 특정 토미널 노드 영역에 해당하는 클래스 비율에 관심을 갖기도 합니다.

회귀 트리와 마찬가지로, 분류 트리를 성장시키기 위해서 recursive binary splitting을 사용합니다. 회귀 트리에서는 binary split을 만드는 기준으로 RSS를 사용합니다. 분류 트리에서는 binary split을 만드는 기준으로 **Classification error rate**, **Gini index**, **Entropy**를 사용합니다.

먼저 **Classification error rate**입니다.

$ E = 1 - \max\_k \left( \hat{p}_{mk} \right)$

- $\hat{p}_{mk}$ : the proportion of training observations im the $m$th region that are from the $k$th class

오분류율은 해당 영역에서 가장 일반적인 클래스에 속하지 않는 training observation의 비율을 의미합니다. 그러나 이는 트리를 성장시키는 데에 충분히 민감하지 않다고 여겨져 이에 대한 대안으로 **Gini index**가 등장합니다.

$ G = \sum_{k=1}^K \hat{p}\_{mk} \left( 1 - \hat{p}\_{mk} \right) $

- $\hat{p}_{mk}$ : the proportion of training observations im the $m$th region that are from the $k$th class

지니 계수는 $K$개의 클래스에 걸친 total variance를 나타냅니다. 지니 계수는 모든 $\hat{p}_{mk}$가 0 또는 1에 가깝다면 작은 값을 갖습니다. 이러한 이유로 지니 계수는 node purity에 대한 척도로 여겨집니다. 지니 계수가 작은 값을 갖는다는 것은 노드가 주로 단일의 클래스의 관측치를 포함함을 나타내기 때문입니다. 지니 계수에 대한 대안으로 **Entropy**도 존재합니다.

$ D = - \sum_{k=1}^K \hat{p}\_{mk} \log \hat{p}\_{mk} $

- $\hat{p}_{mk}$ : the proportion of training observations im the $m$th region that are from the $k$th class

엔트로피는 지니 계수와 마찬가지로 $m$번째 노드가 순수하다면 작은 값을 갖습니다. 실제로 지니 계수와 엔트로피는 수치 상 상당히 비슷하다고 합니다.

### 8.1.3 Trees vs. Linaer Models

- $Linear \; Regression : f(X) = \beta_0 + \sum_{j=1}^p X_j \beta_j$
- $Regression \; Trees : f(X) = \sum_{m=1}^M c_m \cdot 1_{\left( X \in R_m \right)}$ 

**Which model is better?**

feature와 response 사이의 관계가 선형적인 경우에는 선형 모델을 사용하는 것이 더 좋은 결과를 가져올 수 있습니다. 반면 feature과 response 사이의 관계가 비선형적이고 복잡한 경우에는 의사결정트리를 사용하는 것이 더 좋은 결과를 가져올 수 있습니다. 

![]({{site.url}}/assets/images/fig8-7.png)

- 상단 : true decision boundary = linear 일 때 선형 모델 (왼쪽) 및 의사결정트리 (오른쪽)
- 하단 : true decision boundary = non-linear 일 때 선형 모델 (왼쪽) 및 의사결정트리 (오른쪽)

물론 이러한 test error 측면 이외에 다른 측면을 고려해볼 수도 있습니다. <U>예를 들어, 모델의 해석(interpretability)과 시각화(visualization)를 위해 트리를 사용하는 것을 선호할 수도 있습니다.</U>

### 8.1.4 Advantages and Disadvantages of Trees

- **장점**  
  - 트리는 사람들에게 설명하기에 매우 쉽습니다.
  - 트리가 다른 접근들보다 인간의 의사결정을 더 잘 반영한다고 볼 수 있습니다.
  - 트리는 시각적으로 표현될 수 있고, 그래서 심지어 비전문가조차도 쉽게 해석할 수 있습니다.
  - 트리는 더미 변수를 생성하지 않고도 질적 predictor를 쉽게 다룰 수 있습니다.
- **단점**  
  - 트리는 일반적으로 예측 정확도가 좋지 않습니다.
  - 트리는 매우 non-robust 할 수 있습니다. 즉, 데이터의 작은 변화가 최종 트리에 큰 변화를 야기할 수 있습니다.


