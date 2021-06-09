---
title: '[NLP] 문자 단위 RNN (Character level RNN) Code'

categories:
  - Artificial Intelligence
tags:
  - Natural Language Processing

last_modified_at: 2021-03-01T08:06:00-05:00

classes: wide
use_math: true
---

문자 단위 RNN (Char RNN)은 입출력의 단위가 문자 수준(character level)인 RNN을 말합니다. 이는 다대다(many-to-many) 구조를 갖는 RNN으로, 품사 태깅, 개체명 인식 등에 사용됩니다.

## Code

문자 시퀀스 apple!에 대하여 apple을 입력받으면 pple!를 출력하는 RNN을 구현하고자 합니다.

```python
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np
```

### 1. 데이터 생성

```python
# 입력 데이터와 레이블 데이터 생성
input_str = 'apple' # input string 생성
label_str = 'pple!' # label string 생성
```

```python
# 문자 집합과 문자 집합 크기 생성
char_vocab = sorted(list(set(input_str+label_str))) # 문자 집합 : 중복을 제거한 문자들의 집합
vocab_size = len(char_vocab)
print('문자 집합 : {}'.format(char_vocab))
print('문자 집합의 크기 : {}'.format(vocab_size))
```

    문자 집합 : ['!', 'a', 'e', 'l', 'p']
    문자 집합의 크기 : 5

### 2. 데이터 전처리

```python
# 정수 인코딩
# 문자 => 인덱스
char_to_index = dict((c, i) for i, c in enumerate(char_vocab)) # 각각의 문자에 고유한 정수 인덱스 부여
print(char_to_index)
```

    {'!': 0, 'a': 1, 'e': 2, 'l': 3, 'p': 4}

```python
# 디코딩 (예측 결과를 다시 문자 시퀀스로 보기 위해)
# 인덱스 => 문자
index_to_char={}
for key, value in char_to_index.items():
    index_to_char[value] = key
print(index_to_char)
```

    {0: '!', 1: 'a', 2: 'e', 3: 'l', 4: 'p'}

```python
# 입력 데이터와 레이블 데이터에 대하여 정수 인코딩
x_data = [char_to_index[c] for c in input_str]
y_data = [char_to_index[c] for c in label_str]
print(x_data)
print(y_data)
```

    [1, 4, 4, 3, 2]
    [4, 4, 3, 2, 0]

```python
# 배치 차원 추가
# 파이토치의 nn.RNN()은 기본적으로 3차원 텐서를 입력받기 때문
x_data = [x_data]
y_data = [y_data]
print(x_data)
print(y_data)
```

    [[1, 4, 4, 3, 2]]
    [[4, 4, 3, 2, 0]]

```python
# 원-핫 인코딩
x_one_hot = [np.eye(vocab_size)[x] for x in x_data]
print(x_one_hot)
```

    [array([[0., 1., 0., 0., 0.],
           [0., 0., 0., 0., 1.],
           [0., 0., 0., 0., 1.],
           [0., 0., 0., 1., 0.],
           [0., 0., 1., 0., 0.]])]

```python
# 입력 데이터와 레이블 데이터를 텐서로 변환
X = torch.FloatTensor(x_one_hot)
Y = torch.LongTensor(y_data)
print('훈련 데이터의 크기 : {}'.format(X.shape))
print('레이블의 크기 : {}'.format(Y.shape))
```

    훈련 데이터의 크기 : torch.Size([1, 5, 5])
    레이블의 크기 : torch.Size([1, 5])

### 3. 모델 구현

```python
# 하이퍼파라미터 설정
input_size = vocab_size # 입력의 크기는 문자 집합의 크기
hidden_size = 5
output_size = 5
learning_rate = 0.1
```

```python
# 모델 정의
class Net(torch.nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super(Net, self).__init__()
        self.rnn = nn.RNN(input_size, hidden_size, batch_first=True) # rnn
        self.fc = nn.Linear(hidden_size, output_size, bias=True) # 출력층

    def forward(self, x):
        x, _status = self.rnn(x)
        x = self.fc(x)
        return x
```

```python
# 모델 선언
net = Net(input_size, hidden_size, output_size)
```

```python
# 출력의 크기 확인
outputs = net(X)
print(outputs.shape) # 3차원 텐서
```

    torch.Size([1, 5, 5])

```python
print(outputs.view(-1, input_size).shape) # 2차원 텐서로 변환
```

    torch.Size([5, 5])

```python
# 레이블 데이터의 크기 확인
print(Y.shape)
print(Y.view(-1).shape)
```

    torch.Size([1, 5])
    torch.Size([5])

### 4. 학습

```python
# loss & optimizer
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(net.parameters(), learning_rate)
```

```python
# training
epochs = 100
for i in range(epochs):
    
    # gradient 초기화
    optimizer.zero_grad()
    
    # forward pass
    outputs = net(X)
    loss = criterion(outputs.view(-1, input_size), Y.view(-1)) # view를 통해 배치 차원 제거
    
    # back propagation
    loss.backward()
    optimizer.step()

    # 예측 결과 출력
    result = outputs.data.numpy().argmax(axis=2) # 최종 예측값인 각 time-step 별 5차원 벡터에 대해서 가장 높은 값의 인덱스를 선택
    result_str = ''.join([index_to_char[c] for c in np.squeeze(result)])
    print(i, "loss: ", round(loss.item(), 3), "prediction: ", result, "true Y: ", y_data, "prediction str: ", result_str)
```

    0 loss:  1.508 prediction:  [[3 3 3 3 4]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  llllp
    1 loss:  1.309 prediction:  [[4 4 4 4 4]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  ppppp
    2 loss:  1.121 prediction:  [[4 4 3 4 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pplp!
    3 loss:  0.96 prediction:  [[4 4 3 0 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  ppl!!
    4 loss:  0.782 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    5 loss:  0.615 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    6 loss:  0.472 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    7 loss:  0.346 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    8 loss:  0.241 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    9 loss:  0.165 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    10 loss:  0.116 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    11 loss:  0.087 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    12 loss:  0.069 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    13 loss:  0.056 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    14 loss:  0.046 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    15 loss:  0.037 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    16 loss:  0.03 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    17 loss:  0.025 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    18 loss:  0.021 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    19 loss:  0.017 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    20 loss:  0.015 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    21 loss:  0.013 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    22 loss:  0.011 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    23 loss:  0.01 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    24 loss:  0.009 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    25 loss:  0.008 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    26 loss:  0.007 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    27 loss:  0.006 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    28 loss:  0.005 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    29 loss:  0.005 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    30 loss:  0.004 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    31 loss:  0.004 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    32 loss:  0.004 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    33 loss:  0.003 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    34 loss:  0.003 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    35 loss:  0.003 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    36 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    37 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    38 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    39 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    40 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    41 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    42 loss:  0.002 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    43 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    44 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    45 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    46 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    47 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    48 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    49 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    50 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    51 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    52 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    53 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    54 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    55 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    56 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    57 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    58 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    59 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    60 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    61 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    62 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    63 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    64 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    65 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    66 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    67 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    68 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    69 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    70 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    71 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    72 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    73 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    74 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    75 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    76 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    77 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    78 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    79 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    80 loss:  0.001 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    81 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    82 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    83 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    84 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    85 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    86 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    87 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    88 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    89 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    90 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    91 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    92 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    93 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    94 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    95 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    96 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    97 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    98 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!
    99 loss:  0.0 prediction:  [[4 4 3 2 0]] true Y:  [[4, 4, 3, 2, 0]] prediction str:  pple!

## Code

```python
import torch
import torch.nn as nn
import torch.optim as optim
```

### 1. 데이터 생성 및 전처리

```python
sentence = ("if you want to build a ship, don't drum up people together to "
            "collect wood and don't assign them tasks and work, but rather "
            "teach them to long for the endless immensity of the sea.")
```

```python
# 문자 집합 생성
char_set = list(set(sentence)) # 문자 집합 : 중복을 제거한 문자들의 집합
```

```python
# 정수 인코딩
char_dic = {c: i for i, c in enumerate(char_set)} # 각 문자에 대하여 정수 인코딩
print(char_dic)
```

    {'c': 0, 'b': 1, 'y': 2, 'o': 3, 'i': 4, 'e': 5, ' ': 6, 'r': 7, 'g': 8, 'w': 9, 'u': 10, 'd': 11, 't': 12, 'h': 13, "'": 14, '.': 15, 'm': 16, 'a': 17, 'n': 18, 'f': 19, 'l': 20, 'p': 21, 's': 22, ',': 23, 'k': 24}

```python
# 문자 집합의 크기 생성
dic_size = len(char_dic)
print('문자 집합의 크기 : {}'.format(dic_size))
```

    문자 집합의 크기 : 25

```python
# 하이퍼파라미터 설정
hidden_size = dic_size # 사용자 지정
sequence_length = 10 # 사용자 지정 (10개 단위로 끊어서 샘플 생성할 것)
learning_rate = 0.1
```

```python
# 입력 데이터와 레이블 데이터 생성
# 문자 => 인덱스 (정수 인코딩)
x_data = []
y_data = []

for i in range(0, len(sentence) - sequence_length):
    x_str = sentence[i:i+sequence_length]
    y_str = sentence[i+1:i+sequence_length+1]
    print(i, x_str, '->', y_str)

    x_data.append([char_dic[c] for c in x_str]) # x str to index
    y_data.append([char_dic[c] for c in y_str]) # y str to index
```

    0 if you wan -> f you want
    1 f you want ->  you want 
    2  you want  -> you want t
    3 you want t -> ou want to
    4 ou want to -> u want to 
    5 u want to  ->  want to b
    6  want to b -> want to bu
    7 want to bu -> ant to bui
    8 ant to bui -> nt to buil
    9 nt to buil -> t to build
    10 t to build ->  to build 
    11  to build  -> to build a
    12 to build a -> o build a 
    13 o build a  ->  build a s
    14  build a s -> build a sh
    15 build a sh -> uild a shi
    16 uild a shi -> ild a ship
    17 ild a ship -> ld a ship,
    18 ld a ship, -> d a ship, 
    19 d a ship,  ->  a ship, d
    20  a ship, d -> a ship, do
    21 a ship, do ->  ship, don
    22  ship, don -> ship, don'
    23 ship, don' -> hip, don't
    24 hip, don't -> ip, don't 
    25 ip, don't  -> p, don't d
    26 p, don't d -> , don't dr
    27 , don't dr ->  don't dru
    28  don't dru -> don't drum
    29 don't drum -> on't drum 
    30 on't drum  -> n't drum u
    31 n't drum u -> 't drum up
    32 't drum up -> t drum up 
    33 t drum up  ->  drum up p
    34  drum up p -> drum up pe
    35 drum up pe -> rum up peo
    36 rum up peo -> um up peop
    37 um up peop -> m up peopl
    38 m up peopl ->  up people
    39  up people -> up people 
    40 up people  -> p people t
    41 p people t ->  people to
    42  people to -> people tog
    43 people tog -> eople toge
    44 eople toge -> ople toget
    45 ople toget -> ple togeth
    46 ple togeth -> le togethe
    47 le togethe -> e together
    48 e together ->  together 
    49  together  -> together t
    50 together t -> ogether to
    51 ogether to -> gether to 
    52 gether to  -> ether to c
    53 ether to c -> ther to co
    54 ther to co -> her to col
    55 her to col -> er to coll
    56 er to coll -> r to colle
    57 r to colle ->  to collec
    58  to collec -> to collect
    59 to collect -> o collect 
    60 o collect  ->  collect w
    61  collect w -> collect wo
    62 collect wo -> ollect woo
    63 ollect woo -> llect wood
    64 llect wood -> lect wood 
    65 lect wood  -> ect wood a
    66 ect wood a -> ct wood an
    67 ct wood an -> t wood and
    68 t wood and ->  wood and 
    69  wood and  -> wood and d
    70 wood and d -> ood and do
    71 ood and do -> od and don
    72 od and don -> d and don'
    73 d and don' ->  and don't
    74  and don't -> and don't 
    75 and don't  -> nd don't a
    76 nd don't a -> d don't as
    77 d don't as ->  don't ass
    78  don't ass -> don't assi
    79 don't assi -> on't assig
    80 on't assig -> n't assign
    81 n't assign -> 't assign 
    82 't assign  -> t assign t
    83 t assign t ->  assign th
    84  assign th -> assign the
    85 assign the -> ssign them
    86 ssign them -> sign them 
    87 sign them  -> ign them t
    88 ign them t -> gn them ta
    89 gn them ta -> n them tas
    90 n them tas ->  them task
    91  them task -> them tasks
    92 them tasks -> hem tasks 
    93 hem tasks  -> em tasks a
    94 em tasks a -> m tasks an
    95 m tasks an ->  tasks and
    96  tasks and -> tasks and 
    97 tasks and  -> asks and w
    98 asks and w -> sks and wo
    99 sks and wo -> ks and wor
    100 ks and wor -> s and work
    101 s and work ->  and work,
    102  and work, -> and work, 
    103 and work,  -> nd work, b
    104 nd work, b -> d work, bu
    105 d work, bu ->  work, but
    106  work, but -> work, but 
    107 work, but  -> ork, but r
    108 ork, but r -> rk, but ra
    109 rk, but ra -> k, but rat
    110 k, but rat -> , but rath
    111 , but rath ->  but rathe
    112  but rathe -> but rather
    113 but rather -> ut rather 
    114 ut rather  -> t rather t
    115 t rather t ->  rather te
    116  rather te -> rather tea
    117 rather tea -> ather teac
    118 ather teac -> ther teach
    119 ther teach -> her teach 
    120 her teach  -> er teach t
    121 er teach t -> r teach th
    122 r teach th ->  teach the
    123  teach the -> teach them
    124 teach them -> each them 
    125 each them  -> ach them t
    126 ach them t -> ch them to
    127 ch them to -> h them to 
    128 h them to  ->  them to l
    129  them to l -> them to lo
    130 them to lo -> hem to lon
    131 hem to lon -> em to long
    132 em to long -> m to long 
    133 m to long  ->  to long f
    134  to long f -> to long fo
    135 to long fo -> o long for
    136 o long for ->  long for 
    137  long for  -> long for t
    138 long for t -> ong for th
    139 ong for th -> ng for the
    140 ng for the -> g for the 
    141 g for the  ->  for the e
    142  for the e -> for the en
    143 for the en -> or the end
    144 or the end -> r the endl
    145 r the endl ->  the endle
    146  the endle -> the endles
    147 the endles -> he endless
    148 he endless -> e endless 
    149 e endless  ->  endless i
    150  endless i -> endless im
    151 endless im -> ndless imm
    152 ndless imm -> dless imme
    153 dless imme -> less immen
    154 less immen -> ess immens
    155 ess immens -> ss immensi
    156 ss immensi -> s immensit
    157 s immensit ->  immensity
    158  immensity -> immensity 
    159 immensity  -> mmensity o
    160 mmensity o -> mensity of
    161 mensity of -> ensity of 
    162 ensity of  -> nsity of t
    163 nsity of t -> sity of th
    164 sity of th -> ity of the
    165 ity of the -> ty of the 
    166 ty of the  -> y of the s
    167 y of the s ->  of the se
    168  of the se -> of the sea
    169 of the sea -> f the sea.

```python
print(x_data[0])
print(y_data[0])
```

    [4, 19, 6, 2, 3, 10, 6, 9, 17, 18]
    [19, 6, 2, 3, 10, 6, 9, 17, 18, 12]

```python
# 원-핫 인코딩
x_one_hot = [np.eye(dic_size)[x] for x in x_data]
```

```python
# 입력 데이터와 레이블 데이터를 텐서로 변환
X = torch.FloatTensor(x_one_hot)
Y = torch.LongTensor(y_data)
print('훈련 데이터의 크기 : {}'.format(X.shape))
print('레이블의 크기 : {}'.format(Y.shape))
```

    훈련 데이터의 크기 : torch.Size([170, 10, 25])
    레이블의 크기 : torch.Size([170, 10])

### 2. 모델 구현

```python
# 모델 정의
class Net(torch.nn.Module):
    def __init__(self, input_dim, hidden_dim, layers):
        super(Net, self).__init__()
        self.rnn = nn.RNN(input_dim, hidden_dim, num_layers=layers, batch_first=True)
        self.fc = nn.Linear(hidden_dim, hidden_dim, bias=True)

    def forward(self, x):
        x, _status = self.rnn(x)
        x = self.fc(x)
        return x
```

```python
# 모델 선언
net = Net(dic_size, hidden_size, 2) # hidden layer 2개
```

```python
# 출력의 크기 확인
outputs = net(X)
print(outputs.shape) # 3차원 텐서
```

    torch.Size([170, 10, 25])

```python
print(outputs.view(-1, dic_size).shape) # 2차원 텐서로 변환
```

    torch.Size([1700, 25])

```python
# 레이블 데이터의 크기 확인
print(Y.shape)
print(Y.view(-1).shape)
```

    torch.Size([170, 10])
    torch.Size([1700])

### 3. 학습

```python
# loss & optimizer
criterion = torch.nn.CrossEntropyLoss()
optimizer = optim.Adam(net.parameters(), learning_rate)
```

```python
# training
epochs = 100
for i in range(epochs):
    
    # gradient 초기화
    optimizer.zero_grad()
    
    # forward pass
    outputs = net(X) # X의 텐서 크기는 (170, 10, 25)
    loss = criterion(outputs.view(-1, dic_size), Y.view(-1))
    
    # back propagation
    loss.backward()
    optimizer.step()

    # 예측 결과 출력
    results = outputs.argmax(dim=2) # results의 텐서 크기는 (170, 10)
    predict_str = ""
    for j, result in enumerate(results):
        if j == 0: # 처음에는 예측 결과를 전부 가져오지만
            predict_str += ''.join([char_set[t] for t in result])
        else: # 그 다음에는 마지막 글자만 반복 추가
            predict_str += char_set[result[-1]]

    print(predict_str)
```

    hhfhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhuhhhhhhhhhhhhhhhhhhhhhhhhfhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhuhhhhhhhhhhhhhhhhhhhhhfhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
    td                    d                                                   d                                                               d                                        
      's ''s '    '  ''  ''           'h '   't   's  h '   '    's ''      ''       '   h           '     'd  h 't  'h     'r    '   hmr  h'   ''   't  'h  '     '     '  '         m
    o  o                                                                                                                                                                               
    nhn n n n                n                                                                                                 n                                                       
    ntit titltttnttttttttttttitttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttltttttttttttttttttttttttttttttttitttttttttttttttttttttttttttttttatttattttttttttttttttttt
    nt r tht ttttttrtttrrrtttrtrrtrttttrrtttttrtrtttrttttrttrtttttttttrttttrttrrttttrttttrttttrrttrttttttrtrtttttttrrrtttrrttrttttrttrttrttttttrrtrtrttttrtrtttrtttrttrtttttrrtrttrttrr
    teta at taa   a  aa a  aaa  a aa   aaa a  a   a  a  aa  aa   a  aa   a  aaa  aa  aa     aa    aaa  aa    aa  aa     a    aaa  aa a  aaa  a  aaa  aa  aa  aa  aa  aa  aa a aa  aa  a
    tet t t t                                                                                                                                                                          
    tet                                                                                                                                                                                
    wnt                                                                                                                                                                                
    wh                                                                                                                                                                                 
    wh     o                                                                    o                              o                                                                       
     h t   o    o         e                                    o          o     o               oe   o      o  o           o    oo    oe   o           oe  oo                   oe   o 
    lt t   o   to tt t  too tttot    ot tt t t  o t  toot o  tto to tt ooto   t o to t o    tt toe tto  tttoo to t  tt ot to  ttoot   o  ttoot  tttoot oe too t   ttot   t o  t oe toot
    lt t t o ttto tt ttttot tttot tttot tt t ttto t  to t ot tto to tt   to ttt otto ttott  ttttoe tto tttttotto tt tt ottto  ttooto tot tto to ttto ttoe toott t tt ttt ttot ttoe t ot
    te t too  ttottt t  ttte ttet t tot tt tt tto  t to   te ttotto   t   o  ttte tot totte ttttoe  tot ttttetto  t tteotttoe ttottoetoe  to to  tto ttoe toe  tt ttettt ttot ttoe ttot
    te t  to   to tt    toto  totot tot         o    to   oe  to to     o o   toe to  tot      toe  toe   toe to    t     toe  toetoe oe  to  o   to  toe  oe     tte  tet o   toe too 
    te o  to  ttoetto   toto  toto  tot    to  oo    toe  oe  toeto    oo o   toe to  tot o    toe  toe   toe to    tto  otoe ttoetoe oe  to to   to  toe ton     tt  ttet o o toe toet
    teto  to    o        oto    to              o     o   oe   o  o       o   to   o      o     oe   o         o           oe  to  o  oe   o  o    o   o   o               o    oe  o  
    te he  o  ttonta ntttaaantttton tot n  ta tton t ton the  tonthn  taoto  ttaenton tottntt  toe  ton t ttento  t tt ttanhe ttontoetoe  tontan  tan toe ton  tt ttn tt taat nthe tann
    t to  to   to ta    thto    to         to        to   oe  to to       o   th  to      e    toe  to        to    tt     oe  to to  oe  to to   to  to  to       t       o    oe  o t
    p to  to   to tt    ttto    to         to        to   he  to to       o   the to      e    toe  to    t e to    tt     oe  to toe oe  to to   to  toe to       t  t   to   toe te t
    p th eto   to tae   ttteett to  tot    te te   e to  the  th th   te to t tte to  totteet  the  the e tte to    tte t the  thotoe oe  th te   te  the to   teett  te tto   the te t
    p th  to   to ta    ttto tt to  t t    to t   t  to  the  to to   t  to tttae to  t tto t  the  tht   tte to  t tte t the  to to  oe  th to   to  the to   t  tt  te tt t  the te t
    f th  to   to ta    ttto tt to  t t    to t   t  to  toe  to to   to to t tte to  t tto    toe  to    tt  to  t tt  t toe  to to toe  to to   to  toe to   t  tt  te  t t  toe te t
    c ta lto   to ta    ttae tteto  tot    te te  le to etoe  to to e ta to t tte to  totte    toe  toi   tte to  t tt  tathe  tooto toe  to to   to  toe te   te t   te  t t  toe te t
    m ta lto   to ta    ttae tteto  tot  t t  te  le to ethe  to to l ta to t tte to ltottie   toe  toite tte to ,t tt  ttthe  to to toem to to   to ,toemte   t  t   te  t t  toemte t
    m th ,to , to ba    ttte lt to ,t t  t t  t   le to ethe  to bo ll t to d ttd to ,t tth    toe  tont  tte to ,t tt  t the  to to toe  to to , to ,toemth   t  t   te  t t  toemte t
    c th lto ' th th    tnhnent ton't t  t t  t   le th ethe  to th l  a to d tah to 't tthen  the  thnte tth to 't tt  t the  to th toe  th to ' to 'the th    tetn  tee t t  the te t
    c th 'to   thnth    tnheent ton't t    t  te  le thnethe  tonth e  h to l tnh ton t ttheg  the  thnee tth to 't tt  t the  tonthetoe  th te   to 'the ts   ttetnn thegt t  the tern
    c ah 'to   th bh l  tnhee t ton't t  l t  te  le th lthe  to bh l  t to l tnd ton't ttheg  toe  tonee tnh to 't tt  t the  tonthetoe  to be ' bo 'the ts  l t tn  tdelo t  the tert
    l ah 'to t tontadle tnsnent ton t t    te te  le tonethe  tonth ee a to   tne ton t tnhel  toe  tonte tne to lt tae t the  tonthetoe  to ton' ton'the to  e eetn  neelo tn the tert
    p nh 'to t to tatle tnthelt ton't t l  t  te  le to lthe  to to e  t to   tni ton't tnsel  toe  tontt tns to 't tt  t the  torthetoe  to to ' ton'toe tn  ept tn  tiilo t  toe tert
    p nh 'to t to batle tntselt ton't t '  t ete  le to ethe  to bo eept to l tni ton't ttsil  toe  tonta tni to  t tai tathe  torthetoe  to to ' to 'toe tnd ete tn  tiilo t  the tert
    pysh lto t th tain  tnssilt ton't ard  t  te  le th lther to to l rt to p tni ton't atsiln them tontt tni to  t bai tathem torth toem to bo l to  the trd ens tn  tiiia a  themtsrt
    p sh lto t th bsile tnsselt ton t tr   t ete  le th ethem th bh ee t to   tni tonlt ttsiln them tonds tni to  t bsi tathem torth toem to bo l bo  themtnd ete tn  tiiih ar themtsrt
    p sa eto t th buil  tnssilt ton t tr   t ete  le th ethem to bo l  t to   tni ton't atsiln toem tonds tns to    but tathem torth toem to bo   bo  themtnd e s tn  tiiia a  themtert
    l ao eto t to tsslp tnsselt ton t trt  t ete  le to ethem to to ee t to p tns ton t ttseln toem tondsotns to  t bus tanhem torth toem to ton ebo  themtnd ets tnn nsi a ar toemtert
    l o  eto t to build tnsselt ton't tr e t ete  le to ethem to bo le t to d tns ton't ttsell them tonds tns to    but tanhem torch them to bon  bo kthemtndgets tn  tdepa an themtert
    p oo eto t tontsind tnssent ton t tr   t  te  le tonenhem to to le t to d tni ton t tnseln toem tonds tns to  t tut tanhem tornh toem to ton ebo  themtnd etsetn  niita an toemtern
    p uo lto t tonbuild tnssenb ton t ar d t  te  le thnenhem th bo d  t to d tni ton't anhiln them thnds ans to  t bus aanhem thrch them th bon  borethemtndgets tnn tsita an themtert
    p uo etort to build tnssell  o  t t    t  te  le to enhem to bo ee t to d tni to 't tnsiln  hem tonds ani to  , but tanhem torch them to be   bo  themtnd eps tn  tiita an toemtern
    p ta ltont tonbuilp tnshepb ton't artd t  ae ple tonenhem to bo lirt torp tni ton't ansiln them tonds ans ton , but aauhem torch aoem to bon' bor themtnd etsitnn uiita an toemteru
    p to ltont to build tnship, bon't ar d t  te ple to ethem to bo lett tood tni bon't tnsiln them tonds ans to  , but aauher torch them to bon' boo themtnd ets tnn tiita an toemteru
    p to ltond to tuile tnshinl bon't ao e t  te ple to enhem to to ee t to d tnd ton't ansiln toem tonss and to  , but aathem tooch toem to to ' too toemtnd etsitn  ndita ar toemtert
    p to ltont to build tnshec, bon't trt, tt te ple th euhem to boneept tood tsd bon't ttsilndthem thnds ats ,o k, bus aauhem thrch them th bon'eborkthemtndlets tnn tdita an themtert
    m t  ltond to build anshin, bon't trte tt te  le to ethem to bo lert tord tsd ton't tnsiln them tonds and tor , but aather torch toem to bon'etor toemtrd ets tnn ndita a  toemtert
    m torltond to build anshep, bon't trtd tt te ple to ether to to eett tood tsd ton't tnseln the  tonts and to  , bui aather torch the  to bon'etor toe tnd etsitnm ndita a  the tert
    m torltont th build asshep, bon't tr d t  te ple th ether to boneept tord tsd bon't tnseln ther thnws ans ,o  , but aather torch the  th bon'ebor the tnd ets tn  psita an the tert
    m torltond to bsild anshin, bon't ao e t  te ple to enhem to bo lepa tood tnd ton't ansign them tonds ans to  , but aathem torch them to bon'eboo toemtnd etsitnm ndima ar themtert
    m torltond to buind anshin, bon't arul tp te  le to enhem to bo lept tord tnd ton't ansign them tonds and tor , but aanhem torch them to bon' for themtnd etsitnm ndima ao themtert
    m torgtont to build anshin, bon't arud tp teople th enhem to bonlept tord tns ton't ansiln ther thnks ans tor , but aauher torch ther th bon' bor thertndletsitnm nsity ar therteru
    m tor tont to build anshin, bon't ao d tp te pleoto enher to bonlept tord tnd don't tnsiln the  tonss ans to  , but aauher torch toe  to bon' bor thertndletsetnm psity ao the tert
    m tor tond to buigd asshin, bon't to m tt teople to ethem to bo litt tord tsd ton't tns gn the  tonks tnd to  , but aauhem toach the  to bon' foo themtndlens tnm ndity ar the teru
    m aorltond to cuigd anship, bon't to m tt teo le to ethem to co lept tord tsd ton't tnsign the  tands and to  , but aauhem toach the  to con' for themtadlensitnmendita ao the tort
    p torltond to cuild asshin, bon't ar m tp people to enhe  to conlept tord tnd don't ansign the  tonds and ,or , but aauhe  torch the  to con' for the tndlens tnmensita ao the tert
    p tor tond to cuill asshin, b n't ao m t  teople to enhem to conlept tord tsd con't ansign them tonds tnd dor , bui aauhem torch the  to con' for the tndlets tnm ndiih a  the tert
    p tor tond to duilt anshin, bon't arum tp terple tonethe  to donlept tord tss ton't ansign the  tonks ass wor , but aathe  torch the  to don' for thertndlets tnmensith ao the tert
    p tor tond to builp anshim, bon't arum tp terple to enhe  to bo lept tord and don't ansign toem tonks and dor , bum aathe  torch the  to bon' for the tndlets tnmensita ao the terc
    g tor tond to builp asshin, bon't ar m tm teo le to ethem to bo lept tord asd don't ansign them tonks tnd dor , bum aathem torch them to bon' for themtsdlemsitnmendiia a  the terc
    g tor tond to build asshin, bon't arum tp terple together to bonlept tord asd ton't ansign them tonks ans tor , but aather torch ther to bon' for themtndlessitnmensity ar therterc
    p aor tord to duind anshin, bon't arum tp terpleptogethem to donlept tord and ton't ans gn toem tanss ans tor , but aathe  torch them to bong for toemtrdlets tnmensity rr the tert
    g aor tond to build asshic, bon't arum tp terple togethem to bonlept ,ord asd don't ansign them tonks and dor , but aathe  torch them th bon' bor themtndlensitnmendity ao the terc
    p tor tond to build anship, bon't arum tp terple to ether to bo lept tord and don't ansign them tonss and dor , but aather toach them to bon' for themtsdlens tnmendisy ao therterc
    p aor tont to duigp anship, bon't arum tp terple togethe  to donlent tord and ton't atsign the  tanks and tor , but aathe  toach the  to don' for thertndlens tnmendity ao thertert
    p aor tond to duild anshi , don't arum tp terpleptogether to donlett tord and don't atsign the  tonks and dor , but aather toach the  to don' forkthe tndlens tnmendith ao thertert
    g aor tond to build anship, bon't arum tp perpleptogether to bollept ,ord tsd don't ansign them tonks and dor , but aather torch them to bolg forkthemtsdlens tnmendish ao the teat
    g aor tont to fuild atship, bon't arum tp perple together to bollept word asd don't ansign them tonks and dor , but aather toach them to bong for themtndlens tnmendity ao therteac
    g aor tont to fuild atshim, don't arum tp perple together to bonlept tord asd don't ansign them tonks and dor , but aather toach them to bong for themtndlessitnmendity ao the teac
    p aor tond to builp anshi , don't aoum tp people together to bonlept tord asd won't ansign them tonss and wor , but rather toach them to bong for themtsdless tnmendity ro therteac
    p aor tond to build anshi , bon't trum tp perple together to donlent ,ord asd won't tnsign them tonks and dor , but tather toach them th dong for themtsdless tnmendity ro themteac
    p aor tond to build anshi , bon't arum tp perple together to bonlent tord and won't ansign them tonks and dork, but rather toach them to bong for thertndlens tnmendity ro therteac
    g aor tont to build atshim, bon't arum tp perdle together to bonlept tord and don't ansign them tanks and dork, but aather toach them ta bong forkthemtndlets tnmendity ro themteac
    g aor tond to build anshi , bon't arum tp perple together to bonlept tord and don't ansign them tanks and dor , but rather toach them ta dong for themtsdlens tnmendity ro therteat
    g tor tond to build anshi , bon't rrum tp perple together to bo lent tord asd won't rnsign them tanks and doo , but rather toach them to dong for thertsdlens tnmendity ro therteac
    g tor tont to build anshi , bon't arum tp perpleptogether to bonlent word tsd don't ansign them tanks and dork, but rather toach them ta bong forkthemtndlens tnmensity ro the teat
    p tor tont to duild anshi , don't arum tp people together to donlent tord asd don't ansign them tanks and work, but aather toach them ta dong for themtndlens tnmensity ro therteat
    p tor tant to duild anshi , don't arum tp people together to donlent word asd don't ansign them tanks and work, but aather toach them ta dong for themtndless tnmendity ro therteac
    g aor tont to build anshi , dongt arum tp peopke together to donlent word and won't ansign them tanks and wor , but aather toach them ta dong for themtndlens tnmensity ro the teat
    g aor tant to build anship, don't rrum tp people together to bonlent word asd don't rnsign them tanks and dor , but rrther torch phem ta dong for themtndlens tnmensity ro the teac
    g aor tont to build anshi , don't trum tp people together to donlent word and don't tnsign them tonks and dork, but rather toach them to don' for themtndlens tnmensity ro the teac
    g aor tont to duild anshi , don't arum tp people together to dollent word and don't ansign them tonks and dork, but rather toach them to don' forkthe tndlens tnmensity ro therteat
    p aor tant to build anshi , don't arum tp peopleptogether to dollept wood and don't ansign them tonks and work, but rather toach them to dong for themtndless tnmensity ro themteac
    p aor tant to build anshi , bon't arum tp people together ta bollent tord and won't ansign them tanks and work, but rather taach them ta bong for themtndlessitnmensity rf the teac
    g aor tant to build anshi , don't arum tp people together to bonlent word and won't ansign the  tanks and work, bus rather toach the  ta bong forkthe tndlens tnmensity rf therteat
    g aor tont to build anshi , bon't trum tp people togethe  to donlent word and won't tnsign toem tonks and work, but rathem toach them to dong for themtndless tnmensity rf themteac
    p aor tant to build anship, bon't arum tp perple together to bollent word and don't ansign them tonks and work, but rather toach them ta bong for themtndless tnmensity ro therteac
    g aor tant to build anshi , bon't arum up perple together to bonlent word asd won't ansign them tanks and work, bus rather toach them ta bon' forkthertndless tnmensity ro thertert
    g aor tant togbuild asshi , bon't arum tp people togethem to dollent word and won't assign them tonks and wor , but rathem toach them to dong for themtndlens tsmensity rf themteac
    g aor tant to duild asshin, don't arum up people togethem to dollent word tnd don't assign them tanks and dork, but rathe  toach them ta dong forkthemtsdlens tsmensity ro the teac
    g aor tant to duild asship, bon't arum up people togethe  to dollent word and don't assign them tonks and dork, but rathe  thach them th dong forkthemtndlensiasmensity rf themteac
    p aor tant to fuild asshi , bon't arum up people together to dollent word and won't assign them tanss and work, but rather toach them ta dong for themtndlens tsmensity rf therteac
    p aor tent to duild asshi , don't trum up people together to dollent ,ord asd won't tssign them tonks asd work, but rather toach ther togdong forkthe cndlets tsmensity rf therceac
    p a r tont to build asshin, don't arum up people aonether to dorlent tood and ton't assiln toem tonks and work, but rather toach them to dorg for toemtndless tsmensito uf themteat
    g aor tant to duild asship, don't arum up people together to dollent tood asd won't assign ahem tonks and work, bus rather toach them to dorg for themtndless tsmensity uf themteac
    p aor tent to cuild anshi , bon't arum up people together to dollent tood asd don't assign them tanks and dor , but rather thach them th dong f r themtndless tsmensity rf themteac
    m aor tant to duild aashi , don't arum up peopli together to dollent tord and don't ansigndthem tanks tad dork, but rather toach them ta dong for themtndlens tsmensity rf therteac
    m aor tans to cuild asshi , don't trum up people together to bollent ,ord and don't tssign them tonks and dork, bui rather toach themeto dong for themsndless tsmensity rf themseag
    p aor tant do build asship, don't arum up people together to bollent ,ood asd don't askign them tonke and dork, bu  rather toach them to bong for whe tndlens tnmensith uf therteac

## 참고자료

[PyTorch로 시작하는 딥 러닝 입문](https://wikidocs.net/book/2788)

