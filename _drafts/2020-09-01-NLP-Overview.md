---
title: '[NLP] 개요'

categories:
  - Natural Language Processing
tags:
  - Natural Language Processing

last_modified_at: 2020-09-01T08:06:00-05:00
---

자연어(Nautral Language)란 인간이 일상에서 사용하는 언어를 말합니다. 자연어 처리(Natural Laguage Processing)란 기계가 자연어를 이해하고 해석하여 처리할 수 있도록 하는 일을 말합니다. 자연어 처리(Natural Laguage Processing)는 줄여서 NLP라고 부릅니다.

자연어 처리(NLP)와 텍스트 분석(Text Mining)은 엄밀히 말하면 다른 개념입니다. 자연어 처리는 기계가 인간의 언어를 해석하는데 중점이 두어져 있다면, 텍스트 분석은 텍스트에서 의미 있는 정보를 추출하여 인사이트를 얻는데 더 중점이 두어져 있습니다. 다만, 머신러닝이 보편화되면서 자연어 처리와 텍스트 분석을 구분하는 것이 큰 의미가 없어졌습니다.

## NLP가 활용되는 분야 (Tasks)

- **Text Classification ; 텍스트 분류**
  - Supervised Learning ; 지도학습
  - 텍스트가 특정 분류, 카테고리에 속하는 것을 예측하는 기법을 통칭합니다. 스팸 메일 분류나 뉴스 기사의 내용을 기반으로 연애/정치/사회/문화 중 어떤 카테고리에 속하는지 자동으로 분류해주는 프로그램이 이에 속합니다.
  - Sentiment Analysis ; 감성 분석
  - 텍스트에 나타나는 감정/기분 등의 주관적 요소를 분석하는 기법을 통칭합니다. SNS의 글을 분석하여 글쓴이의 감정을 분석하는 것, 영화 및 제품의 리뷰를 분석하는 것 등이 이에 속합니다. 지도학습 뿐만 아니라 비지도학습을 이용할 수도 있습니다.
- **Text Summarization ; 텍스트 요약**
  - 텍스트에서 중요한 주제를 추출하여 요약하는 기법을 의미합니다.
  - 토픽 모델링(Topic Modeling)이 이에 속합니다.
- **Text Clustering ; 텍스트 군집화**
  - 비슷한 유형의 텍스트에 대해 군집화하는 기법을 뜻합니다.
- **Machine Translation ; 기계 번역**
  - 구글 번역기나 파파고와 같은 번역기에도 활용됩니다.
- **Machine Reading Comprehension ; 기계 독해**
- **Machine Reasoning ; 기계 추론**
- **Question Answering (QA) ; 질의응답 시스템**
  - 대화 시스템 및 자동 질의 응답 시스템
  - 애플의 시리나 삼성 갤럭시의 빅스비, 챗봇 등이 이에 속합니다.

## NLP Project Process

앞으로 자세히 다루겠지만 간단하게 NLP의 프로세스에 대해 알아보겠습니다.

1. Text Preprocessing ; 텍스트 전처리
   - 대/소문자 변경, 특수문자 삭제, 이모티콘 삭제 등의 전처리 작업, 단어(Word) 토큰화 작업, 불용어(Stop word) 제거 작업, 어근 추출(Stemming/Lemmatization) 등의 텍스트 정규화 작업을 수행하는 것이 텍스틑 전처리 단계에 속합니다.
2. Feature Vectorization / Word Representation
   - 피처 벡터화 (Feature Vectorization)
   - 전처리된 텍스트에서 피처를 추출하고 여기에 벡터 값을 할당합니다. 이에 대해서는 추후 자세히 알아보겠습니다. 대표적인 피처 벡터화 기법은 BOW(Bag of words)와 Word2Vec이 있습니다.
3. Modeling
   - 피처 벡터화된 데이터에 대하여 모델을 수립하고 학습/예측을 하는 단계입니다.

## 3. NLP를 위한 라이브러리

사이킷런은 머신러닝을 위한 라이브러리여서 NLP를 위한 다양한 라이브러리를 제공하지는 않습니다. 물론 사이킷런에서도 텍스트 가공을 위한 기본적인 라이브러리는 제공하지만 더 다양한 텍스트 분석이 적용돼야 하는 경우, 아래와 같은 3개의 라이브러리를 주로 사용합니다.

- `NLTK(National Language Toolkit for Python)` : 파이썬 NLP 패키지의 시조새에 해당합니다. 가장 기본적인 패키지이며, 오랫동안 연구되었습니다. 하지만 최근에서 수행 속도 측면에서 다소 느려 제대로 활용되고 있지는 않습니다. 다만, 초기에 NLP를 공부하는데에는 좋은 라이브러리라고 볼 수 있습니다.
- `Gensim` : 토픽 모델링 분야에서 가장 두각을 보이는 NLP 패키지입니다.
- `SpaCy` : 수행 성능이 좋아 최근 가장 많이 활용되고 있는 NLP 패키지입니다.
