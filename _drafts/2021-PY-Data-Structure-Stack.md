---
title: '[Python] 자료구조 (Data Structure) - 스택 (Stack)'

categories:
  - Python
tags:
  - Python
  - Data Structure

last_modified_at: 2021-03-15T08:06:00-05:00

classes: wide
---

이 글은 자료구조 중 배열에 관한 기록입니다.

## 개념

- 스택(stack) : 가장 나중에 넣은 데이터를 가장 먼저 꺼낼 수 있는 데이터 구조

![]({{site.url}}/assets/images/PY/stack.png)

- 스택은 LIFO 또는 FILO 데이터 관리 방식을 따릅니다.
    - LIFO (Last In First Out) : 가장 나중에 넣은 데이터를 가장 먼저 꺼내는 데이터 관리 정책
    - FILO (First In Last Out) : 가장 먼저 넣은 데이터를 가장 나중에 꺼내는 데이터 관리 정책
- 스택은 한쪽 끝에서만 데이터를 넣거나 뺄 수 있어 데이터에 제한적으로 접근할 수 있는 구조를 가집니다.
    - 즉 스택의 맨 위에서만 데이터의 추가 및 삭제가 이루어집니다.

## 활용

- 컴퓨터 내부의 프로세스 구조의 함수 동작 방식
- 함수의 콜스택
    - 콜스택은 함수의 호출을 기록하는 자료구조다. 만약 우리가 어떤 함수를 실행시킨다면, 우리는 스택 위에 무언가를 올리는(push) 행위를 하게 되고, 함수로부터 반환을 받을 때, 스택의 맨 위를 가져오게(pop)되는 것이다.
- 문자열 역순 출력

## 장점 및 단점

- 장점
    - 구조가 단순해서 구현이 쉽다
    - 데이터 저장/읽기 속도가 빠르다
- 단점
    - 데이터 최대 갯수를 미리 정해야 한다
        - 파이썬의 경우 재귀 함수는 1000번까지만 호출이 가능함
    - 저장 공간의 낭비가 발생할 수 있다
        - 미리 최대 갯수만큼 저장 공간을 확보해야 함

스택의 장점은 참조 지역성이 높다는 것이다. 한번 참조된 곳은 다시 참조될 확률이 높다. 
단점은 데이터를 탐색하기 어렵는 것이다

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
    
    # 스택이 비어 있는지 여부 반환하는 메소드
    def isEmpty(self):
        return len(self.stack)==0
    
    # 스택에 해당 값이 포함되어 있는지 여부를 반환하는 메소드
    def isContain(self, x):
        return x in self.stack
```

```python
class Stack:
    #리스트를 이용하여 스택 생성
    def __init__ (self):
        self.top = []
    #스택의 크기를 출력
    def __len__(self):
        return len(self.top)

    #스택 내부 자료를 string으로 변환하여 반환
    def __str__(self):
        return str(self.top[::1])
    #PUSH
    def push (self, item):
        self.top.append(item)
    #POP
    def pop(self):
        #if Stack is not empty
        if not self.isEmpty():
            #pop and return 
            return self.top.pop(-1)
        else:
            print("Stack underflow")
            exit()
    #스택 초기화
    def clear(self):
        self.top=[]
    #자료가 포함되어 있는지 여부 반환
    def isContain(self, item):
        return item in self.top
    #스택에서 top의 값을 읽어온다
    def peek(self):
        if not self.isEmpty():
            return self.top[-1]
        else:
            print("underflow")
            exit()
    #스택이 비어있는지 확인
    def isEmpty(self):
        return len(self.top)==0

    #스택 크기 반환
    def size(self):
        return len(self.top)
```


