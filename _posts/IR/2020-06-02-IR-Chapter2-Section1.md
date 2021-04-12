---
title: '[정보검색] 제2장 색인 및 시소러스 - 제1절 색인 개요'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-02T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## 1. 색인 개요

### 색인 정의

- 색인 (Indexing) : 개개의 정보자료의 특성을 표현하는 데이터 요소를 추출하여 각 정보자료를 표현하는 작업
- 색인어 (Index Term) = 메타데이터 : 색인 결과 추출된 데이터 요소

## 색인 종류

- 어떠한 유형의 데이터 요소를 표현하는지에 따라
    - 주제색인어
        - 정보자료의 주제를 나타내는 색인어
        - 키워드, 디스크립터 / 주제명
    - 비주제색인어
        - 정보자료의 주제를 직접적으로 표현하지 않는 색인어
        - 저자명, 기관명, 출판년, 언어 등
- 검색작업에서는 주제색인어와 비주제색인어 모두 검색어로 이용

### 색인어

- 온라인 데이터베이스
    - 데이터베이스 레코드를 구성하는 거의 모든 필드의 데이터로부터 색인어를 추출하여 검색어로 사용
    - 즉 데이터베이스 검색 시 표제, 초록, 디스크립터 필드에 출현한 키워드를 비롯하여 저자명, 디스크립터, 회의명, 저널명, 출판물 유형 등이 검색어로 사용
- 온라인 목록
    - 온라인 목록의 서지 데이터 항목
- 메타데이터
    - 전자적 정보자원의 특성을 기술하기 위해 정의되는 메타데이터 요소
- 주제색인어는 사람의 지적 작업이나 자동색인 프로그램에 의해 선정
- 웹 검색엔진을 포함한 거의 모든 정보검색 시스템
    - 표제, 초록/요약, 또는 전문으로부터 자동색인 과정에 의해 키워드를 색인어로 추출
- 대부분의 온라인 데이터베이스나 온라인 목록
    - 키워드 이외에 시소러스나 주제명표로부터 색인전문가가 선정한 디스크립터나 주제어를 추가적인 주제색인어로 수록

## 2. Title-Term Indexing

- 색인어가 표제인 것
- 표제는 주제를 굉장히 명확하게 표현
- 따라서 title-term을 이용할 경우, 검색 문헌은 소수 but 검색된 적합 문헌은 다수
    - 높은 정확률
    - 낮은 재현율
- title-term을 이용할 경우의 효율성
    - Terminalogical Consistency = $\frac{1}{n}$
    - 개념의 개수 = $1$
    - 하나의 개념을 표현하는 단어의 수 = $n$
    - Terminological Consistency가 높을수록 Title-Term Indexing의 효율성 증가
- title-term을 이용할 경우의 효율성은 주제분야마다 다를 수 O
    - 효율성이 높은 주제분야 : hard science (이공계열)
    - 효율성이 낮은 주제분야 soft science (인문사회계열)

## 3. 자연언어 색인과 통제언어 색인

- 색인어 선택 시 용어에 통제가 가해졌는지 여부에 따라
- 자연언어 색인과 통제언어 색인으로 구분

### 자연언어 (Natural Language) 색인

- 색인어 선택 시 용어에 통제 X
- 자연언어 중 불용어(stop word)를 제거한 전부를 색인어로 사용
- 자동색인 기법에 의해 텍스트에 나타난 형태 그대로의 용어를 색인어로 채택
- 색인전문가가 임의로 색인어를 선택
- 자연 언어 형태의 색인어 : 키워드
- 용어 색인 (Term Indexing)
    - 색인 대상이 개념이라기보다는 텍스트에 출현한 용어이기 때문
- 검색 시 하나의 특정 용어를 검색어로 사용했을 때 질의와 관련된 모든 정보자료를 찾아낼 수 X
    - 동일한 개념이라도 색인하고자 하는 여러 텍스트에서 서로 다른 용어로 표현되어 있을 경우 각각 다른 색인어가 선택되기 때문
    - 같은 어근/어간을 갖는 용어들이라도 형태가 다를 경우 각기 다른 용어로 간주되기 때문
- 이용자는 용어절단 검색 기법, 동의어/유의어 사전을 이용하여 검색효율을 높일 수 O

### 통제언어 (Controlled Language) 색인

- 색인어 선택 시 용어에 통제 O
- 통제어휘(controlled vocabulary)를 참조하여 동일한 개념은 항상 하나의 색인어로 표현
- 통제어휘 (Controlled Vocabulary)
    - 시소러스, 주제명표 등
    - 색인자와 이용자의 개념을 하나의 단어로 통일시켜주는 역할
- 시소러스를 통제어휘로 사용하는 경우의 색인어 : 디스크립터
- 주제명표를 통제어휘로 사용하는 경우의 색인어 : 주제명
- 개념 색인 (Concept Indexing)
    - 색인 대상이 각 개념이기 때문
- 검색 시 특정 용어를 검색어로 사용했을 때 질의와 관련된 모든 정보자료를 검색할 수 O
    - 특정한 개념은 항상 같은 용어에 의해 색인이 가능하기 때문
- 검색어로 통제어를 사용하는 경우 이용자가 적절한 검색어 즉 디스크립터를 선정할 수 있도록 온라인 시소러스 제공
- 검색어로 자연어를 입력한 경우 에는 컴퓨터에 내장된 사전파일을 이용하여 해당되는 통제 색인어로 자동 변환한 후 검색어로 사용