---
title: '[정보검색] 제7장 정보검색 모형 - 제4절 퍼지집합 검색'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-16T08:06:00-05:00

classes: wide
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## 퍼지집합 이론 (Fuzzy Set Theory)

- 퍼지집합
    - 부분적인 구성원을 허용하는 집합
- 퍼지집합의 각 구성원에 해당 집합에 속하는 정도를 나타내는 0과 1사이의 값을 부여
    - 1 : 완전한 멤버쉽
    - 0 : 비멤버쉽
- 멤버쉽 함수 (Membership Function)
    - 집합에 속하는 각 구성원의 멤버쉽 정도를 결정하는 표나 규칙

## 퍼지집합 검색

- 합집합 : 구성원 X가 퍼지집합 A와 B에 각각 속하는 정도를 나타내는 두 값 가운데 큰 값 선택
- 교집합 : 구성원 X가 퍼지집합 A와 B에 각각 속하는 정도를 나타내는 두 값 가운데 작은 값 선택
- 차집합
> 예시
- D1 = {(디지털, 0.5), (도서관, 0.6)}
- D2 = {(디지털, 0.7), (도서관, 0.2)}
- D3 = {(디지털, 0.9), (도서관, 0.4)}
- 검색문 = “디지털 AND 도서관”
    - D1 = min(0.5, 0.6) = 0.5 (1순위)
    - D2 = min(0.7, 0.2) = 0.2 (3순위)
    - D3 = min(0.9, 0.4) = 0.4 (2순위)
- 검색문 = “디지털 OR 도서관”
    - D1 = max(0.5, 0.6) = 0.6 (3순위)
    - D2 = max(0.7, 0.2) = 0.7 (2순위)
    - D3 = max(0.9, 0.4) = 0.9 (1순위)
- 검색문 = “NOT 도서관”
    - D1 = 1 - 0.6 = 0.4 (3순위)
    - D2 = 1 - 0.2 = 0.8 (1순위)
    - D3 = 1 - 0.4 = 0.6 (2순위)

## 퍼지집합 검색 장점

- 문헌이 다루고 있는 개념의 중요도를 표현할 수 있으므로 불리언 검색보다 융통성이 있다.
- 검색된 문헌들을 적합성에 따라 순위를 매길 수 있다.

## 퍼지집합 검색 단점

- 색인어에 가중치를 부여해야만 부분 멤버쉽 함수를 적용할 수 있다.
- 검색된 문헌의 순위 부여 능력이 검색문을 구성하는 모든 검색어에 대해 민감하지 못하다.
    - 민감성 문제는 집합연산에서 색인어 가중치 값들 중 AND의 경우 최소값을, OR의 경우 최대값을 선택하기 때문에 발생
    - 예
    - D1 = {(A, 0.5), (B, 0.5)}
    - D2 = {(A, 0.9}, (B, 0.1)}
    - 검색문 = “A AND B”
        - D1 = 0.5 > D2 = 0.1
    - 그러나 이용자가 검색어 A를 더 중요하게 생각하는 경우에는 D2가 오히려 정보요구에 더 적합할 수 O
    - 이는 AND와 OR 연산에서 최소값과 최대값을 모두 반영하는 MMM 모형 혹은 질의어에도 가중치를 부여하는 P-norm 모형을 통해 어느 정도 해결