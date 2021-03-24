---
title: '[텍스트 전처리] 불용어 제거 (Stopwords Removal)'

categories:
  - Data Analysis
tags:
  - Text Preprocessing

last_modified_at: 2020-08-26T08:06:00-05:00

classes: wide
---

이 글은 불용어를 제거하는 방법에 관한 기록입니다.

## 1. Zipf's Law (지프의 법칙)

Zipf’s law는 어떠한 자연어 말뭉치 표현에 나타나는 단어들을 그 사용 빈도가 높은 순서대로 나열하였을 때, 모든 단어의 사용 빈도는 해당 단어의 순위에 반비례함을 나타냅니다. 다시 말해, 가장 사용 빈도가 높은 단어는 두번째 단어보다 빈도가 약 두배 높으며, 세번째 단어보다는 빈도가 약 세배 높다는 것입니다. Zipf’s law에 따르면, 문헌집단에 나타나는 단어들의 빈도수를 시각화 하면 long tail distribution이 나타나고, 이때 highly rank된 단어들 즉 고빈도 단어들 중에는 전치사가 대부분입니다.

이러한 단어의 frequency와 단어의 rank의 곱이 constant 즉 일정하다는 Zipf’s law에 기반하여 Luhn은 단어의 출현 빈도 및 순위와 단어의 문헌 식별력 간의 관계를 밝혀냈습니다. 이에 따르면, 최고 한계 빈도와 최저 한계 빈도 안에 속하는 중간 빈도의 단어들이 문헌 내용의 식별력이 크므로 이들을 색인어로 선정하는 것이 바람직합니다. 고빈도 단어들은 기능적인 역할을 하거나 문헌집단 전반에 걸쳐 나타나기 때문에 특정 문헌의 내용을 대표할 수 없고, 저빈도 단어들은 특정 문헌의 내용을 나타낸다고 보기에 너무 적은 빈도 수를 갖기 때문입니다.
 
## 2. Stopwords Removal (불용어 제거)

Zipf’s law에서 왼쪽에 존재하는 고빈도 단어들을 stopwords라고 합니다. 이러한 stopwords를 모아놓은 것을 stopwords list라고 하고, 이를 활용하여 텍스트 전처리 과정에서 stopwords removal을 하게 됩니다. 영어의 경우 정관사, 전치사 등이 stopwords에 속하고, 한글의 경우 조사 등이 stopwords에 포함됩니다. 불용어 리스트는 정보검색 분야에서 많이 사용된다. 불용어 리스트는 한 라인이 한 단어로 이루어집니다. 영어의 경우 450-500개 정도의 불용어 리스트가 있고, 한국어도 마찬가지로 불용어 리스트를 사용합니다.
 
Stopwords Removal의 목적은 크게 두가지입니다. 단어 정제를 통해 보다 제대로 된 분석을 하기 위함이기도 하고, DTM의 dimension reduction을 통해 분석을 가능하게 하기 위함이기도 합니다. 불용어 리스트를 사용한다는 것은 DTM의 컬럼 수를 줄여주는 것 즉 dimension을 줄여주는 것을 의미합니다. 이를 dimension reduction이라고 합니다. 불용어 리스트를 사용하여 feature들 즉 단어들의 수를 줄여주게 되는데, 그러면 matrix size가 줄어들게 됩니다. 이렇게 불용어 리스트를 사용해서 불용어를 제거하는 것은 dimension reduction에 있어 주요한 역할을 합니다.

## 3. NLTK에서 정의한 불용어 리스트 사용하기 (영어)

```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

example = "Family is not an important thing. It's everything."

# 불용어 리스트 생성
stop_words = set(stopwords.words('english')) 

# 단어 토큰화 실시
word_tokens = word_tokenize(example)

# 단어 토큰화 결과로부터 불용어 제거 실시
result = []
for w in word_tokens: 
    if w not in stop_words: 
        result.append(w) 

# 결과 출력
print(word_tokens) 
print(result) 
```

    ['Family', 'is', 'not', 'an', 'important', 'thing', '.', 'It', "'s", 'everything', '.']
    ['Family', 'important', 'thing', '.', 'It', "'s", 'everything', '.']

NLTK에서 제공하는 불용어 리스트를 활용하여 불용어를 제거한 결과 'is', 'not', 'an'과 같은 단어들이 제거된 것을 볼 수 있습니다.

## 4. 사용자 정의 불용어 리스트 사용하기

실제로 불용어 제거를 할 때에는 다양한 패키지에서 제공하는 불용어 리스트 뿐만 아니라 해당 코퍼스에 특화되어 있는 사용자 정의 불용어 리스트 또한 사용하는 경우가 많습니다. 일반적으로 불용어 리스트는 txt 파일 혹은 csv 파일 형태로 저장하는데, 여기에는 각각의 불용어가 줄바꿈을 기준으로 한줄에 하나씩 적혀있습니다. 하지만 이번에는 편의상 임의로 불용어 리스트를 생성하여 실습을 진행하도록 하겠습니다.

```python
from nltk.corpus import stopwords 
from nltk.tokenize import word_tokenize

example = "고기를 아무렇게나 구우려고 하면 안 돼. 고기라고 다 같은 게 아니거든. 예컨대 삼겹살을 구울 때는 중요한 게 있지."

# 불용어 리스트 생성
stop_words = "아무거나 아무렇게나 어찌하든지 같다 비슷하다 예컨대 이럴정도로 하면 아니거든"
stop_words = stop_words.split(' ')

# 단어 토큰화 실시
word_tokens = word_tokenize(example)

# 불용어 제거 실시
result = [word for word in word_tokens if word not in stop_words]

# 결과 출력
print(word_tokens) 
print(result)
```

    ['고기를', '아무렇게나', '구우려고', '하면', '안', '돼', '.', '고기라고', '다', '같은', '게', '아니거든', '.', '예컨대', '삼겹살을', '구울', '때는', '중요한', '게', '있지', '.']
    ['고기를', '구우려고', '안', '돼', '.', '고기라고', '다', '같은', '게', '.', '삼겹살을', '구울', '때는', '중요한', '게', '있지', '.']

