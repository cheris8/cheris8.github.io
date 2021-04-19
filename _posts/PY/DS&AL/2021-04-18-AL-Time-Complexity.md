---
title: '[알고리즘] 시간 복잡도 (Time Complexity) - 빅오 표기법 (Big-O Notation)'

categories:
  - Python
tags:
  - Algorithm

last_modified_at: 2021-04-18T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 알고리즘의 시간 복잡도 및 빅오 표기법에 관한 기록입니다.

## 알고리즘의 시간 복잡도 (Time Complexity)

- 시간 복잡도(Time Complexity)는 $n$개의 입력 데이터에 대하여 알고리즘이 문제를 해결하는 데에 얼만큼의 시간이 걸리는지를 나타내는 것입니다.
- 일반적으로 시간 복잡도를 나타내기 위해 점근적 표기법(asymptotic notation)을 사용합니다.
  - 점근적 표기법이란 중요하지 않은 상수와 계수들을 제거하여 알고리즘의 실행시간에서 중요한 성장률에 집중하는 방법을 의미합니다.
  - 즉, 점근적이라는 의미는 가장 큰 영향을 주는 항만 계산한다는 의미입니다.
- 점근적 표기법에는 다음과 같은 세가지 종류가 존재합니다.
  - 오메가 표기법 (Big-$\Omega$ Notation)
  - 세타 표기법 (Big-$\theta$ Notation)
  - 빅오 표기법 (Big-$O$ Notation)

## 빅오 표기법 (Big-O Notation)

> Big-O notation is a way of converting the overall steps of an algorithm into algebraic terms, then excluding lower order constants and coefficients that don’t have that big an impact on the overall complexity of the problem.

- 빅오표기법은 알고리즘의 시간 복잡도와 공간 복잡도를 나타내는 데에 사용됩니다.
  - 시간 복잡도는 입력 데이터의 개수에 따라 실행되는 알고리즘의 단계의 수를 의미합니다.
  - 공간 복잡도는 알고리즘이 실행될 때 사용하는 메모리의 양을 의미합니다.

### 대표적인 빅오 표기법 (Big-O Notation)

- $O(1)$ : 상수 시간
  - 알고리즘이 문제를 해결하는 데에 필요한 단계의 수가 오직 한 단계인 경우
  - 즉, 입력 데이터의 개수와 관계없이 처리 시간 혹은 메모리 사용량이 일정
- $O(n)$
  - 알고리즘이 문제를 해결하는 데에 필요한 단계의 수가 입력 데이터의 개수 $n$에 비례하는 경우
  - 즉, 입력 데이터의 개수에 따라 처리 시간 혹은 메모리 사용량이 선형적으로 증가
- $O(n^2)$
  - 알고리즘이 문제를 해결하기 위해 필요한 단계의 수가 입력 데이터의 개수 $n$의 제곱인 경우
  - 정렬 알고리즘
- $O(\log n)$ : 로그 시간
  - 알고리즘이 문제를 해결하는 데에 필요한 단계의 수가 연산마다 특정 요인에 의해 감소하는 경우
- $O(n \log n)$
  - 정렬 알고리즘
- $O(C^n)$ : 지수 시간
  - 알고리즘이 문제를 해결하는 데에 필요한 단계의 수가 주어진 상수값 $C$의 $n$제곱인 경우

|Complexity   |1|10|100|
|-------------|-|--|---|
|$O(1)$       |1|1 |1|
|$O(\log n)$  |0|2 |5|
|$O(n)$       |1|10|100|
|$O(n \log n)$|0|20|461|
|$O(n^2)$     |1|100|10000|
|$O(2^n)$     |1|1024|1267650600228229401496703205376|
|$O(n!)$      |1|3628800|화면에 표시 불가|

### 정렬 알고리즘 별 Big-O 비교

<br>

|Sorting Algorithm|최선|평균|최악|
|--------------|-------------|---------------|--------------|
|Bubble Sort   |$O(n)$       |$O(n^2)$       |$O(n^2)$      |
|Heapsort      |$O(n \log n)$|$O(n \log n)$  |$O(n \log n)$ |
|Insertion Sort|$O(n)$       |$O(n^2)$       |$O(n^2)$      |
|Merge Sort    |$O(n \log n)$|$O(n \log n)$  |$O(n \log n)$ |
|Quick Sort    |$O(n \log n)$|$O(n \log n)$  |$O(n^2)$      |
|Selection Sort|$O(n^2)$     |$O(n^2)$       |$O(n^2)$      |
|Shell Sort    |$O(n)$       |$O(n \log n^2)$|$O(n \log n^2)$|
|Smooth Sort   |$O(n)$       |$O(n \log n)$  |$O(n \log n)$  |

### 자료구조 별 Big-O 비교

<br>

|Data Structure|Search (Average)|Insert (Average)|Delete (Average)|Search (Worst)|Insert (Worst)|Delete (Worst)|
|------------------|-----------|-----------|-----------|-----------|-----------|-----------|
|Sorted Array      |$O(\log n)$|$O(n)$     |$O(n)$     |$O(\log n)$|$O(n)$     |$O(n)$     |
|Array             |$O(n)$     |N/A        |N/A        |$O(n)$     |N/A        |N/A        |
|Linked List       |$O(n)$     |$O(1)$     |$O(1)$     |$O(n)$     |$O(1)$     |$O(1)$     |
|Doubly Linked List|$O(n)$     |$O(1)$     |$O(1)$     |$O(n)$     |$O(1)$     |$O(1)$     |
|Stack             |$O(n)$     |$O(1)$     |$O(1)$     |$O(n)$     |$O(1)$     |$O(1)$     |
|Hash Table        |$O(1)$     |$O(1)$     |$O(1)$     |$O(n)$     |$O(n)$     |$O(n)$     |
|Binary Search Tree|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(n)$     |$O(n)$     |$O(n)$     |
|B-Tree            |$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|
|Red-Black Tree    |$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|
|AVL Tree          |$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|$O(\log n)$|
