---
title: '[텍스트 전처리] 단어 토큰화 (Word Tokenization)'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing
  - Python

last_modified_at: 2020-08-22T08:06:00-05:00

classes: wide
---

이 글은 단어 토큰화(Word Tokenization)의 개념 및 코드에 관한 기록입니다.

## 1. 개념

단어 토큰화는 토큰의 단위를 단어로 하여, 코퍼스 내 텍스트를 단어 단위로 구분하는 작업을 의미합니다. 이때 단어는 single word 이외에도 단어구 등 의미를 갖는 단위를 포괄합니다. 영어의 경우 텍스트를 단어 단위로 구분할 때 보통 띄어쓰기 즉 공백(whitespace)을 기준으로 합니다. 예를 들어 다음과 같은 코퍼스가 있다고 가정해봅시다.

코퍼스 : Time is an illusion. Lunchtime double so!

이에 대하여 구두점을 제거하고 띄어쓰기를 기준으로 단어를 구분하면 다음과 같습니다.

"Time", "is", "an", "illustion", "Lunchtime", "double", "so"

이를 가장 기초적인 단어 토큰화라고 할 수 있습니다. 하지만 현실에서 단어 토큰화를 수행할 때에는 훨씬 더 많은 고려해야 할 사항들이 존재합니다.

한국어의 경우에는 텍스트를 단어 단위로 구분할 때 띄어쓰기만을 기준으로 하면 단어 토큰화가 제대로 수행되지 않습니다. 왜냐하면 근본적으로 한국어는 교착어이고, 영어에 비해 띄어쓰기가 잘 지켜지지 않는 경향이 있기 때문입니다.

## 2. 단어 토큰화에서 고려해야 할 사항

굉장히 다양한 상황들이 존재하기 때문에, 단어 토큰화를 단지 구두점을 제거하고 공백을 기준으로 단어를 구분하는 작업이라고 여길 수는 없습니다. 단어 토큰화를 수행할 때에는 여러 고려사항이 존재합니다.

**구두점 및 특수 문자**  
코퍼스에 대해서 단어 토큰화를 수행하고자 할 때, 구두점 혹은 특수문자를 무조건 제거하는 것은 바람직하지 않습니다. 이때 구두점(punctuation)은 온점(.), 쉼표(,), 물음표(?), 느낌표(!) 세미콜론(;) 등과 같은 기호를 의미합니다. 온점(.)의 경우, 기본적으로 문장의 경계를 알 수 있는 문장 구분자(boundary)로서의 역할을 합니다. 또 "Ph.D"와 같이 단어 자체에 구두점이 포함된 경우도 존재합니다. 특수문자 중 $의 경우 "$45.55"와 같이 가격을 의미하기도 하고, /의 경우 "01/02/06"와 같이 날짜를 의미하기도 합니다. 이렇게 다양한 상황들이 존재하기 때문에 구두점과 특수문자를 무조건적으로 제거하는 것은 바람직하지 않습니다.

**줄임말**  
영어의 경우 아포스트로피(')는 단어를 줄임말로 쓸 때 사용됩니다. 예를 들어, "we're"은 "we are", "I'm"은 "I am"의 줄임말인데, 이때 "re"와 "m"을 접어(clitic)라고 합니다. 예를 들어 다음과 같은 코퍼스에 대해서 단어 토큰화를 수행한다고 가정해봅시다.

코퍼스 : Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop.

이때 아포스트로피를(')가 존재하는 단어 "Don't"와 "Jone's"에 대하여 어떻게 토큰으로 구분할 것인지의 문제가 존재합니다.

```python
# word_tokenize
from nltk.tokenize import word_tokenize
text = "Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."
print(word_tokenize(text))
```

    ['Do', "n't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', ',', 'Mr.', 'Jone', "'s", 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']


Don't => Do 와 n't 로 구분  
Jone's => Jone 와 's로 구분 

```python
# WordPunctTokenizer : 구두점을 별도의 토큰으로 구분
from nltk.tokenize import WordPunctTokenizer
text = "Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."
print(WordPunctTokenizer().tokenize(text))
```

    ['Don', "'", 't', 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', ',', 'Mr', '.', 'Jone', "'", 's', 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop', '.']


Don't => Don 와 ' 와 t 로 구분  
Jone's => Jone 와 ' 와 s 로 구분

**단어 내에 공백이 존재하는 경우**  
예를 들어, "New York"이나 "rock n roll"과 같은 단어의 경우에는 한 단어임에도 불구하고 단어 내에 공백이 존재합니다. 단어 토큰화는 이러한 단어들을 한 단어 즉 한 토큰으로 인식할 수 있어야 합니다.

## 3. Penn Treebank Tokenization

Penn Treebank Tokenization은 표준으로 쓰이고 있는 토큰화 방법 중 하나입니다. Penn Treebank Tokenization의 규칙은 다음과 같습니다.

규칙 1. 하이푼으로 구성된 단어는 하나로 유지한다.
규칙 2. 아포스트로피로 접어가 함께 하는 단어는 분리한다.

아래와 같은 코퍼스에 대하여 Penn Treebank Tokenization을 수행해보고자 합니다.

코퍼스 : "Starting a home-based restaurant may be an ideal. it doesn't have a food chain or restaurant of their own."

```python
from nltk.tokenize import TreebankWordTokenizer
tokenizer = TreebankWordTokenizer()
text = "Starting a home-based restaurant may be an ideal. it doesn't have a food chain or restaurant of their own."
print(tokenizer.tokenize(text))
```

    ['Starting', 'a', 'home-based', 'restaurant', 'may', 'be', 'an', 'ideal.', 'it', 'does', "n't", 'have', 'a', 'food', 'chain', 'or', 'restaurant', 'of', 'their', 'own', '.']

규칙1에 따라 "home-based"의 경우 하나의 토큰으로 인식합니다.  
규칙2에 따라 "dosen't"의 경우 "does"와 "n't"로 분리합니다.

