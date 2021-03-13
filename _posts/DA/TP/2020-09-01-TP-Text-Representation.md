---
title: '[텍스트 전처리] 텍스트 표현 (Text Representation)'

categories:
  - Data Analysis
tags:
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing

last_modified_at: 2020-09-01T08:06:00-05:00

classes: wide
---

자연어 처리에는 텍스트를 표현하는 여러가지 방법들이 있습니다. 특히 컴퓨터가 이해할 수 있도록 문자를 숫자로 바꾸는 방법들이 존재합니다. 이러한 단어 표현 방법들에 대해서 알아보고자 합니다.

## 1. 단어 표현 (Word Representation) 방법

단어 표현 방법에는 크게 **국소 표현(Local Representation)** 방법과 **분산 표현(Distributed Representation)** 방법이 있습니다. 

**국소 표현(Local Representation)** 방법은 어떤 단어를 표현하고자 할 때 해당 단어 그 자체만을 보고 단어를 표현하는 방법입니다. 해당 단어에 특정 값을 매핑합니다. 국소 표현 방법은 **이산 표현(Discrete Representation)** 이라고도 합니다. **분산 표현(Distributed Representation)** 방법은 어떤 단어를 표현하고자 할 때 해당 단어의 주변을 참고하여 단어를 표현하는 방법입니다. 분산 표현 방법은 **연속 표현(Continuous Represnetation)** 이라고도 합니다. **국소 표현 방법은 단어의 의미나 뉘앙스를 표현할 수 없지만, 분산 표현 방법은 이를 표현할 수 있다는 점에서 차이가 있습니다.**

예를 들어, puppy, cute, lovely라는 세개의 단어가 있다고 가정해봅시다. 만약 각 단어에 1번, 2번, 3번 등과 같이 숫자를 매핑하여 값으로 부여한다면,  이는 국소 표현 방법에 해당됩니다. 만약 puppy라는 단어 근처에는 주로 cute, lovely이라는 단어가 자주 등장하므로 puppy라는 단어는 cute, lovely한 느낌이다 로 단어를 표현한다면, 이는 분산 표현 방법에 해당됩니다.

## 2. 단어 표현 (Word Representation) 방법 분류

![]({{site.url}}/assets/images/DA/TP/wordrepresentation.PNG)

**Local Representation**

- [One-Hot Vector]({{site.url}}/data%20analysis/TP-Encoding/) : Local Representation
- N-gram : Local Representation
​

- BoW : Local Representation, Count based
- DTM : Local Representation, Count based


**Continuous Representation**
​
- LSA : Continuous Representation, Count based

- Embedding Vector : Continuous Representation
https://cheris8.github.io/
    - [Word2Vec]({{site.url}}/​deep%20learning/NLP-WordEmbedding-Word2Vec/) : Continuous Representation, Prediction based
    - FastText : Continuous Representation, Prediction based
    - GloVe : Continuous Representation, Count based + Prediction based

## 3. 참고자료

[딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/book/2155)

