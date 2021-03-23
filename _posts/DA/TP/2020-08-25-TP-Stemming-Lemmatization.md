---
title: '[텍스트 전처리] 어간 추출 (Stemming) & 원형 복원 (Lemmatization)'

categories:
  - Data Analysis
  - Text Preprocessing
tags:
  - Data Analysis
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing
  - Python

last_modified_at: 2020-08-25T08:06:00-05:00

classes: wide
---

단어의 형태 변화(lexical variations of term ; term variation)에 따라 같은 단어라도 다른 단어인 것처럼 취급되는 문제를 해결하기 위해 사용되는 보편적인 방법으로 어간 추출(Stemming)과 원형 복원(Lemmatization)이 있습니다.

## 1. Stemming (어간 추출)

Stemming이란 어형이 변형된 단어로부터 접사 등을 제거하고 그 단어의 어간을 분리해내는 것을 의미합니다. 이때 어간이 반드시 어근과 같아야 하는 것은 아니며, Stemming의 목적은 어근과 차이가 있더라도 관련이 있는 단어들이 일정하게 동일한 어간으로 매핑되게 하는 것입니다. 이러한 역할을 하는 것을 Stemming Algorithm 또는 Stemmer라고 합니다.

stemming은 정보검색 분야에서 처음 등장했습니다. 정보검색시스템의 색인 과정에서, 단어의 prefix에 variation이 많은 경우 질의어와 색인어 간에 매칭이 잘 이루어지지 않아 질의어도 stemming을 하고 색인어도 stemming을 하여 검색 성능을 높이고자 하였습니다. 많은 웹 검색엔진들은 동일한 어간을 가진 단어들을 동의어로 취급하는 방식으로 질의어 확장을 하여 검색 결과의 품질을 높입니다.

예를 들어, 'automate', 'automatic', 'automation' 이렇게 세개의 단어가 있다고 가정해봅시다. 이들은 각각 'e', 'ic', 'ion'이라는 접사를 가지고 있어 어형이 변형되었지만 모두 'automat'라는 어간을 가지고 있습니다. 이러한 단어들에 대하여 접사를 제거하고 동일한 어간인 'automat'으로 매핑되도록 하는 작업이 stemming입니다.

대표적인 Stemming Algorithm으로 Martin Porter가 고안한 Porter Stemming Algorithm = Porter Stemmer 가 있습니다. rule base로, 많은 규칙들에 기반하여 어미를 분리 및 제거함으로써 어간을 추출해줍니다. Porter Stemming Algorithm은 aggressive하지 않다는 점에서 편리합니다. aggressive한 stemming algorithm의 경우에는 단어로부터 더 많은 부분을 제거하여 더 적은 부분만을 어간으로 남깁니다. 그 결과 term variation 때문에 다른 단어인 것처럼 취급된 것이 아니라 완전히 다른 단어임에도 불구하고 전혀 다른 단어들이 하나의 어간으로 처리되는 경우가 발생합니다. 이러한 점을 방지하는 데에 Porter Stemming Algorithm이 가장 효과적입니다.

stemming은 DTM의 dimension reduction에 효과적이라는 장점을 갖습니다. Zipf's law에 따르면, 단어들의 출현빈도는 long tail이기 때문에 문서집단 내에는 극저빈도의 단어들이 많습니다. 이러한 단어들의 term variation을 잡아줌으로써 하나의 단어로 통일하여 의미를 보존한 채 DTM의 차원을 축소할 수 있습니다. 힌편 앞서 언급한 것과 같이, stemming은 뜻을 구분할 수 없을 정도로 어간이 짧게 잡히는 경우가 발생할 수 있다는 단점을 갖습니다. 또한 stemming을 할 경우 품사 정보를 알 수 없어 문제가 되는 경우도 존재합니다.

```python
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer

text = "This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes."

# word tokenization
words = word_tokenize(text)

# stemming
s = PorterStemmer()
result = [s.stem(w) for w in words]

# 결과 출력
print(text)
print(words)
print(result)
```

    This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes.
    ['This', 'was', 'not', 'the', 'map', 'we', 'found', 'in', 'Billy', 'Bones', "'s", 'chest', ',', 'but', 'an', 'accurate', 'copy', ',', 'complete', 'in', 'all', 'things', '--', 'names', 'and', 'heights', 'and', 'soundings', '--', 'with', 'the', 'single', 'exception', 'of', 'the', 'red', 'crosses', 'and', 'the', 'written', 'notes', '.']
    ['thi', 'wa', 'not', 'the', 'map', 'we', 'found', 'in', 'billi', 'bone', "'s", 'chest', ',', 'but', 'an', 'accur', 'copi', ',', 'complet', 'in', 'all', 'thing', '--', 'name', 'and', 'height', 'and', 'sound', '--', 'with', 'the', 'singl', 'except', 'of', 'the', 'red', 'cross', 'and', 'the', 'written', 'note', '.']

```python
from nltk.stem import PorterStemmer
from nltk.stem import LancasterStemmer

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']

# stemming
s = PorterStemmer() # 포터 스태머
l = LancasterStemmer() # 랭커스터 스태머
ss = [s.stem(w) for w in words]
ll = [l.stem(w) for w in words]

# 결과 출력
print(words)
print(ss)
print(ll)
```

    ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
    ['polici', 'do', 'organ', 'have', 'go', 'love', 'live', 'fli', 'die', 'watch', 'ha', 'start']
    ['policy', 'doing', 'org', 'hav', 'going', 'lov', 'liv', 'fly', 'die', 'watch', 'has', 'start']

## 2. Lemmatization (표제어 추출 ; 원형 복원)

Lemmatization은 한 단어가 여러 형식으로 표현되어 있는 것을 단일 형식으로 묶어주는 기법입니다. 예를 들어, 'am', 'are', 'is' 세개의 단어에 대하여 lemmatization을 실시할 경우 그 결과는 'be'가 됩니다.

단어의 의미적 단위를 고려하지 않고 기계적으로 경험적 법칙에 의해 결정되는 stemming과 달리, Lemmatization은 단어의 의미적인 단위를 고려하고 자연어 처리 기법의 근간이 되는 형태소 분석을 통해 이루어지기 때문에 더 정확한 단어 수준 분석을 수행할 수 있습니다. 즉 Lemmatization을 수행할 경우, 품사 정보가 남아있기 때문에 의미론적 관점에서 더 효과적입니다. 물론 lemmatization은 stemming만큼 DTM dimension reduction 측면에서 효과적이진 않습니다. 왜냐하면 품사 정보를 여전히 가지고 있어 stemming에 비해 더 적은 단어들이 하나의 단어로 통일되기 때문입니다.

```python
from nltk.stem import WordNetLemmatizer

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']

# lemmatization
n = WordNetLemmatizer()
result = [n.lemmatize(w) for w in words]

# 결과 출력
print(words)
print(result)
```

    ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
    ['policy', 'doing', 'organization', 'have', 'going', 'love', 'life', 'fly', 'dy', 'watched', 'ha', 'starting']

```python
# 단어의 품사 정보를 알려주어 다시 출력
print(n.lemmatize('dies', 'v'))
print(n.lemmatize('watched', 'v'))
print(n.lemmatize('has', 'v'))
```

    die
    watch
    have

## 3. Stemming vs. Lemmatization

|비교|Stemming|Lemmatization|
|-|--------|-------------|
|의미|어간 추출|원형 복원|
|접근 방법|정보검색적|언어학적|
|DTM dimension reduction 관점|good|bad|
|의미론적 관점|bad (품사 X)|good (품사 O)|

영어 텍스트의 경우에는 stemming과 lemmatization이 명확하게 구분되어 텍스트 전처리 과정에서 무엇을 사용할지를 결정해야 합니다. 반면 한글 텍스트의 경우에는 형태소 분석 과정에서 stemming과 lemmatization이 함께 이루어진다고 볼 수 있습니다.

