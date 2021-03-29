---
title: '[데이터 크롤링] 웹 크롤링 with Python'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Data Crawling

last_modified_at: 2021-03-12T08:06:00-05:00

classes: wide
---

이 글은 파이썬을 이용한 정적 크롤링에 관한 기록입니다.

## 1. Import library

## 정적 페이지 vs 동적 페이지

## 정적 크롤링
- `request` : 웹 사이트에 접속, 데이터를 받아오는 역할
- `BeautifulSoup` : 데이터를 HTML로 해석하는 역할

## 동적 크롤링
- Web Driver + Selenium
- Open API : 원하는 기능에 대한 오픈API가 있다면 그것을 사용
- Ajax: 웹클라이언트에서 서버에 데이터를 요청하는 역할

### 1. Selenium
웹 드라이버로는 크롬드라이버와 파이퍼폭스 드라이버가 있는데, 일반적으로 크롬 드라이버를 많이 사용합니다.
셀레니움에 관련된 자세한 내용은 이 곳에서 확인하실 수 있습니다.

### 2. Open AIP
### open api
자신이 보유한 정보나 애플리케이션 등을 타 정보 시스템에서 네트워크를 통하여 활용할 수 있도록 공개하는 것
데이터를 제어할 수 있는 간단하고 직관적인 인터페이스의 제공을 통해 사용자의 참여를 유도하는 사용자 중심의 비즈니스 모델
### url
?부터 query string이 시작
parameter = value로 구성
&로 여러 개를 분리
### JSON (Java Object Notation
텍스트 데이터를 기반
자바 스크립트에서 사용하는 객체 표기 방법을 기반
다양한 소프트웨어와 프로그래밍 언어끼리 데이터 교환할 때 많이 사용
### JSON decoding
JSON 문자열을 Python 데이터 타입(딕셔너리, 리스트, 튜플)로 변경하는 작업
Json.loads() 함수를 사용하여 문자열을 Python 데이터 타입으로 변경
반대작업은 JSON encoding

## Naver Open API
비로그인 방식 OPEN API
네이버의 뉴스, 블로그 및 카페의 글들을 로그인 하지 않고 비로그인 방식으로 데이터를 조회하여 가지고 올 수 있음
HTTP 헤더에 클라이언트 ID와 secret 값만 추가하여 전송하면 데이터를 가져올 수 있음


### 3.Ajax

### Ajax (Asynchronous javascript and XML
자바스크립트를 이용하여 비동기적으로 서버와 브라우저가 데이터를 교환하는 통신 방식

### Ajax의 요청과 처리
웹 브라우저는 XMLHttpRequest 객체를 이용하여 Ajax요청을 보냄
서버는 JSON형태로 요청 온 정보만을 담아 순수한 텍스트로 구성된 데이터 문자열을 반환
이 문자열을 객체화 하기 위해 JSON.parse()를 이용
