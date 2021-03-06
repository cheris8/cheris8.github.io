---
title: '[정보검색] 제7장 정보검색 모형 - 제1절 검색 모형의 분류'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-01T08:06:00-05:00

classes: wide
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

### 정보검색 시스템

- 기본적으로 입력된 질의에 대해 각 문헌과 질의 간의 유사도(similarity)를 측정한 다음 유사도가 일정한 기준을 넘는 문헌들을 검색
- 시스템이 채택한 검색 모형에 따라 질의의 표현 방법과 질의-문헌 간 유사도를 측정하는 방법이 상이
- 예시) 불리언 검색 모형
  - 검색문/질의는 검색어/질의어와 검색논리를 표현하는 불리언 연산자로 구성
  - 질의와 문헌의 유사도는 검색문을 완전히 만족시키는 경우에는 1, 그 이외의 경우에는 0
  - 따라서 유사도가 1인 문헌들만 검색

### 검색 모형 종류

- 집합이론적(set theoretic) 모형
  - [불리언 검색 (Boolean Retrieval) 모형]({{site.url}}/informatoin%20retrieval/IR-Chapter7-Section3/)
  - [퍼지집합 검색 (Fuzzy Set Retrieval) 모형]({{site.url}}/informatoin%20retrieval/IR-Chapter7-Section4/)
  - [확장 불리언 검색 (Extended Boolean Retrieval)]({{site.url}}/informatoin%20retrieval/IR-Chapter7-Section5/)
- 대수적(algebraic) 모형
  - [벡터공간 검색 (Vector Space Retrieval) 모형]({{site.url}}/informatoin%20retrieval/IR-Chapter7-Section6/)
- 확률적(probabilistic) 모형
  - [확률 검색 (Probabilistic Retrieval) 모형]({{site.url}}/informatoin%20retrieval/IR-Chapter7-Section8/)
