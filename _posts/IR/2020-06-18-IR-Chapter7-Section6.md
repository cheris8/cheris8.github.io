---
title: '[정보검색] 제7장 정보검색 모형 - 제6절 벡터공간 검색'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-18T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## 벡터공간 모형 (Vector Space Model: VSM)

- 문헌과 질의를 각각 용어 벡터 형태로 표현한 다음 두 벡터 간의 유사도를 산출하여 검색문헌들을 순위화하는 검색모형

### 문헌 및 질의

- $N$차원 벡터로 표현
	- $N$ : 문헌 데이터베이스 내 용어의 수
- 문헌벡터와 질의벡터는 각각의 색인어와 질의어의 가중치 값으로 구성
### 문헌-질의 유사도

- 질의벡터와 가장 가까운 문헌벡터가 가장 유사도가 높다고 판단
- 문헌벡터 $D_i$와 질의벡터 $Q$ 사이의 각 $\theta_i$의 코사인 값으로 측정
- 코사인 유사계수 공식 사용

