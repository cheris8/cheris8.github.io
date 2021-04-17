---
title: '[Python] 자료구조 (Data Structure) - 큐 (Queue)'

categories:
  - Python
tags:
  - Python
  - Data Structure

last_modified_at: 2021-03-18T08:06:00-05:00

classes: wide
---

이 글은 자료구조 중 큐에 관한 기록입니다.

## 개념

- 큐(queue) : 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 데이터 구조

![]({{site.url}}/assets/images/PY/queue.png)

- 큐는 FIFO 또는 LILO 데이터 방식을 따릅니다.
    - FIFO(First In First Out) : 가장 먼저 넣은 데이터를 가장 먼저 꺼내는 데이터 관리 정책
    - LILO(Last In Last Out) : 가장 나중에 넣은 데이터를 가장 나중에 꺼내는 데이터 관리 정칙
- 큐는 한쪽 끝에서만 데이터를 넣을 수 있고 다른 한쪽 끝에서만 데이터를 뺄 수 있는 구조를 가집니다.

## 종류

- 선형 큐 (Linear Queue)
- 환형 큐 (Circular Queue)
- 우선순위 큐 (Priority Queue)
<!--
### 1. 선형 큐 (Linear Queue)
### 2. 원형 큐 (Circular Queue)
### 3. 우선순위 큐 (Priority Queue)
-->

## 활용

- 스트리밍(streaming)
- 너비 우선 탐색(breath first search ; bfs)

## Abstract Data Type ; ADT

- `enqueue()` : 맨 뒤에 값을 추가 (큐에 데이터를 넣기)
- `dequeue()` : 맨 앞의 값을 제거 (큐에서 데이터를 꺼내기)
- `peek()` : 맨 앞(`front`)의 값을 출력
- `front()` : 큐의 맨 앞 인덱스
- `rear()` : 큐의 맨 뒤 인덱스
- `isEmpty()` : 큐가 비어 있는지 여부를 반환
    - 선형 큐에서, `front=rear`이면 `True` 반환
-  `isFull()` : 큐가 꽉 차 있는지 여부를 반환
    - 선형 큐에서, `rear=n-1`이면 `True` 반환

## Implementation with Python

1. `list` 사용하기
2. `collections` 모듈의 `deque` 사용하기
3. `queue` 모듈의 `Queue` 사용하기

### 1. `list` 사용하기

```python
class ListQueue(object):
    def __init__(self):
        self.queue = []
    
    def dequeue(self):
        if len(self.queue)==0:
            return -1
        return self.queue.pop(0)
    
    def enqueue(self, x):
        self.queue.append(x)
    
    def printQueue(self):
        print(self.queue)
```

### 2. `collections` 모듈의 `deque` 사용하기

```python
from collections import deque

# 큐 선언
dq = deque([])

# 큐에 데이터를 넣기
dq.append(1)
dq.append(2)
dq.append(3)
dq.append(4)
print(dq)

# 큐에서 데이터를 꺼내기
dq.popleft() # 1
dq.popleft() # 2

```

```python
from collections import deque

class DequeQueue(deque):

    def enqueue(self, x):
        super().append(x)
    
    def dequeue(self):
        super().popleft()
    
    def display(self):
        for node in self.__iter__():
            print(node)
```

### 3. `queue` 모듈의 `Queue` 사용하기

`queue` 모듈의 `Queue` 객체는 다음과 같은 메서드를 갖습니다.

- `queue.qsize()` : `queue`의 크기 반환
- `queue.empty()` : `queue`가 비어 있으면 `True`, 비어 있지 않으면 `False` 반환
- `queue.full()` : `queue`가 꽉 차 있으면 `True`, 꽉 차 있지 않으면 `False` 반환
- `queue.put(item)` : `queue`에 `item`을 추가
- `queue.get()` : `queue`에서 `item`을 제거하고 `item`을 반환 

```python
from queue import Queue

# 큐 선언
q = Queue()

# 큐에 데이터를 넣기
q.put(1)
q.put(2)
q.put(3)
q.put(4)

# 큐에서 데이터를 꺼내기
q.get() # 1
q.get() # 2
```

## 참고자료

[Python Documentation](https://docs.python.org/3/library/queue.html)
