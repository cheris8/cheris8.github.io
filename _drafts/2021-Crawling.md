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




● JavaScript는 객체 기반의 스크립트 언어입니다.
○ 스크립트 언어는 기존 소프트웨어를 제어하는 데 사용되며, 간단한 작업을 반복적으로 실행하는 데
적합한 프로그래밍 언어입니다. 한 줄씩 바로 실행이 되는 Python도 스크립트 언어입니다.
○ 스크립트 언어는 인터프리터가 필요합니다. C처럼 컴파일러가 필요한 언어는 컴파일 언어입니다.
○ JavaScript의 경우, 웹 브라우저의 엔진이 인터프리터의 역할을 합니다.
● JavaScript는 HTML 및 CSS와 함께 사용됩니다.
○ HTML은 웹 페이지의 전체 틀을 잡고, CSS는 개별 요소의 디자인을 맡습니다.
○ JavaScript는 사용자와의 상호작용을 통해 웹 페이지에서 보여주는 콘텐츠를 동적으로 제어합니다.



● AJAX는 JavaScript 라이브러리 중 하나이며, ‘Asynchronous Javascript And XML’(비동기 JavaScript 및 XML)의 머리글자입니다.
○ 웹 브라우저를 새로고침하면 웹 페이지가 새로 열리는데, 이렇듯 웹 페이지의 전체 요소가 한꺼번에 바뀌는 것을 ‘동기’ 방식이라고 합니다.
○ AJAX는 웹 서버와 통신할 때 전체 웹 페이지를 새로고침하는 대신, 특정 부분에 사용되는 데이터만 웹 서버로부터 내려받으므로 ‘비동기’ 방식이라고 합니다. 따라서 AJAX는 경제적인 방식이 됩니다.
● AJAX는 HTTP 요청 대신 XHR(XML Http Request) 객체를 사용합니다.
○ AJAX는 웹 서버와의 통신을 통해 XML 및 JSON 형태의 데이터를 주고 받습니다.
 
JSON에 대한 이해
● JSON은 JavaScript Object Notation의 줄임말로, JavaScirpt를 이용하여 데이터를 주고 받을 때 사용되는 교환 형식입니다.
● JSON 또한 XML처럼 사람과 컴퓨터가 인식하기 쉽고 용량도 작은 장점이 있어서 XML을 대체하는 데이터 교환 형식으로 많이 사용됩니다.
● JSON의 형태는 다음과 같습니다.
○ 중괄호{}안에Key:Value형식이반복됩니다.
○ Value가 여러 개이면 대괄호 [ ] 안에 콤마(,)로 연결됩니다.
 