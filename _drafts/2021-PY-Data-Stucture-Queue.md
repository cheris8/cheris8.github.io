---
title: '[Python] 자료구조 (Data Structure) - 큐 (Queue)'

categories:
  - Python
tags:
  - Python
  - Data Structure

last_modified_at: 2021-03-15T08:06:00-05:00

classes: wide
---

## 큐의 개념

가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 데이터 구조

FIFO(First-In, First-Out) 또는 LILO(Last-In, Last-Out) 방식으로 스택과 꺼내는 순서가 반대
큐란 목록 한쪽 끝에서만 자료를 넣고 다른 한쪽 끝에서만 자료를 빼낼 수 있는 자료구조입니다. 먼저 집어넣은 데이터가 먼저 나오는 구조로 데이터를 저장합니다. 데이터가 입력한 순서대로 처리되어야 할 경우에 사용합니다 큐에 새로운 데이터가 들어오면 큐의 끝에 데이터가 추가되며(enqueue) 반대로 삭제될 때는 첫번째 위치의 데이터가 삭제됩니다.(dequeue)

큐(queue)는 선입선출, FIFO(First In First Out) 기반의 매우 유명한 자료 구조입니다. 큐를 사용하면 데이터를 추가한 순서대로 제거할 수 있기 때문에 스트리밍(streaming), 너비 우선 탐색(breath first search) 등 소프트웨어 개발에서 널리 응용되고 있습니다.

## 큐의 종류

- 선형 큐
- 환형 큐
- 우선순위 큐

선형 큐는 배열의 마지막 index를 가리키는 변수가 있고, dequeue가 일어날 때마다 비어있는 공간이 생기면서 이를 활용할 수 없게 됩니다. 이 방식을 해결하기 위해 front를 고정시킨 채 뒤에 남아있는 데이터를 앞으로 한칸씩 땡길 수 밖에 없습니다. 이에 대한 대안으로 사용하는 것이 원형큐입니다.

## ADT

- Enqueue: 큐에 데이터를 넣는 기능
- Dequeue: 큐에서 데이터를 꺼내는 기능

## 구현

이번 포스트에서는 파이썬에서 큐 자료 구조를 어떻게 사용할 수 있는지 알아보도록 하겠습니다.

### 1. list

리스트를 사용했을 경우에는 추가(enqueue)의 시간복잡도는 O(1)이며, dequeue(삭제)는 O(N)입니다.
추가의 경우에는 큐의 맨 끝에서만 일어나지만, 큐의 첫번째 원소를 삭제할 경우에는 두번째 원소부터 맨 마지막 원소까지 모든 원소들의 위치를 왼쪽으로 한칸씩 옮겨주어야 하기 때문입니다.

```python
class ListQueue(object):
    def __init__(self):
        self.queue = []
    
    def dequeue(self):
        if len(self.queue)==0:
            return -1
        return self.dequeue.pop(0)
    
    def enqueue(self, x):
        self.queue.append(x)
    
    def printQueue(self):
        print(self.queue)

```

파이썬에서 큐를 사용하는 가장 간단한 방법은 범용 자료 구조인 list를 사용하는 것입니다. list 객체의 pop(0) 함수를 호출하면 첫 번째 데이터를 제거할 수 있습니다.


```python
>>> queue = [4, 5, 6]
>>> queue.append(7)
>>> queue.append(8)
>>> queue
[4, 5, 6, 7, 8]
>>> queue.pop(0)
4
>>> queue.pop(0)
5
>>> queue
[6, 7, 8]
```
이런 방식으로 list를 사용하면 뒤에서 데이터를 추가하고 앞에서 데이터를 제거할 수 있기 때문에 큐 자료 구조를 사용하는 효과가 납니다.

반대 방향으로 큐 자료 구조를 사용하고 싶다면 insert(0, x) 함수를 사용하면 맨 앞에서 데이터를 삽입할 수도 있습니다. 이럴 경우, 데이터가 앞에서 들어가고 뒤로 나오게 됩니다.

```python
>>> queue = [4, 5, 6]
>>> queue.insert(0, 3)
>>> queue.insert(0, 2)
>>> queue
[2, 3, 4, 5, 6]
>>> queue.pop()
6
>>> queue.pop()
5
>>> queue
[2, 3, 4]
```
하지만 이렇게 list를 큐 자료 구조의 효과를 내기 위해서 사용하는 것은 성능 측면에서 추천되지 않습니다. 파이썬의 list는 다른 언어의 배열처럼 무작위 접근(random access)에 최적화된 자료 구조이기 때문에 pop(0)또는 insert(0, x)는 성능적으로 매우 불리한 연산입니다. 이 두 연산은 시간 복잡도는 O(N)이기 때문에 담고 있는 데이터의 개수가 많아질 수록 느려집니다. 왜냐하면 첫 번째 데이터를 제거한 후에는 그 뒤에 있는 모든 데이터를 앞으로 한 칸식 당겨줘야 하고, 맨 앞에서 데이터를 삽입하려면 그 전에 모든 데이터를 뒤로 한 칸씩 밀어줘야 하기 때문입니다. 이렇게 기존에 queue가 담고 있던 모든 데이터를 앞/뒤로 쉬프트(shift)해주지 않으면 queue[i]의 결과값이 정확하지 않을 것입니다. 😆


### 2.deque

deque(데크)는 double-ended queue의 줄임말로, 앞과 뒤 양방향에서 데이터를 처리할 수 있는 자료구조를 의미합니다. 파이썬의 리스트와 유사하지만 collections.deque의 시간복잡도를 확인해보면 앞뒤에서 데이터를 처리하는 속도가 O(1)로 매우 빠른 것을 알 수 있습니다. 이는 내부적으로 doubly 링크드 리스트로 구현되어 있기 대문입니다.

```python
from collections import deque

# deque 선언
dq = deque([])

# deque에 데이터 추가
dq.append(1)
dq.append(2)
dq.append(3)
dq.append(4)
print(dq)

# deque의 첫번째 원소 제거
dq.popleft() # 1
dq.popleft() # 2

```

collections 모듈의 deque는 double-ended queue의 약자로 데이터를 양방향에서 추가하고 제거할 수 있는 자료 구조입니다.

deque는 list에는 없는 popleft()라는 메서드를 제공하는데요. 이 메서드를 사용하면 첫 번째 데이터를 제거할 수 있습니다. 데이터의 흐름은 list 객체의 pop(0) 메서드를 사용할 때 처럼 뒤에서 앞으로 흐르게 됩니다.

### 3. Queue

파이썬의 Queue 모듈에서는 큐, 우선순위 큐, 스택을 제공하고 있습니다. 특히 큐 모듈은 스레드 환경을 고려하여 작성되어 있기 때문에 여러 스레드들이 동시에 큐, 우선순위 큐, 스택과 같은 객체에 접근하여 작업을 수행하여도 정상적으로 동작하는 것을 보장합니다. 여기선 기본적인 queue를 사용하여 쉽게 queue를 활용해 보도록 하겠슷ㅂ니다.

```python
import queue

q = queue.Queue()

q.put(1)
q.put(2)
q.put(3)
q.put(4)

# q에서 첫번째 원소 제거
q.get()
q.get()

from collections import deque

class Queue(deque):

    def enqueue(self, x):
        super().append(x)
    
    def dequeue(self):
        super().popleft()
    
    def display(self):
        for node in self.__iter__():
            print(node)
```

파이썬에서 큐를 사용하는 마지막 방법은 queue 모듈의 Queue 클래스를 사용하는 것입니다. 이 방법은 주로 멀티 쓰레딩(threading) 환경에서 사용되며, 내부적으로 라킹(locking)을 지원하여 여러 개의 쓰레드가 동시에 데이터를 추가하거나 삭제할 수 있습니다.

deque와 달리 방향성이 없기 때문에 데이터 추가와 삭제가 하나의 메서드로 처리됩니다. 따라서, 데이터가 추가하려면 put(x) 메서드를 사용하고, 데이터를 삭제하려면 get() 메서드를 사용합니다.


### linked list 사용
