---
title: '[데이터 크롤링] 웹 크롤링'

categories:
  - Data Analysis
tags:
  - Data Crawling

last_modified_at: 2021-03-11T08:06:00-05:00

classes: wide
---

이 글은 웹 크롤링에 관한 기록입니다.

## 1. 크롤링 개념

웹 크롤링(web crawling) 혹은 웹 스크래핑(web scraping)은 웹 페이지에 존재하는 내용들을 수집하는 것을 말합니다. 이에 사용되는 것을 웹 크롤러(web crawler)라고 합니다. 웹 크롤러는 웹 스파이더(web spider), 웹 로봇(web robot), 봇(bots) 등으로도 불립니다.

### HTTP (HyperText Transfer Protocol)

HTTP는 클라이언트/웹 브라우저와 웹 서버가 인터넷 상에서 데이터를 주고받을 때 사용됩니다. 보통 HTML, JSON 등을 주고받습니다.

클라이언트는 웹 브라우저를 활용하여 웹 서버에 데이터를 요청(Request)하고, 웹 서버는 해당 요청에 대한 결과를 응답(Response)합니다. 클라이언트가 데이터를 요청하는 방식(Method)에는 크게 GET 방식과 POST 방식이 존재합니다.

### HTTP 요청 (HTTP Request)

- 클라이언트가 웹 서버에 HTTP 요청(Request)을 할 때 웹 서버에 제공해야 하는 **요청 메시지**는 요청 방식에 따라 달라집니다.
  - GET 방식 :  요청 라인, 요청 헤더 
  - POST 방식 :  요청 라인, 요청 헤더, 메시지 바디
- **요청 라인**
  - 요청 방식 (GET or POST), 경로 (URI) 등
- **요청 헤더**
  - 가능한 콘텐츠 형식(Content-Type), 가능한 인코딩 방식 (Character set), 인증 스펙 (Authorization), Cookies, User-agent, Referer 등
- **메시지 바디**
  - URI에 포함된 파라미터

### HTTP 응답 (HTTP Response)

- 웹 서버는 클라이언트의 요청(Request)에 대하여 **응답 메시지**로 응답(Response)합니다.
- **응답 메시지**는 **응답 헤더**와 **응답 바디**로 구성되어 있습니다.
  - **응답 헤더** : HTTP 버전, 상태코드, 일시, 콘텐츠 형태, 인코딩 방식, 크기 등
  - **응답 바디** : HTML

_참고)_

|상태코드|내용|
|------|---|
|1XX|정보 교환|
|2XX|데이터 전송 성공 즉 수락|
|3XX|server-side redirect|
|4XX|클라이언트 오류|
|5XX|서버 오류|

### URI vs. URL

- URI(Uniform Resource Indicator) : 리소스를 식별하는 문자열들을 차례대로 배열한 것
- URL(Uniform Resource Locator) : 리소스가 포함되어 있는 위치
- 즉 URI가 URL을 포괄하는 개념입니다.
- 예시) `https://search.naver.com/search.naver?ie=utf8&query=아이유`
  - `https://search.naver.com/search.naver` : URL(Scheme://hostname/path)
  - `?` : query string begin
  - `ie=utf8` : parameter name=parameter value
  - `&` : query string separator
  - `query=아이유` : parameter name=parameter value

## 2. 크롤링 과정

웹 크롤링은 클라이언트가 웹 서버에 데이터를 요청(Request)하고, 웹 서버가 해당 요청에 대한 결과를 응답(Response)하면서 이루어집니다. 웹 크롤링 과정은 다음과 같습니다.

1. HTTP Request & HTTP Response
    - HTTP 요쳥 및 HTTP 응답 결과 확인
2. HTML Parsing
    - 응답 받은 객체를 HTML/JSON로 변환
3. Extract & Preprocessing Data
    - CSS Selector, XPath 등을 이용하여 HTML로부터 수집하고자 하는 데이터 추출 및 전처리
4. Save Data

## 3. 크롤링 종류

크롤링에는 크게 정적 웹 페이지를 크롤링 하는 정적 크롤링과 동적 웹 페이지를 크롤링 하는 동적 크롤링이 존재합니다.

### 정적 페이지 vs. 동적 페이지

- 정적 웹 페이지
  - 웹 서버에 이미 저장된 HTML 문서를 클라이언트에게 전송하는 웹 페이지
  - 사용자는 서버에 저장된 데이터가 변경되지 않는 한 고정된 웹페이지를 보게 됨
  - 모든 사용자는 같은 결과의 웹 페이지를 서버에 요청하고 응답받음
- 동적 웹 페이지
  - 요청 정보를 처리한 후에 제작된 HTML 문서를 클라이언트에게 전송하는 웹 페이지
  - 사용자는 상황, 시간, 요청 등에 따라 달라지는 웹페이지를 보게 됨
  - 같은 페이지라도 사용자마다 다른 결과의 웹페이지를 서버에 요청하고 받을 수 있음

### 정적 크롤링 vs. 동적 크롤링

- [정적 크롤링]({{site.url}}//data%20analysis/DC-Static-Webpage-Crawling/)
  - `requests` : HTTP 관련 라이브러리
    - `requests.get()` : HTTP 요청
  - `bs4` : HTML/XML 파싱 관련 라이브러리
    - `BeautifulSoup()` : HTTP 응답 객체를 HTML로 변환
- [동적 크롤링](https://cheris8.github.io/data%20analysis/DC-Dynamic-Webpage-Crawling/)
  1. [Selenium with Web Driver]({{site.url}}//data%20analysis/DC-Selenium/)
  2. Open API
  3. AJAX
  