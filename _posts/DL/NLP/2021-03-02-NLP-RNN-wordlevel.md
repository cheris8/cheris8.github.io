---
title: '[NLP] 단어 단위 RNN Code'

categories:
  - Deep Learning
tags:
  - Natural Language Processing
  - Text Mining
  - Text Preprocessing
  - Deep Learning
  - PyTorch

last_modified_at: 2021-03-02T08:06:00-05:00

classes: wide
use_math: true
---

단어 단위 RNN은 입출력의 단위가 단어 수준(word level)인 RNN을 말합니다.

## Code

"Repeat is the best medicine for"을 입력받으면 "is the best medicine for memory"를 출력하는 단어 단위 RNN을 만들고자 합니다. 그리고 단어 단위를 사용함에 따라 `PyTorch`에서 제공하는 임베딩 층(embedding layer)를 사용합니다.

### 1. 데이터 생성 및 전처리

```python
import torch
import torch.nn as nn
import torch.optim as optim
```

```python
sentence = 'Repeat is the best medicine for memory'.split(' ')
```

```python
# 단어 집합 생성
vocab = list(set(sentence))
print(vocab)
```

    ['best', 'medicine', 'is', 'for', 'Repeat', 'the', 'memory']

```python
# 단어를 key로 인덱스를 value로 갖는 딕셔너리 생성
# 단어(문자) => 인덱스(정수)
word2index = {tkn: i for i, tkn in enumerate(vocab, 1)} # 각 단어에 고유한 정수 인덱스 부여 (1부터)
word2index['<unk>'] = 0 # 단어 집합에 모르는 단어를 의미하는 <unk> 토큰 추가
print(word2index)
```

    {'best': 1, 'medicine': 2, 'is': 3, 'for': 4, 'Repeat': 5, 'the': 6, 'memory': 7, '<unk>': 0}

```python
# 확인
print(word2index['memory'])
```

    7

```python
# 예측 단계에서 예측한 문장을 확인하기 위해 인덱스를 key로, 단어를 value로 갖는 딕셔너리 생성
# 인덱스(정수) => 단어(문자)
index2word = {v: k for k, v in word2index.items()} # 단어 집합 word2index의 key와 value를 바꾸어 index2word 생성
print(index2word)
```

    {1: 'best', 2: 'medicine', 3: 'is', 4: 'for', 5: 'Repeat', 6: 'the', 7: 'memory', 0: '<unk>'}

```python
# 확인
print(index2word[2])
```

    medicine

```python
# 데이터 생성 함수 정의
def build_data(sentence, word2index):
    encoded = [word2index[token] for token in sentence] # 각 단어를 정수로 변환 (정수 인코딩)
    input_seq, label_seq = encoded[:-1], encoded[1:] # 입력 시퀀스와 레이블 시퀀스를 분리
    input_seq = torch.LongTensor(input_seq).unsqueeze(0) # 배치 차원 추가
    label_seq = torch.LongTensor(label_seq).unsqueeze(0) # 배치 차원 추가
    return input_seq, label_seq
```

```python
# 입력 데이터와 레이블 데이터 생성
X, Y = build_data(sentence, word2index)
```

```python
# 입력 데이터와 레이블 데이터 확인
print(X)
print(Y)
```

    tensor([[5, 3, 6, 1, 2, 4]])
    tensor([[3, 6, 1, 2, 4, 7]])

### 2. 모델 구현

```python
# 하이퍼파라미터 설정
vocab_size = len(word2index) # 단어 집합 크기
input_size = 5 # 임베딩 차원 크기 = 입력 차원 크기
hidden_size = 20 # 은닉 상태 크기
```

```python
# 모델 정의
class Net(nn.Module):
    def __init__(self, vocab_size, input_size, hidden_size, batch_first=True):
        super(Net, self).__init__()
        self.embedding_layer = nn.Embedding(num_embeddings=vocab_size, # 워드 임베딩
                                            embedding_dim=input_size)
        self.rnn_layer = nn.RNN(input_size, hidden_size, # 입력 차원, 은닉 상태의 크기 정의
                                batch_first=batch_first)
        self.linear = nn.Linear(hidden_size, vocab_size) # 출력의 차원은 단어 집합 크기 = 원-핫 벡터 차원

    def forward(self, x):
        # 1. 임베딩 층
        # 크기 변화 : (배치 크기, 시퀀스 길이) => (배치 크기, 시퀀스 길이, 임베딩 차원)
        output = self.embedding_layer(x)
        # 2. RNN 층
        # output 크기 변화 : (배치 크기, 시퀀스 길이, 임베딩 차원) => (배치 크기, 시퀀스 길이, 은닉층 크기)
        # hidden 크기 변화 : (배치 크기, 시퀀스 길이, 임베딩 차원) => (1, 배치 크기, 은닉층 크기)
        output, hidden = self.rnn_layer(output)
        # 3. 출력층
        # 크기 변화 : (배치 크기, 시퀀스 길이, 은닉층 크기) => (배치 크기, 시퀀스 길이, 단어 집합 크기)
        output = self.linear(output)
        # 4. view를 통해 배치 차원 제거
        # 크기 변화 : (배치 크기, 시퀀스 길이, 단어 집합 크기) => (배치 크기*시퀀스 길이, 단어 집합 크기)
        return output.view(-1, output.size(2))
```

```python
# 모델 선언
model = Net(vocab_size, input_size, hidden_size, batch_first=True)
```

```python
# loss & optimizer
loss_function = nn.CrossEntropyLoss() # 소프트맥스 함수 포함이며 실제값은 원-핫 인코딩 안 해도 ok
optimizer = optim.Adam(params=model.parameters())
```

```python
# 모델 확인 (가중치는 전부 랜덤 초기화 된 상태)
output = model(X)
print(output)
```

    tensor([[ 0.0702,  0.0857,  0.1352,  0.0065, -0.2174, -0.2087, -0.0938, -0.3729],
            [-0.1581,  0.0726, -0.3605, -0.1337, -0.3888, -0.1497, -0.1801, -0.0356],
            [-0.1138, -0.1225,  0.1579,  0.1254, -0.2498,  0.1265,  0.1882, -0.3915],
            [ 0.0388,  0.2732, -0.4646, -0.1958, -0.4034, -0.1787,  0.0434,  0.0856],
            [ 0.1103, -0.1595,  0.4052,  0.1230,  0.0292,  0.0667, -0.1724, -0.3276],
            [-0.0367,  0.1632, -0.0482,  0.0543, -0.3224, -0.4305, -0.1772, -0.3652]],
           grad_fn=<ViewBackward>)

```python
# 예측값 크기 확인 : (시퀀스 길이, 은닉층 크기)
print(output.shape)
```

    torch.Size([6, 8])

예측값의 크기는 (6, 8)입니다. 이는 각각 (시퀀스의 길이, 은닉층의 크기)에 해당됩니다. 모델은 훈련시키기 전에 예측을 제대로 하고 있는지 예측된 정수 시퀀스를 다시 단어 시퀀스로 바꾸는 decode 함수를 만듭니다.

```python
# 정수 시퀀스 => 단어 시퀀스
decode = lambda y: [index2word.get(x) for x in y]
```

### 3. 학습

```python
# training
n_epochs = 100
for epoch in range(1, n_epochs+1):

    optimizer.zero_grad()

    output = model(X)
    loss = loss_function(output, Y.view(-1))
    
    loss.backward()
    optimizer.step()

    if epoch % 20 == 0:
        print("[{:02d}/100] {:.4f} ".format(epoch, loss))
        pred = output.softmax(-1).argmax(-1).tolist()
        print(" ".join(["Repeat"] + decode(pred)))
        print()
```

    [20/100] 1.8604 
    Repeat medicine the best best for is
    
    [40/100] 1.5029 
    Repeat is the best medicine for memory
    
    [60/100] 1.1430 
    Repeat is the best medicine for memory
    
    [80/100] 0.8419 
    Repeat is the best medicine for memory
    
    [100/100] 0.6199 
    Repeat is the best medicine for memory

## 참고자료

[PyTorch로 시작하는 딥 러닝 입문](https://wikidocs.net/book/2788)

