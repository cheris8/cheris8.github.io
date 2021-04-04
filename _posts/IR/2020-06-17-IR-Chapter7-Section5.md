---
title: '[정보검색] 제7장5절 정보검색 모형 - 확장 불리언 검색'

categories:
  - Informatoin Retrieval
tags:
  - Informatoin Retrieval

last_modified_at: 2020-06-17T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 정영미 교수님의 [정보검색연구](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=17330455)를 바탕으로 연세대학교 문성빈 교수님의 수업을 공부한 기록입니다.

## MMM (Mixed Min and Max) 모형

- 두가지 유형의 검색문에 대해 최소값과 최대값을 모두 이용하여 문헌과 질의의 유사도를 산출

### 검색문
- $Q_{and} = (A1 \ and \ A2 \ and \cdots and \ An)$
- $Q_{or} = (A1 \ or \ A2 \ or \cdots or \ An)$

### 문헌 - 질의 간 유사도
- $SIM(Q_{and}, \ D) = C_{and1} \times \min(d_{A1}, d_{A2}, \cdots , d_{An}) + C_{and2} \times \max(d_{A1}, d_{A2}, \cdots , d_{An})$
- $SIM(Q_{or}, \ D) = C_{or1} \times \max(d_{A1}, d_{A2}, \cdots , d_{An}) + C_{or2} \times \min(d_{A1}, d_{A2}, \cdots, d_{An})$
	- $A1, A2, \cdots , An$ : 용어
	- $d_{A1}, d_{A2}, … , d_{An}$ : 문헌 D가 용어에 대해 갖는 색인어 가중치
	- $C_{and1}, C_{and2}, C_{or1}, C_{or2}$ : 두 불리언 연산자의 적용을 덜 엄격하게 만드는 것 : 연성계수 (Softness Coefficient)
		- $C_{and1} > C_{and2}$ : AND 연산자에 대해서는 색인어 가중치의 최소값에 더 큰 중요도 부여
		- $C_{or1} > C_{or2}$ : OR 연산자에 대해서는 색인어 가중치의 최대값에 더 큰 중요도 부여
		- $C_{and1} + C_{and2} = 1, \ C_{or1} + C_{or2} = 1$

> 예시

$D1 = \\\{ (A, 0.7), (B, 0.5) \\\}$  
$D2 = \\\{ (A, 0.9), (B, 0.1) \\\}$  
$Q_{and} = (A \ and \ B)$  
$C_{and1} = 0.6, \ C_{and2} = 0.4$  
$SIM(Q_{and}, \ D1) = C_{and1} \times \min(0.7, 0.5) + C_{and2} \times \max(0.7, 0.5) = 0.6 \times 0.5 + 0.4 \times 0.7 = 0.58$  
$SIM(Q_{and}, \ D2) = C_{and1} \times \min(0.9, 0.1) + C_{and2} \times \max(0.9, 0.1) = 0.6 \times 0.1 + 0.4 \times 0.9 = 0.42$

## P-norm 모형

- 문헌의 색인어 뿐만 아니라 검색어에도 가중치를 부여할 수 O

### 문헌 및 질의

- $D = (d_{A1}, d_{A2}, \cdots , d_{An})$
- $Q_{andp} = (A1, a1) \ andp \ (A2, a2) andp \cdots andp \ (An, an)$
- $Q_{orp} = (A1, a1) \ orp \ (A2, a2) \ orp \cdots orp \ (An, an)$

### 문헌-질의 간 유사도

- $SIM(Q_{andp}, \ D) = 1 - \sqrt[p]{ \frac{ a_1^p (1-d_{A1})^p + a_2^p (1-d_{A2})^p + \cdots + a_n^p (1-d_{An})^p }{ a_1^p + a_2^p + \cdots + a_n^p } }$
- $SIM(Q_{orp}, \ D) = \sqrt[p]{ \frac{ a_1^p d_{A1}^p + a_2^p d_{A2}^p + \cdots + a_n^p d_{An}^p }{ a_1^p + a_2^p + \cdots + a_n^p } }$
	- $A1, A2, \cdots, An$ : 용어
	- $a_1, a_2, \cdots, a_n$ : 용어의 질의에서의 가중치
	- $d_{A1}, d_{A2}, \cdots, d_{An}$ : 용어의 문헌에서의 가중치
	- $p$ : 불리언 연산자를 얼마나 엄격하게 적용하는가를 의미하는 파라미터
		- $p = 1$ : 내적계수를 사용하는 벡터공간 모형
			- $SIM(Q_{andp}, \ D) = \cdots = SIM(Q_{orp}, \ D) =$ 내적계수
			- 즉 불리언 연산자의 의미 상실
		- $p = \infty$ : 퍼지모형과 유사
			- $SIM(Q_{andp}, \ D) = 1 - \max = \min( d_{A1}, d_{A2}, \cdots, d_{An} )$
			- $SIM(Q_{orp}, \ D) = \max( d_{A1}, d_{A2}, \cdots, d_{An} )$
			- 즉 불리언 연산자의 의미 강화
		- $p = 2$ : 아주 좋은 성능을 보이는 것으로 보고
