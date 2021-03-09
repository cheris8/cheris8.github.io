---
title: '[Python] 2차원 리스트를 1차원 리스트로 변환하기'

categories:
  - Data Analysis
tags:
  - Python
  - Data Analysis
  - Data Preprocessing

last_modified_at: 2021-03-03T08:06:00-05:00

classes: wide
---

이 글은 2차원 리스트(이중 리스트)를 1차원 리스트로 변환하는 방법에 관한 기록입니다.

- list comprehension 사용하기
- `sum()` 함수 사용하기
- `itertools`와 unpacking 사용하기
- `itertools`의 `from_iterable()` 메소드 사용하기
- `functools`의 `reduce()` 함수 사용하기
- `functools`의 `reduce()` 함수와 `operator`의 `add()` 함수 사용하기
- `numpy`의 `flatten()` 메소드 사용하기 (바깥쪽 리스트의 각 원소 즉 안쪽 리스트의 길이가 모두 동일한 경우에만 사용 가능)

## Code

```python
ls = [['A', 'B'], ['C', 'D'], ['E', 'F']]
```

### 1. list Comprehension 사용하기

```python
[el for ar in ls for el in ar]
```

    ['A', 'B', 'C', 'D', 'E', 'F']

### 2. `sum()` 함수 사용하기

```python
sum(ls, [])
```

    ['A', 'B', 'C', 'D', 'E', 'F']

### 3. `itertools`와 unpacking 사용하기

```python
import itertools

list(itertools.chain(*ls))
```

    ['A', 'B', 'C', 'D', 'E', 'F']

### 4. `itertools`의 `from_iterable()` 메소드 사용하기

```python
import itertools

list(itertools.chain.from_iterable(ls))
```

    ['A', 'B', 'C', 'D', 'E', 'F']

### 5. `functools`의 `reduce()` 함수 사용하기

```python
from functools import reduce

list(reduce(lambda x, y: x+y, ls))
```

    ['A', 'B', 'C', 'D', 'E', 'F']

## 6. `functools`의 `reduce()` 함수와 `operator`의 `add()` 함수 사용하기

```python
from functools import reduce
from operator import add

list(reduce(add, ls))
```

    ['A', 'B', 'C', 'D', 'E', 'F']

## 7. `numpy`의 `flatten()` 메소드 사용하기

```python
import numpy as np

np.array(ls).flatten().tolist()
```

    ['A', 'B', 'C', 'D', 'E', 'F']

