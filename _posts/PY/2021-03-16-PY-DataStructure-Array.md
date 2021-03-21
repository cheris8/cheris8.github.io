---
title: '[Python] 자료구조 (Data Structure) - 배열 (Array)'

categories:
  - Python
tags:
  - Python
  - Data Structure

last_modified_at: 2021-03-16T08:06:00-05:00

classes: wide
---

이 글은 자료구조 중 배열에 관한 기록입니다.

## 개념 및 종류

- 배열(array) : 데이터를 나열하고, 각 데이터를 인덱스에 대응하여, 인덱스로 데이터에 접근할 수 있도록 하는 데이터 구조
    - 파이썬에서는 `list` 자료형이 배열의 기능을 지원합니다.
- 1차원 배열(1D Array) / 2차원 배열(2D Array) / 3차원 배열(3D Array)

![]({{site.url}}/assets/images/PY/array.png)

## 활용

- 같은 종류의 데이터를 효율적으로 관리하기 위해 사용합니다.
- 같은 종류의 데이터를 순차적으로 저장하기 위해 사용합니다.

## 장점 및 단점

- 장점
    - 데이터에 빠르게 접근할 수 있습니다.
- 단점
    - 미리 배열의 최대 길이를 지정해야 합니다.
    - 데이터의 추가 및 삭제가 어렵습니다.

## Implementation with Python

```python
# 1D Array
arr = [1, 2, 3, 4, 5, 6]
print(arr)
```

    [1, 2, 3, 4, 5, 6]

```python
# 2D Array
arr = [[1, 2, 3], [4, 5, 6]] # 3 by 2
print(arr)
print(arr[0])
print(arr[0][2])
```

    [[1, 2, 3], [4, 5, 6]]
    [1, 2, 3]
    3

```python
# 3D Array
arr = [[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]] # 3 by 2 by 2
print(arr)
print(arr[0])
print(arr[0][1])
print(arr[0][1][2])
```

    [[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]]
    [[1, 2, 3], [4, 5, 6]]
    [4, 5, 6]
    6

## 참고자료

[잔재미코딩](https://www.fun-coding.org/Chapter04-array-live.html)
