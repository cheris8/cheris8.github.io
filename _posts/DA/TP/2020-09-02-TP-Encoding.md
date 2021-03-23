---
title: '[텍스트 전처리] 정수 인코딩 & 원-핫 인코딩'

categories:
  - Data Analysis
  - Text Preprocessing
tags:
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing

last_modified_at: 2020-09-02T08:06:00-05:00

classes: wide
---

이 글은 정수 인코딩과 원-핫 인코딩에 관한 기록입니다.

## 정수 인코딩 (Integer Encoding)

컴퓨터는 텍스트보다는 숫자를 더 잘 처리합니다. 따라서 자연어 처리에서는 텍스트를 숫자로 바꾸는 여러가지 방법들을 필요로 합니다. 이러한 기법들을 적용하기에 앞서 일반적으로 각 단어를 고유한 정수에 매핑하는 전처리 과정이 요구됩니다. 이를 정수 인코딩(Integer Encdoing)이라고 합니다.

예를 들어 갖고 있는 텍스트에 단어가 5000개가 있다면, 5000개의 단어들 각각에 1번부터 5000번까지 인덱스, 즉 단어와 매핑되는 고유한 정수를 부여합니다. 가령, book은 150번, dog는 171번, love는 192번, books는 212번과 같이 숫자가 부여됩니다. 

단어에 인덱스를 부여하는 방법 중 하나로, 단어를 빈도 수 기준으로 정렬한 단어 집합(vocabulary)을 만들고, 빈도 수가 높은 순서대로 낮은 숫자부터 차례대로 정수를 부여하는 방법이 있습니다.

### 1. dictionary 사용하기

```python
from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
```

```python
text = "A barber is a person. a barber is good person. a barber is huge person. he Knew A Secret! The Secret He Kept is huge secret. Huge secret. His barber kept his word. a barber kept his word. His barber kept his secret. But keeping and keeping such a huge secret to himself was driving the barber crazy. the barber went up a huge mountain."
print(text)
```

    A barber is a person. a barber is good person. a barber is huge person. he Knew A Secret! The Secret He Kept is huge secret. Huge secret. His barber kept his word. a barber kept his word. His barber kept his secret. But keeping and keeping such a huge secret to himself was driving the barber crazy. the barber went up a huge mountain.

이때 `text`는 여러 문장(sentence)으로 구성된 문서(document)임을 알 수 있습니다.

`text`에 대하여 문장 토큰화를 수행합니다.

```python
# 문장 토큰화
text = sent_tokenize(text)
print(text)
```

    ['A barber is a person.', 'a barber is good person.', 'a barber is huge person.', 'he Knew A Secret!', 'The Secret He Kept is huge secret.', 'Huge secret.', 'His barber kept his word.', 'a barber kept his word.', 'His barber kept his secret.', 'But keeping and keeping such a huge secret to himself was driving the barber crazy.', 'the barber went up a huge mountain.']

이제 `text`는 문장 단위로 토큰화 된 문장들을 요소로 가지고 있는 리스트 입니다.

다음으로 `text`에 대하여 단어 토큰화를 수행합니다. 이에 더하여 몇가지 텍스트 전처리 작업을 함께 수행합니다. 모든 문자들을 소문자로 바꾸고, 길이가 짧은 단어들과 불용어는 제거합니다. 또한 단어와 단어 빈도 수를 갖는 단어 집합을 생성합니다.

```python
# 텍스트 전처리 및 단어 토큰화
vocab = {} # 단어(key)와 단어 빈도 수(value)를 갖는 딕셔너리
sentences = [] # 단어 토큰화 이후 텍스트
stop_words = set(stopwords.words('english')) # 불용어

for i in text: # 문장에 대하여 반복
    sentence = word_tokenize(i) # 문장을 단어로 토큰화 하여 sentence 변수에 저장
    result = []
    for word in sentence: # 단어에 대하여 반복
        word = word.lower() # 모두 소문자로 변경
        if word not in stop_words: # 불용어 제거
            if len(word) > 2: # 길이가 짧은 단어 제거
                # result에 word 추가
                result.append(word)
                # 빈도 초기값 생성
                if word not in vocab:
                    vocab[word] = 0
                # vocab에 빈도 추가
                vocab[word] += 1
    
    sentences.append(result)
```

```python
print(sentences)
```

    [['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]

`senteces`는 단어 단위로 토큰화 된 단어들을 요소로 갖는 리스트를 요소로 가지고 있는 이중 리스트 입니다.

```python
print(vocab)
```

    {'barber': 8, 'person': 3, 'good': 1, 'huge': 5, 'knew': 1, 'secret': 6, 'kept': 4, 'word': 2, 'keeping': 2, 'driving': 1, 'crazy': 1, 'went': 1, 'mountain': 1}

`vocab`은 단어를 키(key)로, 단어의 빈도 수를 값(value)로 갖는 딕셔너리입니다. 위 `sentences`와는 달리 단어가 중복되지 않는 것을 볼 수 있습니다.

단어 집합 `vocab`을 빈도 수가 높은 순으로 정렬합니다.

```python
# 빈도 수가 높은 순서대로 정렬
vocab_sorted = sorted(vocab.items(), 
                      key=lambda x: x[1], # vocab의 value인 빈도 수를 key로 할당
                      reverse=True)
```

```python
print(vocab_sorted)
```

    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3), ('word', 2), ('keeping', 2), ('good', 1), ('knew', 1), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)]

이제 높은 빈도수를 가진 단어일수록 낮은 인덱스를 부여합니다.

```python
# 단어 빈도 수가 높은 순서대로 차례로 낮은 인덱스를 부여
word_to_index = {}
i = 0
for word, frequency in vocab_sorted:
    if frequency > 1: # 빈도 수가 1인 단어 제거
        i += 1
        word_to_index[word] = i
```

```python
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7}

`word_to_index`는 단어를 key로, 인덱스를 value로 갖는 딕셔너리 입니다. 이때 작은 값의 인덱스를 갖는 단어일수록 높은 빈도 수를 가집니다.

자연어 처리에서는 보통 전처리 과정에서 출현 빈도 수가 낮은 단어들을 분석 대상에서 제외하고자 제거합니다. 출현 빈도 수가 낮은 단어는 자연어 처리에서 유의미하지 않을 가능성이 높기 때문입니다. 

또한 자연어 처리에서는 코퍼스에 존재하는 모든 단어들을 사용하기보다는 빈도 수가 가장 높은 n개의 단어들만을 사용하는 경우가 더 많습니다. 

```python
# 빈도 수가 가장 높은 5개의 단어들만을 사용
vocab_size = 5
words_frequency = [w for w, c in word_to_index.items() if c >= vocab_size+1] # 인덱스가 5 초과인 단어들
for w in words_frequency:
    del word_to_index[w] # 인덱스가 5 초과인 단어 제거
```

```python
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}

이제 `word_to_index`는 빈도 수가 높은 상위 5개의 단어들만을 가지고 있는 딕셔너리 입니다. 

`word_to_index`를 사용하여 `sentences`에 있는 각 단어들을 인덱스로 매핑하는 작업 즉 정수 인코딩을 진행하고자 합니다. 예를 들어, `sentences`의 첫번째 요소 \['barber', 'person'\]는 \[1, 5\]로 인코딩 합니다. 그런데 두번째 요소 \['barber', 'good', 'person'\]에는 더 이상 `word_to_index`에 존재하지 않는 'good'이라는 단어가 존재합니다. 이렇게 단어 집합에 존재하지 않는 단어들을 Out-Of-Vocabulary(OOV)라고 합니다. 따라서 `word_to_index`에 'OOV'라는 단어를 새롭게 추가하고 단어 집합에 없는 단어들은 'OOV'의 인덱스로 인코딩 하고자 합니다.

```python
# OOV에 인덱스 부여
word_to_index['OOV'] = len(word_to_index)+1
```

이제 `word_to_index`를 이용하여 `sentences`의 모든 단어들을 고유 정수로 매핑하는, 즉 정수 인코딩을 수행합니다.

```python
# OOV에 인덱스 부여
word_to_index['OOV'] = len(word_to_index)+1
```

```python
# sentences의 모든 단어들을 고유 정수에 매핑 => 정수 인코딩
encoded = [] # 정수 인코딩 한 sentences를 담을 리스트
for s in sentences: # 문장에 대하여 반복
    temp = []
    for w in s: # 단어에 대하여 반복
        try: # 단어 집합에 존재하는 단어인 경우
            temp.append(word_to_index[w]) # temp에 해당 단어의 인덱스 추가
        except KeyError: # 단어 집합에 존재하지 않는 단어인 경우
            temp.append(word_to_index['OOV']) # temp에 OOV의 인덱스 추가
    encoded.append(temp) # encoded에 정수 인코딩 한 리스트 추가
```

```python
print(encoded)
```

    [[1, 5], [1, 6, 5], [1, 3, 5], [6, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [6, 6, 3, 2, 6, 1, 6], [1, 6, 3, 6]]

앞서 `text`에 할당했던 영어 문장이 정수 인코딩을 통해 위와 같이 변환된 것을 확인할 수 있습니다.

### 2. `Counter` 사용하기

```python
from collections import Counter
```

앞서 사용했던 `senteces`를 사용하고자 합니다. `senteces`는 단어 단위로 토큰화 된 단어들을 요소로 갖는 리스트를 요소로 가지고 있는 이중 리스트 입니다.

```python
print(sentences)
```

    [['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]

먼저 이중 리스트인 `senteces`를 리스트로 변환하여 `words`에 할당합니다.

```python
# 이중 리스트를 리스트로 변환
words = sum(sentences, [])
#words = np.hstack(sentences)
print(words)
```

    ['barber', 'person', 'barber', 'good', 'person', 'barber', 'huge', 'person', 'knew', 'secret', 'secret', 'kept', 'huge', 'secret', 'huge', 'secret', 'barber', 'kept', 'word', 'barber', 'kept', 'word', 'barber', 'kept', 'secret', 'keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy', 'barber', 'went', 'huge', 'mountain']

`Counter()`는 단어의 중복을 제거하고 단어의 빈도 수를 계산하여 단어를 key로, 단어 빈도 수를 value로 갖는 딕셔너리를 반환합니다. 이때 `Counter()`의 입력으로 이중 리스트가 아닌 리스트를 할당해야 한다는 점에 주의해야 합니다.

```python
vocab = Counter(words)
print(vocab)
```

    Counter({'barber': 8, 'secret': 6, 'huge': 5, 'kept': 4, 'person': 3, 'word': 2, 'keeping': 2, 'good': 1, 'knew': 1, 'driving': 1, 'crazy': 1, 'went': 1, 'mountain': 1})

`most_common()`은 빈도 수가 높은 상위 n개 단어만을 갖는 딕셔너리를 반환합니다.

```python
vocab_size = 5
vocab = vocab.most_common(vocab_size) # 빈도 수가 높은 상위 5개 단어만을 저장
print(vocab)
```

    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]

이제 높은 빈도수를 가진 단어일수록 낮은 인덱스를 부여합니다.

```python
word_to_index = {}
i = 0
for (word, frequency) in vocab:
    i = i+1
    word_to_index[word] = i
```

```python
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}

이제 `word_to_index`를 이용하여 `sentences`의 모든 단어들을 고유 정수로 매핑하는, 즉 정수 인코딩을 수행합니다.

```python
# OOV에 인덱스 부여
word_to_index['OOV'] = len(word_to_index)+1
```

```python
# sentences의 모든 단어들을 고유 정수에 매핑 => 정수 인코딩
encoded = [] # 정수 인코딩 한 sentences를 담을 리스트
for s in sentences: # 문장에 대하여 반복
    temp = []
    for w in s: # 단어에 대하여 반복
        try: # 단어 집합에 존재하는 단어인 경우
            temp.append(word_to_index[w]) # temp에 해당 단어의 인덱스 추가
        except KeyError: # 단어 집합에 존재하지 않는 단어인 경우
            temp.append(word_to_index['OOV']) # temp에 OOV의 인덱스 추가
    encoded.append(temp) # encoded에 정수 인코딩 한 리스트 추가
```

```python
print(encoded)
```

    [[1, 5], [1, 6, 5], [1, 3, 5], [6, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [6, 6, 3, 2, 6, 1, 6], [1, 6, 3, 6]]

### 1.3 NLTK의 `FreqDist` 사용하기

NLTK에서는 빈도수 계산 도구인 `FreqDist()`를 지원합니다. 위에서 사용한 `Counter()`와 같은 방식으로 사용할 수 있습니다.

```python
from nltk import FreqDist
import numpy as np
```

`FreqDist()` 또한 `Counter()`와 마찬가지로 입력으로 이중 리스트가 아닌 리스트를 할당해야 합니다.

```python
# 이중 리스트를 리스트로 변환하여 (문장 구분을 제거하여) 입력
vocab = FreqDist(np.hstack(sentences))
```

```python
print(vocab)
```

    <FreqDist with 13 samples and 36 outcomes>

```python
print(vocab["barber"]) # 단어 'barber'의 빈도 수 출력
```

    8

`most_common()`은 빈도 수가 높은 상위 n개 단어만을 갖는 딕셔너리를 반환합니다.

```python
vocab_size = 5
vocab = vocab.most_common(vocab_size) # 빈도 수가 높은 상위 5개 단어만을 저장
print(vocab)
```

    [('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]

이제 높은 빈도 수를 갖는 단어일수록 낮은 인덱스를 부여합니다. 이때 `enumerate()`를 사용하여 좀 더 손쉽게 각 단어에 고유 정수를 매핑할 수 있습니다.

```python
word_to_index = {word[0] : index+1 for index, word in enumerate(vocab)}
print(word_to_index)
```

    {'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}

이제 `word_to_index`를 이용하여 `sentences`의 모든 단어들을 고유 정수로 매핑하는, 즉 정수 인코딩을 수행합니다.

```python
# OOV에 인덱스 부여
word_to_index['OOV'] = len(word_to_index)+1
```

```python
# sentences의 모든 단어들을 고유 정수에 매핑 => 정수 인코딩
encoded = [] # 정수 인코딩 한 sentences를 담을 리스트
for s in sentences: # 문장에 대하여 반복
    temp = []
    for w in s: # 단어에 대하여 반복
        try: # 단어 집합에 존재하는 단어인 경우
            temp.append(word_to_index[w]) # temp에 해당 단어의 인덱스 추가
        except KeyError: # 단어 집합에 존재하지 않는 단어인 경우
            temp.append(word_to_index['OOV']) # temp에 OOV의 인덱스 추가
    encoded.append(temp) # encoded에 정수 인코딩 한 리스트 추가
```

```python
print(encoded)
```

    [[1, 5], [1, 6, 5], [1, 3, 5], [6, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [6, 6, 3, 2, 6, 1, 6], [1, 6, 3, 6]]

## 원-핫 인코딩 (One-Hot Encoding)

### 1. 단어 집합 (Vocabulary)

단어 집합(Vocabulary)은 서로 다른 단어들의 집합입니다. 즉, 단어 집합은 코퍼스에 나타난 모든 단어들이 중복 없이 존재하는 집합이라고 할 수 있습니다. 단어 집합에서는 기본적으로 book과 books와 같은 단어의 변형 형태도 다른 단어로 간주합니다.

### 2. 원-핫 인코딩의 개념

컴퓨터는 텍스트보다는 숫자를 더 잘 처리 할 수 있습니다. 따라서 자연어 처리에서는 텍스트를 숫자로 바꾸는 여러가지 방법들을 필요로 합니다. 이러한 기법들 중 가장 기본적인 방법이 원-핫 인코딩입니다.

원-핫 인코딩(One-Hot Encoding)은 단어 집합의 크기를 벡터의 차원으로 하고, 표현하고 싶은 단어의 인덱스에 1의 값을 부여하고, 나머지 단어들의 인덱스에는 0을 부여하는 단어의 벡터 표현 방식입니다. 원-핫 인코딩을 통해 표현한 벡터를 원-핫 벡터(One-Hot vector)라고 합니다.

원-핫 인코딩의 과정은 다음과 같습니다.
1. 코퍼스에 대하여 단어 집합을 생성합니다.
2. 단어 집합에 존재하는 각 단어에 고유한 정수 즉 인덱스를 부여합니다. (정수 인코딩)
3. 표현하고 싶은 단어의 인덱스 위치에는 1을 값으로 부여하고, 나머지 단어들의 인덱스 위치에는 0을 값으로 부여합니다.

원-핫 인코딩을 통해 아래의 문장을 원-핫 벡터로 표현하는 코드를 살펴보겠습니다.

문장 : 나는 자연어 처리를 배운다

```python
from konlpy.tag import Okt
```

형태소 분석기를 활용하여 단어 토큰화를 수행함으로써 단어 집합을 생성합니다.

```python
okt = Okt() # 형태소 분석기 선언
token = okt.morphs('나는 자연어 처리를 배운다') # 단어 토큰화
print(token)
```

    ['나', '는', '자연어', '처리', '를', '배운다']

다음으로 단어 집합에 존재하는 각 단어에 고유한 정수 즉 인덱스를 부여합니다.

```python
word2index = {}
for voca in token:
    if voca not in word2index.keys():
        word2index[voca] = len(word2index)
```

```python
print(word2index)
```

    {'나': 0, '는': 1, '자연어': 2, '처리': 3, '를': 4, '배운다': 5}

단어와 정수 인코딩 된 단어 집합을 입력으로 넣으면, 해당 단어의 인덱스 위치에는 1을 값으로 부여하고, 나머지 단어들의 인덱스 위치에는 0을 값으로 부여하여 원-핫 벡터를 반환하는 함수를 선언합니다.

```python
# 토큰을 입력하면 해당 토큰에 대한 원-핫 벡터를 반환하는 함수
def one_hot_encoding(word, word2index):
    one_hot_vector = [0]*(len(word2index)) # 단어 집합의 크기가 벡터의 차원, 모든 값을 0으로 초기화
    index = word2index[word] # 표현하고 싶은 단어의 인덱스
    one_hot_vector[index] = 1 # 표현하고 싶은 단어의 인덱스 위치에 1을 값으로 부여
    return one_hot_vector # 원-핫 벡터를 반환
```

```python
one_hot_encoding('자연어', word2index)
```

    [0, 0, 1, 0, 0, 0]

```python
one_hot_encoding('배운다', word2index)
```

    [0, 0, 0, 0, 0, 1]

### 3. 원-핫 인코딩의 한계

원-핫 인코딩에서 원-핫 벡터의 차원은 단어 집합의 크기입니다. 따라서 단어 집합의 단어의 개수가 늘어날수록, 벡터의 차원 또한 늘어납니다. 즉 벡터를 표현하기 위해 필요한 공간이 계속 늘어난다는 것입니다. 예를 들어, 단어 집합에 1000개의 단어가 있다고 가정해봅시다. 원-핫 인코딩을 통해 원-핫 벡터를 만들면 각각의 벡터는 1000개의 차원을 갖게 됩니다. 다시 말해 각각의 모든 단어는 1개의 1값과 999개의 0값을 갖는 벡터가 되는데, 이는 저장 공간 측면에서는 매우 비효율적이라고 볼 수 있습니다.

또한 원-핫 인코딩을 통해 만들어낸 원-핫 벡터는 단어의 유사도를 표현하지 못한다는 단점이 있습니다. 예를 들어 강아지, 개, 냉장고 3개의 단어가 있다고 가정해봅시다. 이 단어들을 원-핫 인코딩을 통해 \[1, 0, 0\], \[0, 1, 0\], \[0, 0, 1\], \[0, 0, 0\] 3개의 원-핫 벡터로 바꾸었습니다. 이때 원-핫 벡터로는 강아지라는 단어가 개와 냉장고라는 단어 중 어떤 단어와 더 유사한지를 알 수 없습니다.

이렇게 단어 간 유사성을 알 수 없다는 점은 검색 시스템 등에서 치명적인 단점으로 작용합니다. 예를 들어, 검색창에 '삿포로 숙소'라는 단어를 검색한다고 합시다. 잘 만들어진 검색 시스템이라면, '삿포로 숙소'라는 검색어에 대해서 '삿포로 게스트 하우스', '삿포로 료칸', '삿포로 호텔'과 같은 유사 단어에 대한 결과도 함께 보여줄 수 있어야 합니다. 하지만 단어 간 유사성을 계산할 수 없다면, '게스트 하우스'와 '료칸'과 '호텔'이라는 연관 검색어를 보여줄 수 없습니다.

이러한 단점을 해결하고자 단어의 잠재 의미를 반영하여 다차원 공간에 단어를 벡터화 하는 방법들이 존재합니다.
1. 카운트 기반 벡터화 방법 : LSA, HAL 등
2. 예측 기반 벡터화 방법 : NNLM, RNNLM, Word2Vec, FastText 등
3. 카운트 기반 & 예측 기반 방법 : GloVe

