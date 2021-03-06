---
title: '[정보검색] 제2장 색인 및 시소러스 - 제2절 인용색인'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-03T08:06:00-05:00

classes: wide
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## 1. 인용색인 (Citation Index) 개념

- 문헌 말미에 수록된 참고문헌을 이용하여 유사한 주제의 문헌들을 묶어주는 기법
    - 간접적인 주제색인
- 인용색인에서는 인용 문헌과 피인용 문헌의 인용 사항이 색인어로서 역할
- 인용색인의 가장 기본적인 가설
    - 인용한 문헌과 인용된 문헌이 내용에 있어 관련이 있다는 것
    - 즉 한 문헌의 저자가 이전의 다른 문헌을 인용하는 이유는 두 문헌의 내용이 서로 관련이 있기 때문
    - 따라서 인용한 문헌과 인용된 문헌은 서로 유사한 주제를 다루고 있을 가능성 high
- 웹 문서의 하이퍼링크들도 관련된 다른 웹 문서나 사이트로 연결시켜주는 인용링크 (citation link)
- 인용링크를 일종의 색인어로 사용 가능
- 인용색인은 연구대상이 디는 분야가 다학문적 주제분야, 학제적 주제분야, 융합적 분야 또는 미성숙분야일 때 효과적인 검색도구

### SCI / SSCI / A&HCI

- ISI (Institute of Scientific Information) 사가 제작
    - SCI (Science Citation Index) : 과학기술 분야의 인용색인
    - SSCI (Social Science Citation Index) : 사회과학 분야의 인용색인
    - A&HCI (Arts & Hmanities Citation Index) : 인문학 분야의 인용색인
- 웹 데이터베이스로 제공 : Web of Science
- Related Records 검색 기능 제공
    - 검색한 논문과 서지적으로 결합되어 있는 다른 논문들을 검색해내는 기능
    - 특정한 논문을 인용한 다른 논문들을 찾은 후 각 인용문헌(citing document)의 완전 레코드(full record)를 출력하면 화면상에 ‘Related Records’ 버튼이 나타나며
    - 이 버튼을 선택할 경우 방금 검색된 논문과 공통된 피인용문헌을 하나이상 갖는 다른 논문들의 리스트가 출력

## 2. 인용색인의 장애 요소

- 손쉽게 구할 수 있는 자료이기 때문에 인용하는 경우 존재
- 두 문헌이 제3의 같은 문헌을 인용하는 경우 인용된 문헌의 같은 부분 즉 동일한 정보를 인용하는 것인지는 알 수 X
- 자신이 쓴 이전의 문헌을 과다하게 인용하는 자기인용과 권위있는 문헌의 무조건 인용 등
- 전혀 인용되지 않는 문헌은 인용문헌망에 나타나지 않기 때문에 인용색인만을 사용할 경우 관련 문헌을 완전히 검색해낼 수는 X
- 영어권의 출판물에 집중

## 3. 서지결합 기법과 동시인용 분석 기법

- 인용문헌을 이용하여 문헌들 간 주제적 관계를 설정하는 기법
- 문헌의 인용패턴에 근거하여 문헌들 간의 관계를 측정
- 일차적으로 검색된 문헌과 관련된 다른 문헌들을 검색하기 위한 기준으로 사용
- 유사한 문헌의 군집화(clustering)를 위한 기준으로 사용

### 서지결합 기법 (Bibliographic Coupling)

- 여러 개의 문헌이 공통되는 인용 문헌을 하나 이상 가지고 있을 때 서로 주제적으로 관련되어 있다고 보는 기법
- 이때 이 문헌들은 서지적으로 결합되어 있다고 표현
- 서지결합도
    - 서지적으로 결합된 문헌들 간 결합 강도
    - 공통으로 인용된 문헌 수
    - 따라서 서지결합도가 높을수록 두 문헌의 주제는 유사
    - 정적

### 동시인용 분석 기법 (Co-Citation Analysis)

- 두 문헌이 후에 출판된 제 3의 문헌 속에서 동시에 인용되었을 때 이 두 문헌이 주제적으로 관련이 있다고 보는 기법
- 동시인용도
    - 관련성의 정도
    - 동시인용 횟수
    - 동적 (고정 X)
    - 이미 출판된 두 논문이 새로 출판되는 논문 속에서 계속 함께 인용될 수 있기 때문
