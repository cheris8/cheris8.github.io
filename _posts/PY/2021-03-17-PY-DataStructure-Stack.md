---
title: '[Python] 자료구조 (Data Structure) - 스택 (Stack)'

categories:
  - Python
tags:
  - Python
  - Data Structure

last_modified_at: 2021-03-17T08:06:00-05:00

classes: wide
---

이 글은 자료구조 중 스택에 관한 기록입니다.

## 개념

- 스택(stack) : 가장 나중에 넣은 데이터를 가장 먼저 꺼낼 수 있는 데이터 구조

![]({{site.url}}/assets/images/PY/stack.png)

- 스택은 LIFO 또는 FILO 데이터 관리 방식을 따릅니다.
    - LIFO (Last In First Out) : 가장 나중에 넣은 데이터를 가장 먼저 꺼내는 데이터 관리 정책
    - FILO (First In Last Out) : 가장 먼저 넣은 데이터를 가장 나중에 꺼내는 데이터 관리 정책
- 스택은 한쪽 끝에서만 데이터를 넣거나 뺄 수 있어 데이터에 제한적으로 접근할 수 있는 구조를 가집니다.
    - 즉 스택의 맨 위에서만 데이터의 추가 및 삭제가 이루어집니다.

## 활용

- 함수의 콜스택
- 문자열 역순 출력

## 장점 및 단점

- 장점
    - 구조가 단순해서 구현이 쉽습니다.
    - 데이터를 읽고 저장하는 속도가 빠릅니다.
- 단점
    - 데이터를 탐색하기 어렵습니다.
    - 데이터의 최대 개수를 미리 정해야 합니다.
    - 저장 공간의 낭비가 발생할 수 있습니다.
        - 미리 데이터의 최대 개수만큼 저장 공간을 확보해야 하기 때문입니다.

## Abstract Data Type ; ADT

- `push()` : 맨 위에 값을 추가 (스택에 데이터를 넣기)
- `pop()` : 맨 위의 값을 제거 (스택에서 데이터를 꺼내기)
- `peek()` : 맨 위(맨 마지막)의 값을 출력 (스택 변형 X)
- `clear()` : 스택을 초기화
- `isEmpty()` : 스택이 비어 있는지 여부를 반환
    - 스택이 비어 있으면 `True`, 비어 있지 않으면 `False` 반환
- `isContain()` : 스택에 해당 값이 포함되어 있는지 여부를 반환
    - 스택에 해당 값이 포함되어 있으면 `True`, 포함되어 있지 않으면 `False` 반환

## Implementation

```python
class Stack:
    def __init__(self):
        self.stack = []
    
    # 맨 위에 값을 추가하는 메소드
    def push(self, x):
        self.items.append(x)
    
    # 맨 위의 값을 제거하는 메소드
    def pop(self):
        if not self.isEmpty():
            return self.stack.pop(-1)
        else:
            print('Stack is Empty')
            exit()

    # 맨 위의 값을 출력하는 메소드
    def peek(self):
        if not self.isEmpty():
            return self.stack[-1]
        else:
            print('Stack is Empty')
            exit()
    
    # 스택을 초기화 하는 메소드
    def clear(self):
        self.stack = []
    
    # 스택이 비어 있는지 여부를 반환하는 메소드
    def isEmpty(self):
        return len(self.stack)==0
    
    # 스택에 해당 값이 포함되어 있는지 여부를 반환하는 메소드
    def isContain(self, x):
        return x in self.stack
```

