---
title: '[텍스트 전처리] 문장 토큰화 (Sentence Tokenization)'

categories:
  - Data Analysis
tags:
  - Text Preprocessing

last_modified_at: 2020-08-21T08:06:00-05:00

classes: wide
---

이 글은 문장 토큰화(Sentence Tokenization)의 개념 및 코드에 관한 기록입니다.

## 개념

문장 토큰화는 토큰의 단위를 문장으로 하여, 코퍼스 내 텍스트를 문장 단위로 구분하는 작업을 의미합니다. 텍스트를 문장 단위로 구분할 때에는 기본적으로 온점(.), 물음표(?), 느낌표(!) 등을 기준으로 합니다. 하지만, 꽤 정확하게 문장 구분자(boundary)로서의 역할을 수행하는 물음표 및 느낌표와는 달리, 온점(.)은 꼭 문장의 끝이 아니더라도 등장할 수 있기 때문에 어려움이 존재합니다. 예를 들어 다음과 같은 코퍼스가 있다고 가정해봅시다.

ex1) IP 192.168.56.31 서버에 들어가서 로그 파일 저장해서 ukairia777@gmail.com로 결과 좀 보내줘. 그러고나서 점심 먹으러 가자.  
ex2) Since I'm actively looking for Ph.D. students, I get the same question a dozen times every year.

ex1의 경우, 문장 토큰화 결과 "IP 192.168.56.31 서버에 들어가서 로그 파일 저장해서 ukairia777@gmail.com로 결과 좀 보내줘.", "그러고나서 점심 먹으러 가자." 두개의 문장으로 구분되는 것이 가장 적절합니다. ex2의 경우, 문장 토큰화 결과 "Since I'm actively looking for Ph.D. students, I get the same question a dozen times every year." 한개의 문장으로 구분되는 것이 가장 적절합니다. 하지만 단순히 온점만을 기준으로 문장을 구분한다면, 이와 같이 적절하게 문장 토큰화가 진행되지 않을 것입니다. 그렇기 때문에 코퍼스가 어떤 국적의 언어인지, 코퍼스 내에서 특수문자들이 어떻게 사용되고 있는지 등에 따라 문장 토큰화 규칙들을 정의할 필요가 있습니다.

## Code

영어의 경우 NLTK의 `sent_tokenize`를 사용하여 영어 문장 토큰화를 수행할 수 있습니다.

```python
from nltk.tokenize import sent_tokenize
text = 'His barber kept his word. But keeping such a huge secret to himself was driving him crazy. Finally, the barber went up a mountain and almost to the edge of a cliff. He dug a hole in the midst of some reeds. He looked about, to mae sure no one was near.'
print(sent_tokenize(text))
```

    ['His barber kept his word.', 'But keeping such a huge secret to himself was driving him crazy.', 'Finally, the barber went up a mountain and almost to the edge of a cliff.', 'He dug a hole in the midst of some reeds.', 'He looked about, to mae sure no one was near.']

```python
# 문장 중간에 .이 있는 경우
from nltk.tokenize import sent_tokenize
text = 'I am actively looking for Ph.D. students. and you are a Ph.D student.'
print(sent_tokenize(text))
```

    ['I am actively looking for Ph.D. students.', 'and you are a Ph.D student.']

한국어의 경우 박상길님이 개발한 KSS(Korean Sentence Splitter)를 통해 한국어 문장 토큰화를 수행할 수 있습니다.

```python
import kss
text = "딥 러닝 자연어 처리가 재미있기는 합니다. 그런데 문제는 영어보다 한국어로 할 때 너무 어려워요. 농담아니에요. 이제 해보면 알걸요?"
print(kss.split_sentences(text))
```

