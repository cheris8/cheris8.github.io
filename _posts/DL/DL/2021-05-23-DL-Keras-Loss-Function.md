---
title: '[딥러닝] 케라스 손실 함수 (Loss Function)'

categories:
  - Deep Learning
tags:
  - Deep Learning
  - Keras

last_modified_at: 2021-05-23T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 케라스(Keras)에서 제공하는 손실 함수(Loss function)에 관한 기록입니다.

```python
import tensorflow as tf

from tensorflow.keras import layers
from tensorflow.keras import losses

```

## 손실 함수의 종류

|Problem type|Last-layer activation|Loss function|Example|
|------------|---------------------|-------------|-------|
|Binary classification|sigmoid|binary_crossentropy|Dog vs cat, Sentiemnt analysis(pos/neg)|
|Multi-class, single-label classification|softmax|categorical_crossentropy|MNIST has 10 classes single label (one prediction is one digit)|
|Multi-class, multi-label classification|sigmoid|binary_crossentropy|News tags classification, one blog can have multiple tags|
|Regression to arbitrary values|None|mse|Predict house price(an integer/float point)|
|Regression to values between 0 and 1|sigmoid|mse or binary_crossentropy|Engine health assessment where 0 is broken, 1 is new

## 1. Binary Crossentropy

- Binary classification 즉 클래스가 2개인 이진 분류 문제에서 사용
- label이 0 또는 1을 값으로 가질 때 사용
- 모델의 마지막 layer는 sigmoid여야 함

```python
# API

tf.keras.losses.BinaryCrossentropy(
    from_logits=False, label_smoothing=0, reduction=losses_utils.ReductionV2.AUTO,
    name='binary_crossentropy'
)

tf.keras.losses.binary_crossentropy(
    y_true, y_pred, from_logits=False, label_smoothing=0
)
```

```python
# Usage

model.add(layers.Dense(1, activation='sigmoid'))

model.compile(loss=losses.BinaryCrossentropy(from_logits=True),
              optimizer='adam', 
              metrics=['accuracy'])

model.compile(loss=losses.binary_crossentropy, 
              optimizer='adam', 
              metrics=['accuracy'])

model.compile(loss='binary_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])
```

## 2. Categorical Crossentropy

- Multi-class classification 즉 클래스가 여러 개인 다중 클래스 분류 문제에서 사용
- label이 원-핫 인코딩 된 형태 즉 label이 class를 나타내는 one-hot vector를 값으로 가질 때 사용
- 예를 들어, 3-class classification 문제에서
  - label이 [1, 0, 0] 혹은 [0, 1, 0] 혹은 [0, 0, 1]을 값으로 가질 때 사용
- 모델의 마지막 레이어는 softmax

```python
# API

tf.keras.losses.CategoricalCrossentropy(
    from_logits=False, label_smoothing=0, reduction=losses_utils.ReductionV2.AUTO,
    name='categorical_crossentropy'
)

tf.keras.losses.categorical_crossentropy(
    y_true, y_pred, from_logits=False, label_smoothing=0
)
```

```python
# Usage

model.add(layers.Dense(num_categories, activation='softmax'))

model.compile(loss=losses.CategoricalCrossentropy(),
              optimizer='adam', 
              metrics=['accuracy'])

model.compile(loss=losses.categorical_crossentropy, 
              optimizer='adam', 
              metrics=['accuracy'])

model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])
```

## 3. Sparse Categorical Crossentropy

- Multi-class classification 즉 클래스가 여러개인 다중 분류 문제에서 사용
- label이 정수 인코딩 된 형태 즉 label이 class index를 값으로 가질 때 사용
- 예를 들어, 3-class classification 문제에서
  - label이 0 혹은 1 혹은 2를 값으로 가질 때 사용
- 모델의 마지막 레이어는 softmax

```python
# API

tf.keras.losses.SparseCategoricalCrossentropy(
    from_logits=False, reduction=losses_utils.ReductionV2.AUTO,
    name='sparse_categorical_crossentropy'
)

tf.keras.losses.sparse_categorical_crossentropy(
    y_true, y_pred, from_logits=False, axis=-1
)
```

```python
# Usage

model.add(tf.keras.layers.Dense(num_categories, activation='softmax'))

model.compile(loss=tf.keras.losses.SparseCategoricalCrossentropy()
              optimizer='adam',
              metrics=['accuracy'])

model.compile(loss=tf.keras.losses.sparse_categorical_crossentropy()
              optimizer='adam',
              metrics=['accuracy'])

model.compile(loss='sparse_categorical_crossentropy', 
              optimizer='adam',
              metrics=['accuracy'])
```
