---
title: '[ISL] Ch.3 Linear Regression'

categories:
  - Machine Learning
tags:
  - Machine Learning
  - ML
  - ISL
  - Regression

last_modified_at: 2020-04-301T08:06:00-05:00
---


# 3.1 Simple Linear Regression

- ### 정의
#### Linear Regression(선형 회귀)

    : Linear Regression(선형 회귀)란 종속 변수 y와 한 개 이상의 독립 변수 또는 Predictor X와의 선형 상관 관계를 모델링하는 회귀분석 기법이다.

 #### Simple Linear Regression(단순 선형 회귀)

    : 그 중에서 설명변수가 딱 하나인 경우가 Simple Linear Regression(단순 선형회귀)
<br>
$\qquad$
가정한 관계식: $Y = \beta_0 + \beta_1X + \epsilon$ $\rightarrow$ 이 때 $\epsilon$은 error term  
<br>
$\qquad$
종속변수 추정: $ \hat{y} = \hat{\beta_0} + \hat{\beta_1}X $ <br>

$\quad\qquad\qquad\rightarrow$ 우리의 할 일은 이 $\beta_0$과 $\beta_1$을 추정하는 것


- ### Coefficients 추정
#### $\beta_0$ 와 $\beta_1$ 추정

   1. 어떤 값이 $\beta_0$와 $\beta_1$의 좋은 추정량이 될까?

    실제 y값과 $\beta_0$와 $\beta_1$를 이용하여 추정한 종속변수 $\hat{y}$간의 차이가 최대한 적도록 만드는 값   
   <br>
   2. 그럼 실제 y값과 $\hat{y}$간의 차이는 어떻게 구할까?  

    보통 실제 y값에서 $\hat{y}$값을 뺀 것에 제곱을 취한 것들을 다 더해서 구한다. 이를 RSS(Residual Sum of Squares)라 함 \
   $\quad RSS = \sum_{i=1}^n (y_i-\hat{\beta_0}-\hat{\beta_1}X_i)$  
   <br>
   3. RSS는 Quadratic Form이다. 따라서 RSS를 각각 $\beta_0$와 $\beta_1$로 미분하면 RSS를 최소로 만들어주는 $\beta_0$와 $\beta_1$값을 찾을 수 있다(정확한 유도 과정은 생략).
   <br>
   $\hat{\beta_1} = \frac{\displaystyle \sum_{i=1}^n (x_i-\bar{x})(y_i-\bar{y})}{\displaystyle \sum_{i=1}^n (x_i-\bar{x})^2}$
   $\quad$ $이\ 때\ \bar{x} = x\ 의\ 평균,\ \bar{y} = y\ 의\ 평균$
   <br>

   $\hat{\beta_0} = \bar{y}-\hat{\beta_1}\bar{x}$


   - ### Coefficient 추정의 가설 검정
#### $\beta_0$ 와 $\beta_1$ 가설 검정

 1. 일단 우리는 $\hat{\beta_0}$ 와 $\hat{\beta_1}$를 통해 모델을 추정했다. 하지만 $\beta_0$ 와 $\beta_1$이 실제로 존재하는 지(0값이 아닌지)에 대해서는 아직 확실하지 않다.
 <br>

 2. 따라서 가설검정을 통해 $\beta_0$ 와 $\beta_1$이 실제로 존재하는 지에 대해 알아보자.
 <br>

 3. 우리가 사용할 검정방법은 t-test이다(유도 과정 생략). t-test를 하기 위해서는 $\hat{\beta_0}$ 와 $\hat{\beta_1}$의 Standard Error를 구해야 한다.
 <br>

 $\qquad SE(\hat{\beta_0})^2 = \sigma^2[\frac{1}{n}+\frac{\bar{x}^2}{\displaystyle \sum_{i=1}^n (x_i-\bar{x})^2}]$ \
 $\qquad SE(\hat{\beta_1})^2 = [\frac{\sigma^2}{\displaystyle \sum_{i=1}^n (x_i-\bar{x})^2}]$ \
 $\qquad$이 때 $\sigma^2$은 $Var(\epsilon)$
 <br>

 4. 이제 우리가 구한 $\hat{\beta}$값들과 이것들의 SE값들을 이용해서 가설검정을 하자($\beta_0$와 $\beta_1$의 가설검정 방법이 동일하므로 $\beta_1$만 서술).
 <br>

   4-1.

   $\;$귀무가설($H_0$): $\beta_1=0$   

   $\;$대립가설($H_1$): $\beta_1 \ne 0$

   4-2.

   $\;$검정통계량 $t=\frac{\hat{\beta_1}-0}{SE(\hat{\beta_1})}$

   4-3.

   $\;$p-value 구하기: 위에서 구한 검정통계량값으로 n-2 자유도를 지닌 t 분포위에서의 우측 또는 좌측 꼬리 확률을 구함.

   4-4.

   $\;$결론: 우리가 설정한 유의수준보다 p-value가 낮다면 귀무가설 기각 $\rightarrow \beta_1$값은 0이 아니다.


   - ### 모델 평가
   #### $R^2$

    모델을 평가하는 데에는 여러 방법이 있지만 그 중에서 대표적인 $R^2$ 값에 대해 알아보자.  

    1. Formula
    <br>

       $R^2=\frac{TSS-RSS}{TSS}=1-\frac{RSS}{TSS}$  
    <br>   
    2. TSS & RSS
    <br>
       TSS와 RSS는 각각 Total Sum of Squares와 Residaul Sum of Squares를 의미하며

       $TSS=\sum_{i=1}^n (y_i-\bar{y})^2, \quad RSS = \sum_{i=1}^n (y_i-\hat{y_i})^2$
    <br>

    3. 의미
    <br>
      반응 변수의 변동량 중에서 적용한 모형으로 설명가능한 부분의 비율을 가리킨다. 라고 하면 너무 어려우니..

      추정한 선형 모형이 주어진 자료에 적합한 정도를 재는 척도라고 생각하면 편하다!

      1에 가까울수록 주어진 자료에 모델이 적합하다는 뜻.


# 3.2 Multiple Linear Regression

- ### 정의
#### Multiple Linear Regression(다중 선형 회귀)

    : Multiple Linear Regression이란 Predictor가 하나가 아니라 여러 개인 선형 회귀를 의미한다.
    <br>
$\qquad$
가정한 관계식: $Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \dots + \epsilon$ $\rightarrow$ 이 때 $\epsilon$은 error term  
<br>
$\qquad$
종속변수 추정: $ \hat{y} = \hat{\beta_0} + \hat{\beta_1}X_1 + \hat{\beta_2}X_2 + \dots $ <br>

$\quad\qquad\qquad\rightarrow$ 우리의 할 일은 이 $\beta_i$값들을 추정하는 것

- ### Coefficients 추정
#### $\beta_i$ 추정

   1. 기본적인 원리는 Simple과 동일하다.

    실제 y값과 $\beta_i$들을 이용하여 추정한 종속변수 $\hat{y}$간의 차이가 최대한 적도록 만드는 $\beta_i$값을 찾으면 됨   
   <br>
   2. 실제 y값과 $\hat{y}$간의 차이를 구하는 방법도 동일  

   $\quad RSS = \sum_{i=1}^n (y_i-\hat{\beta_0}-\hat{\beta_1}X_{i1}-\hat{\beta_2}X_{i2}-\dots)$  
   <br>
   3. 복잡해보이지만 이번에도 RSS는 Quadratic Form이다. 따라서 RSS를 각각의 $\beta_i$들로 미분하면 RSS를 최소로 만들어주는 $\beta_i$값들을 찾을 수 있다.
   <br>

   4. 그런데 이번에는 X 변수들이 좀 많으니 그냥 미분하기는 어려울 것 같고, 행렬을 이용한다.
   <br>

   5. 우리가 가정한 관계식 $Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \dots + \epsilon$을 실제 데이터에 적용하면

   $Y_1=\beta_0+\beta_1X_{1,1}+\dots+\beta_pX_{1,p}+\epsilon_1$  
   $Y_2=\beta_0+\beta_1X_{2,1}+\dots+\beta_pX_{2,p}+\epsilon_2$  
   $\vdots$  
   $Y_n=\beta_0+\beta_1X_{n,1}+\dots+\beta_pX_{n,p}+\epsilon_n$  

   그리고 Intercept를 포함한 X변수의 값들만 뽑아 행렬로 나타내면  
   $\mathbf{X}=\begin{bmatrix} 1 & X_{1,1} &\cdots & X_{1,p} \\ \vdots & \ddots & \vdots \\ 1 & X_{n,1} & \cdots & X_{n,p} \end{bmatrix}$  
   <br>  

   6. 이제 이 X 행렬을 이용해서 $\hat{\beta}$의 컬럼 벡터를 구해보자.

   $\hat{\beta}=argmin_\beta(Y-X\beta)^T(Y-X\beta)=(X^TX)^{-1}X^TY$



   - ### Coefficient 추정의 가설 검정
#### $\beta_i$가설 검정

 1. 이번에도 우리가 구한 Coefficient 추정량에 대해 가설검정을 해보자.
 <br>

 2. 그런데 이번에는 세 가지 검정이 있다.
 <br>
  1)모든 $\beta_i$들에 대한 검정. 2)특정 $\beta_i$들에 대한 검정. 3)특정 $\beta_i$ 하나에 대한 검정  
 <br>

  <br>

  $\qquad$ 3-1. 모든 $\beta_i$ 대한 검정.
  <br>

   $\qquad$$\;$귀무가설($H_0$): 모든 $\beta_i=0$   

   $\qquad$$\;$대립가설($H_1$): 적어도 하나의 $\beta_i \ne 0$

   $\qquad$$\;$검정통계량: $F=\frac{(TSS-RSS)/p}{RSS/(n-p-1)}$ $\quad$이 때, p는 Predictor의 개수(Intercept제외), n는 데이터 개수.

   $\qquad$$\;$p-value: 자유도(p, n-p-1)의 F분포에서 검정통계량으로부터 우측 꼬리 확률을 구한다.

   $\qquad$$\;$결론: p-value가 우리의 유의수준보다 작으면 귀무가설 기각 $\rightarrow$ 적어도 하나의 $\beta_i \ne 0$

 <br>

 $\qquad$ 3-2. 특정 $\beta_i$들에 대한 검정.
  <br>

   $\qquad$$\;$ - 귀무가설($H_0$): 특정 $\beta_i들=0$  $\rightarrow$ 이를 Reduced Model이라 한다.

   $\qquad$$\;$ - 대립가설($H_1$): 모든 $\beta_i \ne 0$ $\rightarrow$ 이를 Full Model이라 한다. 3-1과 3-2의 가설과는 형태가 다르므로 주의.

   $\qquad$$\;$ - 검정통계량: $F=\frac{(RSS(R)-RSS(F))/(df_R-df_F)}{RSS(F)/(df_F)}$$\quad$ 이 때, $\;df_R=n-k,\; df_F=n-p-1,\;$ k=Reduced Model에서 Predictor의 개수 + 1

   $\qquad$$\;$ - p-value: 자유도(p+1-k, n-p-1)의 F분포에서 검정통계량으로부터 우측 꼬리 확률을 구한다.

   $\qquad$$\;$ - 결론: p-value가 우리의 유의수준보다 작으면 귀무가설 기각 $\rightarrow$ Full Model이 맞다.

 <br>

 $\qquad$ 3-3. 특정 $\beta_i$에 대한 검정
 <br>

   $\qquad$$\;$귀무가설($H_0$): 해당 $\beta_i=0$   

   $\qquad$$\;$대립가설($H_1$): 해당 $\beta_i \ne 0$

   $\qquad$$\;$검정통계량: $t=\frac{\hat{\beta_i}-0}{SE(\hat{\beta_i})}$ $\; SE(\hat{\beta_i})=C_{ii}\times RSS/(n-p-1)$ 이 때 $\;C_{ii}=(X'X)^{-1}\sigma^2$의 i번째 대각원소.

   $\qquad$$\;$p-value: 자유도(n-p-1)의 t분포에서 검정통계량으로부터 우측 또는 좌측 꼬리 확률을 구한다.

   $\qquad$$\;$결론: p-value가 우리의 유의수준보다 작으면 귀무가설 기각 $\rightarrow$ 해당 $\beta_i \ne 0$



   - ### 모델 평가
   #### $R^2$

    이번에도 $R^2$값을 통해 모델을 평가할 수 있다.  

    1. Formula
    <br>

       $R^2=\frac{TSS-RSS}{TSS}=1-\frac{RSS}{TSS}$ $\rightarrow$ Simple Linear 때와 동일
    <br>

    2. Adjusted $R^2$
    <br>
       그런데 위의 $R^2$를 그대로 사용하기에는 조금 문제가 있다. 왜냐하면 $R^2$는 변수의 개수가 많아질수록 높아지기 때문이다.

       따라서 우리는 $R^2$를 조금 조정한 Adjusted $R^2$를 사용할 것

       ${R_a}^2=1-\frac{RSS/(n-p-1)}{TSS/(n-1)}$ (RSS와 TSS를 각각의 자유도로 나누어 조정)
    <br>

    3. 의미
    <br>
      주어진 자료에 내 모델이 얼마나 적합한 지를 나타냄. 1에 가까울수록 적합하다고 판단.



      - ### 예측
      #### Interval Estimation of $E(Y_{h(new)})$

       보통 우리는 Model을 만들고 나면 새로운 관측값이 나타날 경우 그 모델에 관측값을 집어넣으면 그것으로 예측이 끝이라고 생각하는 경우가 종종 있다. \
       하지만 우리는 Model을 만들 때 Error($\epsilon$)라는 놈을 상정했다. 따라서 모델에 관측값을 집어넣었을 때 튀어나오는 애는 단지 그 관측값의 종속변수에 대한 기대값이다. 그러므로 좀 더 정확한 예측을 위해서는 기대값에 대한 Interval Estimation을 구할 필요가 있다.
       <br>

       - Prediction Interval of $Y_{h(new)}$

          $\rightarrow$ $\hat{Y_h} \pm t(1-\frac{\alpha}{2};n-p-1)\times s(pred)$ 이 때, $s(pred)^2=\frac{RSS}{n-p-1} + var(\hat{Y_h})$
       <br>

         ** 좀 더 정확한 해석은 여기서는 생략.



# 3.3. 고려사항

- ### Qualitative Predictors
#### Dummy Variable

 만약에 우리가 가지고 있는 자료의 Predictor들이 모두 Quantitative 형태라면 문제가 없다. 하지만 실제로 그런 일은 흔치 않고 항 Qualitative 변수가 섞여있기 마련이다. 따라서 우리는 Qualitative 변수가 포함되어있는 경우의 Regression Model에 대해 고려해보아야 한다.

  우리는 위에서 Predictor들을 따로 뽑아서 X행렬을 만들었고 이를 통해 베타값들을 추정했다. 이번에도 마찬가지로 원리는 동일하다. X 행렬을 만들고 이를 통해 베타값을 추정할 것이다.

  여기서 문제는 X 행렬에서 Qualitative 변수의 값들을 어떻게 처리할 것인가이다.

  그 방법은 Dummy Variable을 새로 만들어주는 것이다.

  이 때 Dummy Variable은 Qualitative 변수 내에서 해당되는 값이 있다면 1을, 아니면 0을 출력하는 변수들을 새롭게 만들어서 합친 것이다.

  그냥 정의로 얘기하면 어려우니 예를 들어 보자. 혈액형에 대해 조사한 변수가 있다고 하자. 그렇다면 혈액형은 총 네가지이므로, 변수를 세 개를 더하여 표현이 가능하다.

  $\; X_{1} \; X_{2} \; X{3}$  
  $\begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{bmatrix}$  

  첫 번째 행: A형  
  두 번째 행: B형  
  세 번째 행: O형  
  네 번째 행: AB형

  #### 회귀식

 Dummy Variable을 이용해서 베타의 추정값들을 구한 뒤 유의해야 할 점이 있다. 바로 회귀식의 형태이다.
 만약 Qualitative 변수 내의 항목들이 배타적인 성격을 가진다면(예를 들어 혈액형은 네 가지 중 하나만 가능), 어떤 항목에 해당하느냐에 따라 회귀식은 달라진다.

 예를 들어, 앞선 혈액형의 예시에서 $X_1, X_2, X_3$ 에 해당하는 베타들이 각각 $\beta_1, \beta_2, \beta_3$라고 하자.

 그러면 A형인 사람의 회귀식에는 $\beta_1$만 포함되고 $\beta_2, \beta_3$는 제외된다. 마찬가지로 AB형인 사람의 경우 $\beta_1, \beta_2, \beta_3$가 모두 제외된다.


 - ### Interactions
#### Interaction Predictors

 Predictor들이 서로 Interaction(상호작용)이 없다면 간단하겠지만 실제 세상은 역시 녹록지 않다. 그러므로 Predictor들끼리 Interaction이 있는 경우를 생각해보자.

 Interaction이 있다고 가정하는 경우 우리는 Interaction Term 또는 Interaction Variable을 새로 포함시켜 주어야 한다.

 1. Quantitative X Quantitative

   우선 Quantitative 변수끼리의 상호작용을 생각해보자.

   예를 들어 $X_1$과 $X_2$ Predictor들이 있고 이것들끼리의 Interaction이 있다고 하자. 그러면 우리는 $X_1X_2$ Variable을 회귀식에 포함시켜준다.

   그리고 이를 통해서 회귀 모델을 만들면

   $E(Y)=\hat{\beta_0}+\hat{\beta_1}X_1+\hat{\beta_2}X_2+\hat{\beta_3}X_1X_2$

   이런 형태로 나올 것이다.

   그리고 이는 $E(Y)=\hat{\beta_0}+(\hat{\beta_1}+\hat{\beta_3}X_2)X_1+\hat{\beta_2}X_2$로 정리할 수 있다.
   <br>

 2. Quantitative X Qualitative

   이번에는 Quantitative와 Qualitative 변수간의 상호작용을 생각해보자.

   예를 들어 income(Quantitative)과 student여부(Qualitative)간의 상호작용을 고려하면 다음과 같은 회귀식을 만들 수 있다.(종속변수는 잔고)

   $E(Y)=\hat{\beta_0}+\hat{\beta_1}\times income+\begin{cases} \hat{\beta_2}+\hat{\beta_3}\times income \quad \text{if student} \\ 0  \;\;\;\qquad\qquad\qquad \text{if not} \end{cases}$

   위에서와 마찬가지로 이 역시도 income에 대해서 정리하면

   $E(Y)=\begin{cases} (\hat{\beta_0}+\hat{\beta_2})+(\hat{\beta_1}+\hat{\beta_3})\times income \quad \text{if student} \\ \hat{\beta_0}+\hat{\beta_1}\times income  \;\;\;\quad\qquad\qquad \text{if not} \end{cases}$


   - ### Nonlinear Relationships
   #### polynomial Regressions

    Linear Regression 챕터에서 갑자기 Nonlinear Relationship을 다루는게 띠용할 수 있겠지만 Nonlinear Relationship이더라도 모델이 Linear이기 때문에 이 챕터에서 다룬다. 이게 무슨 소리인가 싶을테니 한번 살펴보자.

    우리는 지금까지 회귀식을 1차 다항식에 대해서만 다뤘는데 2차, 3차 이렇게 확장해나갈 수 있다. 그리고 이렇게 확장한 회귀를 Polynomial Regression이라고 한다.

    예들 들어서 우리가 R에서 흔히 다루는 Auto 데이터에서 mpg와 horsepower간의 관계를 Polynomial Regression으로 나타내보면,

    $mpg = \beta_0 + \beta_1\times horsepower + \beta_2\times horsepower^2 + \epsilon$

    과 같이 나타낼 수 있다. 그런데 이 때 $horsepower$와 $horsepower^2$를 $X_1$과 $X_2$라는 각각 하나의 변수로 본다면

    얘는 그냥 우리가 계속 봐왔던 Linear Model이다.


    - ### ETC

     그 밖에도

     1) Non-linearity of the Data  
     2) Correlation of error terms  
     3) Non-constant variance of error terms  
     4) Outliers  
     5) High-leverage points  
     6) Collinearity

     등의 문제가 나타날 수 있다.

     (Linear Regression을 적용하는게 여간 어려운 일이 아니다..ㅠㅠ)

     특히 이 중에서 Collinearity 문제는 중요해서 추후 6장에서 그 해결법(Variable Selection, Extraction)에 대해 자세히 다룰 예정이다.
