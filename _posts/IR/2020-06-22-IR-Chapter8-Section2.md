---
title: '[정보검색] 제8장 검색 성능 향상 전략 - 제2절 효과적인 질의 작성'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-22T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## 용어절단 (Word Truncation)

- 용어의 일부분을 생략하고 나머지 부분만을 검색어로 사용하는 방법
- 키워드 형태의 검색어 입력 시 사용 가능
- 용어절단 결과 입력된 검색어와 일치하는 부분을 갖는 모든 용어들이 색인파일로부터 선택되어 검색어로 자동 추가
- 추가된 모든 용어들을 OR로 조합하여 검색문을 작성한 것과 같은 효과

### 무제한절단 vs. 제한절단

- 절단하는 글자 수의 제한 여부에 따라 무제한절단과 제한절단으로 구분
- 무제한절단 (Unlimited/Open Truncation)
  - 절단되는 글자 수에 제한이 없는 방법
  - 절단된 위치에 절단 표시부호 삽입
  - 절단되는 용어에는 복합어도 포함될 수 O
- 제한절단 (Limited/Restricted Truncation)
  - 절단되는 글자 수를 지정하는 방법
  - 절단 표시로는 무제한절단과는 다른 부호를 사용하거나 무제한절단 부호를 절단 글자 수만큼 반복 사용

### 좌측절단 vs. 우측절단 vs. 내부절단

- 절단되는 부분의 위치에 따라 좌측절단과 우측절단과 내부절단으로 구분
- 좌측절단 (Left Truncation)
  - 검색어의 앞부분을 절단하는 것
  - 여러 다른 접두어를 갖는 용어들을 검색어로 사용할 때 유용
- 우측절단 (Right Truncation)
  - 검색어의 뒷부분을 절단하는 것
  - 단수/복수 용어들을 한꺼번에 검색어로 사용할 때 유용
  - 다양한 어미를 갖는 용어들을 검색어로 사용할 때 유용
  - 가장 일반적으로 사용
- 내부절단 (Internal Truncation)
  - 용어의 중간에 오는 글자를 절단 부호로 대신하는 것

## 인접검색 (Proximity Searching)

- 검색어들이 하나의 필드나 문장 속에 인접하여 출현한 경우의 문헌을 검색하고자 할 때 사용
- 검색문을 구성하는 검색어들은 인접 관계를 나타내는 인접연산자(proximity operator)에 의해 결합
- 문헌 텍스트에서 두 용어가 인접해서 출현한다는 것은 그만큼 주제적으로 관련성이 크다는 것을 의미
- 따라서 인접검색은 두개의 용어가 한 문헌에 함께 출현하였더라도 주제적으로 서로 관련성이 없는 위치에 있을 경우 해당 문헌이 검색될 확률을 감소시켜줌으로써 결과적으로 정확률을 높이는 효과
- 인접검색 기능을 제공하는 검색 시스템에서는 적절하게 구축된 도치색인을 통해 문장이나 문단 등의 단락을 검색할 수 O
- 인접검색은 특정한 단어구를 포함한 문헌을 찾는 데에도 유용하게 사용

### Dialog

- W
  - 두 용어가 같은 순서로 인접해서 출현한 경우를 지시하는 연산자
  - solar(W)energy
  - solar(3W)energy : 중간에 최대 세 단어가 올 수 있음을 지시
- N
  - 두 용어가 순서에 상관없이 서로 인접해서 출현한 경우를 지시하는 연산자
- S
  - 두 용어가 동일한 서브필드(예: 하나의 문단)에 출현한 경우를 지시하는 연산자
- L
  - 같은 디스크립터 안에 출현한 경우를 지시하는 연산자

### CSA

- WITHIN n
  - 두 용어가 순서에 상관없이 인접해서 서로 n단어 이내에 출현한 경우를 지시하는 연산자
- NEAR
  - 두 용어가 순서에 상관없이 인접해서 서로 10단어 이내에 출현한 경우를 지시하는 연산자
- BEFORE / AFTER
  - 인접성은 나타내지 않고 상대적인 순서만을 지시하는 연산자

## 제한검색 (Limited Searching)

- 필드 지정 검색 (Fielded Search)
  - 특정한 필드만을 탐색 대상 필드로 지정
  - 검색어가 표제, 초록, 디스크립터 등 특정한 필드에 출현한 경우에만 검색
  - 사용되는 필드코드나 필드지정 형식은 검색 시스템에 따라 다양
- 조건 검색
  - 연도, 언어, 문헌 유형 등의 검색 조건을 지정
  - 언어나 문헌 유형 등은 보통 초기 검색화면에서 선택
