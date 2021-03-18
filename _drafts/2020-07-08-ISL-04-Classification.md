---
title: '[ISL] Ch.4 Classification'

categories:
  - Machine Learning
tags:
  - Machine Learning
  - ML
  - ISL
  - Classification

last_modified_at: 2020-04-301T08:06:00-05:00
---


# 4.1 Overview

- ### Regression VS Classification
#### Regression과 Classification 차이


 - Regression: 종속변수가 수치형(Quantitative, Numerical)  
   ex)나이, 몸무게, 소득, 주가 등


 - Classification: 종속변수가 범주형(Qualitative, Categorical)  
   ex)성별, 브랜드, 감염 여부 등


 - Classification에서 $y_0$의 추정은 곧 $y_0$를 분류(Classification)하는 것.


 - 대표적인 Classifier:  
   Bayes Classifier  
   K-Nearest Neighbors  
   Logistic Regression  
   Linear Discriminant Analysis  
   etc.


  # 4.2 Why not Linear Regression?

  - ### Linear Regression의 한계

    만약 종속변수가 범주형인 경우, 각 항목에 해당하는 값을 어떻게 설정하느냐에 따라 결과가 달라지는 문제가 있다.

    예를 들어  
    $Y=\begin{cases} 1\; \text{if A} \\ 2\; \text{if B} \\ 3\; \text{if C} \end{cases}$  

    이렇게 설정한 거랑  

    $Y=\begin{cases} 1\; \text{if C} \\ 2\; \text{if B} \\ 3\; \text{if A} \end{cases}$  

    이렇게 설정한 것은 결과값이 다르게 나온다.

    #### Binary Case

    Y의 범주가 2개인 Binary Case의 경우에 $Y\in \{0, 1\}$으로 나타내어 추정값인 $\hat{y_0}$가 0.5를 넘는 경우 Y=1로 분류하고 0.5미만인 경우 0으로 분류할 수 있다.

     이 때 $\hat{Y}=X^T\beta$는 조건부 확률인 P(Y=1|X)으로 해석 가능.

     그러나 문제는 OLS로 Fitting할 시, P(Y=1|X)값이 0보다 작거나 1보다 큰 경우가 발생할 수 있다. 따라서 이러한 문제를 해결한 것이 바로 Logistic Regression



# 4.3 Logistic Regressions

- ### Linear Regression과 Logistic Regression의 차이

![nn](Logi.png)

- ### The Logistic Model

 우리가 가진 데이터 X로 $p(X)=p(Y=1|X)$를 fitting 해보자.

 OLS를 그대로 사용하는 경우 $p(X)=\beta_0+\beta_1X$

 여기서 우변의 함수 형태를 다음과 같이 바꿔보면 $p(X)$가 [0,1]구간 안에 포함된다!

 $p(X)=\frac{exp(\beta_0+\beta_1X)}{1+exp(\beta_0+\beta_1X)}$

 우변이 $\beta_0+\beta_1X$형태가 되도록 식을 다시 정리하면,

 $log(\frac{p(X)}{1-p(X)})=\beta_0+\beta_1X$

 여기서 좌변에 해당하는 $log(\frac{p(X)}{1-p(X)})$를 우리는 Y=1일 확률의 log-odds 또는 logit이라 한다.

 따라서 Logistic Regression은 Y=1일 확률의 logit에 대한 선형 모델이라고 할 수 있다.

 - ### The Logistic Model에서 $\beta$의 의미

   $\frac{p(X)}{1-p(X)}=exp(\beta_0+\beta_1X)$의 관계식에서 알 수 있듯이

   X의 한 단위 변화는 "log odds"를 $\beta_1$만큼 변화시킨다.

   즉, X가 한 단위 변화한다고 해서 p(X)의 값이 $\beta_1$값만큼 증가, 또는 감소하는 관계가 아니다.

   X값의 변화에 따른 p(X)값의 변화는 1) 회귀계수의 크기와 방향 2) 현재 X의 수준에 따라 다르다.

   - ### Maximum Likelihood Estimation of $\beta$

     Logistic Regression 에서 가정한 개별 y의 Sampling Density의 식은 다음과 같다.

     $\bullet$ Sampling Density of $y|X = \begin{cases} p(y=1|X)=p(X)=\frac{exp(\beta_0+\beta_1X)}{1+exp(\beta_0+\beta_1X)} \\ p(y=0|X)=1-p(X)=1-\frac{exp(\beta_0+\beta_1X)}{1+exp(\beta_0+\beta_1X)} \end{cases}$

     N개의 $(x_i,y_i)$ iid sample 데이터에 대한 Joint Sampling Density는 다음과 같다(이 때 Y는 벡터).

     $\bullet$ Joint Sampling Density of $p(Y|X)=\prod_{i|y_i=1}p(X_i)\prod_{j|j=0}(1-p(X_j))$

     얘를 MLE의 측면에서 해석하면, 데이터가 주어졌을 때 $\beta$의 식, Joint Likelihood로 볼 수 있다. 이 식을 최대화하는 $\beta$를 추정하는 것이 MLE

     $\bullet$ Joint Likelihood function $L(\beta_0,\beta_1|Y,X)=\prod_{i|y_i=1}p(X_i)\prod_{j|j=0}(1-p(X_j))$


     - ### Response with more than 2 Clases

       위에서는 종속변수가 2개인 경우를 살펴보았다. 그러면 세 개인 경우는 어떨까??

       $Y \in \{A,B,C \}$인 경우를 생각해보자.

       $\bullet$ Sampling Density of $y|X = \begin{cases} p(y=A|X)=\frac{exp(X^T\beta_A)}{1+exp(X^T\beta_A)} \\ p(y=B|X)=\frac{exp(X^T\beta_B)}{1+exp(X^T\beta_B)} \\
     p(y=C|X)=1-p(y=A|X)-p(y=B|X) \end{cases}$  

       $\bullet$ Likelihood $L(\beta_A,\beta_B)=\prod p(A|X)\prod p(B|X)\prod p(C|X)$

       $\bullet$ 이 경우 범주가 추가될 때마다 추정해야 하는 모수의 개수가 p개 만큼 추가된다.

       $\bullet$ 그러나 범주가 2개보다 많은 경우에는 이 방법보다는 대부분 다른 방법을 쓴다. 대표적인 겻이 다음에 나오는 Linear Discriminant Analysis

# 4.4 LDA

- ### LDA Intuition

  Classification Parametric Methods 근본 원리는 동일하다. 결국 Bayes Classifier를 추정하여 가장 높은 값을 가지는 애를 찾고 그에 해당되는 class를 확인하는 것.

  $\bullet$ Bayes Classifier: $p_k(X)=p(Y=k|X=x)$

  그런데 우리가 하는 일은 Bayes Classifier의 절대적인 크기보다는 그 순서이다. 따라서 Bayes Classifier를 Monotone transformation 한 뒤 제일 큰 애를 골라내도 똑같음.

  $\bullet$ Discriminant: $\delta_k(X)=f(p(Y=k|X=x))$

  ** LDA도 Discriminant의 선형함수라고 할 수 있는데 다만 Logistic과의 차이라면 Logistic은 추정 방식에서 MLE를 사용, LDA는 Bayes Rule을 사용.

  - ### Estimating Bayes Classifier

    $\bullet$ Bayes Classifier $p(Y=k|X=x)=\frac{p(Y=k\; and\; X=x)}{p(X=x)}=\frac{p(Y=k)p(X=x|Y=k)}{\sum_kp(Y=k)p(X=x|Y=k)}=\frac{\pi_kf_k(X)}{\sum_{i=1}^K\pi_kf_i(X)}$

    $\bullet$ $\pi_k$:Prior probability of each class k $\rightarrow$ 보통 전체 N개의 관측치 중 class k인 관측치 개수의 비율로 쉽게 구함.

    $\bullet$ $p_k(X)=p(k|X)$: Predictor X가 주어졌을 때 관측치가 class k에 속할 확률. 우리가 구해야 하는 target.

    $\bullet$ $f_k(X)=p(X|k)$: Likelihood of X in class k. 우리가 가정하는 부분. 이것에 대한 가정에 따라 LDA와 QDA로 나뉜다.

    - ### LDA: p=1 One Predictor

      LDA에서는 X|y=k 가 Normal Distribution을 따른다고 가정한다.

      $\bullet$ $f_k(X)=\frac{1}{\sqrt{2\pi\sigma_k^2}}exp(-\frac{(x-\mu_k)^2}{2\sigma_k^2})$

      #### LDA의 가정

      $\bullet$ Different Mean & Shared Variance  

       각 클래스마다 X의 평균은 다르지만, 모든 클래스에서의 X의 분산은 동일하다. 즉, 클래스마다 $\sigma_k^2$를 따로 구할 필요가 없음. 그냥 $\sigma^2$으로 통일

       $\rightarrow$ $f_k(X)=\frac{1}{\sqrt{2\pi\sigma^2}}exp(-\frac{(x-\mu_k)^2}{2\sigma^2})$

       이를 Bayes Classifier $p(Y=k|X)$에 대입하면

       $\rightarrow$ $p(K|X)=\frac{\pi_k\frac{1}{\sqrt{2\pi\sigma^2}}exp(-\frac{(x-\mu_k)^2}{2\sigma^2})}{\sum_l\pi_l\frac{1}{\sqrt{2\pi\sigma^2}}exp(-\frac{(x-\mu_l)^2}{2\sigma^2})}$

       보다시피 식이 많이 복잡해보인다. 그러니까 좀 더 쉽게 간단하게 단조변환해보자.

       $\bullet$ Population Linear Discriminant: $\delta_k(x)=x\frac{\mu_k}{\sigma^2}-\frac{\mu_k^2}{2\sigma^2}+\log(\pi_k)$

       #### Parameter 추정

       그럼 이제 위 식에서 모수 $\mu_k$, $\sigma^2$, $\pi_k$를 추정해보자.

       $\bullet$ $\hat{\mu_k}$: class k인 관측치 x의 평균

       $\bullet$ $\hat{\sigma^2}$: 각 클래스 내에서의 편차 제곱을 모두 더해 N-k로 나눈 것

       $\bullet$ $\hat{\pi_k}$: 전체 관측치 중 class k의 비율  만약 모든 클래스 별 관측치의 개수가 동일한 경우, $log(\hat{\pi_k})$는 의미가 없음. 있으나 마나..

       $\rightarrow$ Sample Linear Discriminant: $\hat{\delta_k(x)}=x\frac{\hat{\mu_k}}{\hat{\sigma^2}}-\frac{\hat{\mu_k^2}}{2\hat{\sigma^2}}+\log(\hat{\pi_k})$

       #### 결론

       어떠한 관측치 $x_0$에 대해 각각의 class k에 대한 $\hat{\delta_k(x)}$를 모두 계산해 그 값이 가장 큰 class k로 관측치 분류

       - ### LDA: p>1 Multiple Predictor

         p>1의 경우 X|y=k ~ Class specific $MVN(\mathcal{M}_k,\sum)$

         $\bullet$ $f_k(X)=\frac{1}{(2\pi^2)^{p\, /\, 2}|\sum|^2} exp(-\frac{(X-\mathcal{M}_k)^T\sum^{-1}(X-\mathcal{M}_k)}{2})$

         $\bullet$ Population Linear Discriminant: $\delta_k(x)=x^T\sum^{-1}\mathcal{M}_k-\frac{1}{2}\mathcal{M}_k^T\sum^{-1}\mathcal{M}+\log(\pi_k)$

        #### Parameter 추정

        $\bullet$ $\hat{\mathcal{M}_k}$: class k에서 각 $x_i$ 관측치의 변수별 평균

         $\bullet$ $\hat{\sum_{i,j}^2}$: 각 클래스에서의 i변수와 j변수의 편차 곱의 합을 모두 더해 N-k로 나눈 것

         $\bullet$ $\hat{\pi_k}$: 전체 관측치 중 class k의 비율

         - ### Bayesian Decision Boundary

           set of $X=(x_1, x_2, ...,x_p)$ where $\delta_k(X)=\delta_l(X)\;$ for $k\ne l$

           $\rightarrow$ 즉, 서로 다른 class에 대해 같은 delta헷 값이 나온다면 걔는 경계선이 된다.

           <img src='BDB.png' align='left'>
           <img src='BDB2.png' align='left'>
           <br>
           <br>
           <br>
           <br>

           $\rightarrow$ 세가지 클래스에 대한 Bayesian Decision Boundary.   
           타원은 각 클래스가 95%의 확률로 나타날 구간을 의미

           $\rightarrow$ 오른쪽 그림은 새로운 data들이 나타났을 때 점선에서 실선으로 Boundary가 이동했음을 보여줌.


           - ### LDA: Training Error Rate

             #### Overfitting Problem

              LDA에서의 Training Error Rate는 대부분 Test Error Rate보다 낮다.  
             $\rightarrow$ Training Data로 Bayes Classifier를 추정하기 때문  
             $\rightarrow$ 이 때문에 만일 모수의 개수와 관측치의 비율인 $\frac{p}{N}$이 크면 Overfitting 문제가 발생할 수 있다.

             #### Null Classifier

             Y=0 or 1에서 그냥 모든 관측치를 Y=1로 분류하는 경우이다.  
             $\rightarrow$ 이 때의 Error Rate은 전체 관측치에서 0의 비율  
             $\rightarrow$ 만약 모델의 Training Error Rate가 이거보다 낮다면 안 쓰는게 낫다.

             - ### LDA: Confusion Matrix

               #### Confusion Matrix  

               타겟의 원래 클래스와 모형이 예측한 클래스가 일치하는지는 갯수로 센 결과를 표나 나타낸 것이다.   
               정답 클래스는 행(row)으로 예측한 클래스는 열(column)로 나타낸다.

               <img src='CM.png' align='left' >


               - ### LDA: Threshold and ROC, AUC

                 오류에는 두 가지 종류가 있다. 1)False Positive(Type I Error), 2)False Negative(Type II Error)

                 상황에 따라서 두 가지 오류 중 한 가지가 더 중요한 경우가 있다. 예를 들어, 파산 예측에서는 실제 파산할 사람인데(실제 True) 파산하지 않는다고 예측(분류 False)하는 경우가 더 큰 문제이다. 즉 False Negative를 줄이는 게 더 중요.

                 그런데 문제는 Bayes Classifier에서는 두 종류의 오류를 동등하게 취급. 따라서 두 가지의 오류 중 하나를 줄이고 싶다면 Threshold를 조정해주어야 한다.

                 #### Threshold

                 1. Threshold > 0.5 조절  

                  이 경우는 확률이 0.5보다 높아야 1로 분류하는 것이므로 1로 분류하는 것이 적어지는 상황. 따라서 True를 1이라고 뒀을 때, False Positive가 줄어들게 되고 False Negative는 높아지게 된다. 즉, 0.5보다 높이면 Type I Error는 줄고 Type II Error는 늘어난다.

                  결론: Type I Error 감소 II 증가
                  <br>

                 2. Threshold < 0.5 조절

                  위와 반대의 상황 따라서 Type II Error 감소 I 증가

                  앞서 파산의 예시에서는 2번을 선택해야 함.


                 <img src='Threshold.png' align='left'>


                 #### ROC & AUC

                 이를 ROC 커브로 그려보면

                 <img src='ROC.png' align='left'>

# 4.5 QDA

- ### QDA 가정

  QDA는 LDA랑 거의 동일하지만 차이가 있다면 분산의 가정에서 차이가 난다. LDA에서는 class 별 분산이 같다고 가정하였지만 QDA에서는 다르다고 본다.

  $\bullet$ Population Discriminant:  

  $\quad \delta_k(x)=-\frac{1}{2}(x-\mu_k)^T\sum_{k}^{-1}(x-\mu_k)-\frac{1}{2}\log|\sum_k|+\log\pi_k \\ \quad \qquad= -\frac{1}{2}x^T\sum_{k}^{-1}x+x^T\sum_{k}^{-1}\mu_k-\frac{1}{2}\mu_k^T\sum_k^{-1}\mu_k-\frac{1}{2}\log|\sum_k|+\log\pi_k $

  ** 보면 Discriminant가 X에 대한 Quadratic 함수이다. 이 때문에 QDA라 부름

  - ### Var-Bias Tradeoff

    $\bullet$ QDA를 사용하는 경우 Covariance Matrix에서 추정해야 하는 Parameter의 개수가 무려 $K\times \frac{p(p+1)}{2}$개이다.

    $\rightarrow$ 즉, QDA는 LDA에 비해 훨씬 유연하며, 따라서 training 데이터가 적은 경우 분산이 굉장히 클 수 있다.

    $\rightarrow$ 대신 실제로 Discriminant가 비선형인 경우, QDA를 사용함으로써 모델의 Bias를 줄일 수 있다.

    $\rightarrow$ 즉, Variance는 올라가는 대신, Bias는 줄어듦.


# 4.6 KNN

- ### Bayes Classifier

  $\bullet$ $p(Y=j|X=x_0)=\frac{1}{K}\sum_{i\in N_0}I(y_i=j)$

  $\rightarrow$ 어떤 점 $x_0$에서 '가장 가까운' K개의 점들의 집합을 $N_0$이라 함

  $\rightarrow$ 그 중 class j의 총 개수: $\sum_{i\in N_0}I(y_i=j)$

  $\rightarrow$ 이렇게 구한 추정 조건부 분포에서 가장 확률이 높은 class가 선택된다.

  $\rightarrow$ K의 값은 우리가 임의로 설정하는데, 더 많은 점을 볼수록 더욱 경직적인 Decision Boundary


# 4.7 Comparison

- ### Classification 방법 별 비교

  #### Logit vs LDA

  둘 다 Bayes Classifier의 선형 모수 추정이라는 점에서 비슷하지만 가정이 다르다.

  $\rightarrow$ LDA: 각 class에서 X의 분포가 정규 분포임을 가정한다. 이러한 가정이 맞을 경우 LDA의 성능이 더 좋을 수 있지만, 아닌 경우 가정이 없는 Logit이 더 좋을 수 있다.

  #### KNN

  KNN은 특정한 모수가 없는 Bayes Classifier의 비모수 추정이다. 이 때 주변의 몇 개의 점을 볼 것인가에 따라 굉장히 유연할 수도 경직적일 수도 있다.(많이 볼수록 경직적)

  #### QDA

  QDA는 모수의 개수를 추가함으로써 LDA, Logit과 KNN의 사이에 있다고 할 수 있다.  
  실제 Decision Boundary가 비선형인 경우, 적은 수의 표본으로도 QDA가 좋은 성능을 낼 수 있다.
  <br>

   $\rightarrow$ 결론: 어떤 상황인지를 보고 그 상황에 알맞은 방법을 쓰자.

   <img src='comp.png'>


      $\rightarrow$ Linear Decision Boundary인 경우이다. 보면 Logit이나 LDA가 성능이 좋고 QDA는 별로임을 알 수 있다.

      <img src='comp2.png'>

        $\rightarrow$ Non-Linear의 경우 Logit이나 LDA는 성능이 별로고 QDA가 좋음을 알 수 있다.
