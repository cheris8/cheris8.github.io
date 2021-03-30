---
title: '[데이터 크롤링] 동적 웹 페이지 크롤링 with Python'

categories:
  - Data Analysis
tags:
  - Data Crawling

last_modified_at: 2021-03-13T08:06:00-05:00

classes: wide
---

이 글은 파이썬을 이용한 동적 크롤링에 관한 기록입니다.

## 동적 크롤링

동적 웹 페이지를 크롤링 하는 데에는 크게 세가지 방식이 존재합니다.

1. Selenium with Web Driver
2. Open API
3. AJAX

## 1. Selenium with Web Driver

웹 드라이버(ChromeDriver, Firefox, Phantom JS 등)와 함께 Selenium을 사용하여 동적 웹페이지를 크롤링 할 수 있습니다. 이에 관한 자세한 내용은 [이 곳]({{site.url}}//data%20analysis/DC-Selenium/)에서 확인하실 수 있습니다.

### 2. Open API (Open Application Programming Interface)

Open API는 자신이 보유한 정보나 애플리케이션 등을 타 정보 시스템에서 네트워크를 통하여 활용할 수 있도록 공개하는 것을 말합니다. 데이터를 제어할 수 있는 간단하고 직관적인 인터페이스의 제공을 통해 사용자의 참여를 유도하는 사용자 중심의 비즈니스 모델입니다.

### Naver Open API

네이버에서는 네이버의 뉴스, 블로그, 카페 등에 존재하는 데이터를 비로그인 방식으로 크롤링 할 수 있도록 Naver Open API를 제공합니다. HTTP 헤더에 클라이언트 ID와 클라이언트 Secret을 추가하여 요청을 보내면 응답을 받을 수 있습니다.

### Code

> 특정 쿼리로 네이버 뉴스 기사를 검색한 결과 크롤링 하기

```python
import urllib.request
import json
import pandas as pd
```

```python
# Naver Open API Settings
client_id = 'XXXX' # Client ID
client_secret = 'XXXX' # Client Secret

# Search Settings
query = urllib.parse.quote('아이유')
url = 'https://openapi.naver.com/v1/search/news?query=' + query
```

```python
# HTTP Request & Response
request = urllib.request.Request(url)
request.add_header('X-Naver-Client-Id', client_id)
request.add_header('X-Naver-Client-Secret', client_secret)

response = urllib.request.urlopen(request)
response_code = response.getcode()

if response_code==200:
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:"+response_code)
```

    {
    "lastBuildDate": "Tue, 30 Mar 2021 23:56:02 +0900",
    "total": 326606,
    "start": 1,
    "display": 10,
    "items": [
    {
    "title": "'볼륨' <b>아이유</b> &quot;'코인' 일 중독이었던 내 20대 빗댄 곡…1위보다 롱런 더 중요...",
    "originallink": "http://www.newsinside.kr/news/articleView.html?idxno=1105992",
    "link": "http://www.newsinside.kr/news/articleView.html?idxno=1105992",
    "description": "사진=<b>아이유</b> '코인' 뮤직비디오 영상캡처 가수 <b>아이유</b>가 '라일락(Lilac)'과 더블 타이틀곡으로 선택하 '코인(Coin)'에 담긴 의미를 설명했다. <b>아이유</b>는 30일 방송된 KBS 쿨FM '강한나의 볼륨의 높여요'에 출연해 4년만에... ",
    "pubDate": "Tue, 30 Mar 2021 22:46:00 +0900"
    
    },
    {
    "title": "'볼륨' 강한나, <b>아이유</b> 폭풍 칭찬에 &quot;부끄러워&quot;",
    "originallink": "https://hankookilbo.com/News/Read/A2021033021570003604?did=NA",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=469&aid=0000592847",
    "description": "배우 강한나가 가수 <b>아이유</b>의 폭풍 칭찬에 수줍어했다. 30일 방송된 KBS 쿨FM '강한나의 볼륨을 높여요'에는 <b>아이유</b>가 출연했다. 과거 드라마 '달의 연인 - 보보경심 려'에서 호흡을 맞췄던 <b>아이유</b>와 DJ 강한나는 이날... ",
    "pubDate": "Tue, 30 Mar 2021 22:15:00 +0900"
    
    },
    {
    "title": "'볼륨' <b>아이유</b> &quot;'내 손을 잡아' 역주행, 기분 좋아&quot;",
    "originallink": "https://hankookilbo.com/News/Read/A2021033021470001672?did=NA",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=469&aid=0000592842",
    "description": "가수 겸 배우 <b>아이유</b>가 '내 손을 잡아'의 역주행에 대한 생각을 밝혔다. 30일 방송된 KBS 쿨FM '강한나의 볼륨을 높여요'에는 <b>아이유</b>가 게스트로 출연했다. 이날 <b>아이유</b>는 청취자들과 활발하게 소통했다. 한 청취자는... ",
    "pubDate": "Tue, 30 Mar 2021 21:57:00 +0900"
    
    },
    {
    "title": "뉴이스트 2집 ‘Romanticize’ 음반 판매순위 1위",
    "originallink": "https://www.sisa-news.com/news/article.html?no=152527",
    "link": "https://www.sisa-news.com/news/article.html?no=152527",
    "description": "2위는 <b>아이유</b> 5집 ‘LILAC’이, 3위는 아스트로 2집 ‘All Yours’가 올랐다. 4위는 손호영의 신보 ‘2021 호이력 HOI at HOME’이 올랐고, 백현 미니 3집 ‘Bambi’의 Photo Book ver.과 Jewel Case ver.이... ",
    "pubDate": "Tue, 30 Mar 2021 21:55:00 +0900"
    
    },
    {
    "title": "<b>아이유</b> &quot;신곡 '코인', 일에 중독된 내 20대에 대한 곡&quot;[볼륨을 높여요]",
    "originallink": "http://star.mt.co.kr/stview.php?no=2021033021203989371",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=108&aid=0002943517",
    "description": "가수 <b>아이유</b>가 새 앨범 타이틀곡 '코인'(Coin)에 담긴 의미를 밝혔다. 30일 방송된 KBS 쿨 FM '강한나의 볼륨의 높여요'에는 정규 5집 '라일락'(Lilac)을 발매한 <b>아이유</b>가 게스트로 출연했다. 이번 앨범은 <b>아이유</b>가 약 4년 만에... ",
    "pubDate": "Tue, 30 Mar 2021 21:36:00 +0900"
    
    },
    {
    "title": "'유 퀴즈' <b>아이유</b>→뽀로로 성우, 100회 맞이 '무언가의 현실판' 특집",
    "originallink": "http://www.topdaily.kr/news/articleView.html?idxno=100186",
    "link": "http://www.topdaily.kr/news/articleView.html?idxno=100186",
    "description": "100회 동안 '유 퀴즈'의 BGM(배경음악)을 가장 많이 책임진 <b>아이유</b> 자기님을 시작으로 체스 드라마의 현실판인 최연소 체스 국가대표 김유빈 자기님, 어린이들의 영원한 최애 캐릭터 뽀로로의 성우 이선 자기님, 영화... ",
    "pubDate": "Tue, 30 Mar 2021 21:19:00 +0900"
    
    },
    {
    "title": "'볼륨' <b>아이유</b> &quot;오랜만의 음악방송 어려워…요즘 아이돌 대단하다&quot;",
    "originallink": "https://www.tvreport.co.kr/2065427",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=213&aid=0001182177",
    "description": "<b>아이유</b>가 오랜만의 음악방송 출연 소감을 전했다. 30일 오후 방송된 KBS Cool FM ‘강한나의 볼륨을 높여요’에는 가수 <b>아이유</b>가 출연했다. 이날 <b>아이유</b>는 SBS '달의 연인'에서 인연을 맺은 DJ 강한나를 보며 반가움을... ",
    "pubDate": "Tue, 30 Mar 2021 20:59:00 +0900"
    
    },
    {
    "title": "티아라 지연, 각 잡고 춤추는 <b>아이유</b> 어떠냐는 질문에 &quot;또르르&quot;",
    "originallink": "http://www.osen.co.kr/article/G1111548734",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=109&aid=0004377866",
    "description": "지연이 <b>아이유</b> 관련 질문에 절친다운 답변을 내놨다. 가수 겸 배우 티아라 지연은 30일 오후 자신의... 특히 한 팬은 &quot;<b>아이유</b> 님 이번엔 각 잡고 춤추시던데 어떠셨나요?&quot;라고 질문했고, 절친 지연은 &quot;워어...또르르... ",
    "pubDate": "Tue, 30 Mar 2021 20:56:00 +0900"
    
    },
    {
    "title": "'유퀴즈' 31일 100회 특집…<b>아이유</b>부터 뽀로로 성우·협상 전문가까지",
    "originallink": "https://sports.hankooki.com/lpage/entv/202103/sp20210330205150136660.htm?s_ref=nv",
    "link": "https://sports.hankooki.com/lpage/entv/202103/sp20210330205150136660.htm?s_ref=nv",
    "description": "가수 <b>아이유</b>가 tvN '유 퀴즈 온 더 블럭' 100회에 출연한다. 31일 방송되는 '유 퀴즈 온 더 블럭'(이하 '유퀴즈... 이날 '유퀴즈' 100회 방송에서는 그동안 BGM(배경음악)을 가장 많이 책임진 <b>아이유</b>를 시작으로 최연소 체스... ",
    "pubDate": "Tue, 30 Mar 2021 20:55:00 +0900"
    
    },
    {
    "title": "<b>아이유</b>, 얼굴로 나라세웠지은",
    "originallink": "http://www.xportsnews.com/?ac=article_view&entry_id=1407065",
    "link": "https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=311&aid=0001281573",
    "description": "<b>아이유</b>의 미모가 눈길을 끈다. 30일 SBS ‘인기가요’ 홈페이지에는 “[PD노트 미션 포토] <b>아이유</b>”라는 제목의 게시글이 게재됐다. 사진 속에는 ‘인기가요’ 무대 속 <b>아이유</b>의 모습이 담겨 있다. 그의 눈부신... ",
    "pubDate": "Tue, 30 Mar 2021 19:53:00 +0900"
    
    }
    ]
    }

```python
# json
j = json.loads(response_body.decode('utf-8'))
j
```

    {'lastBuildDate': 'Tue, 30 Mar 2021 23:56:02 +0900',
     'total': 326606,
     'start': 1,
     'display': 10,
     'items': [{'title': "'볼륨' <b>아이유</b> &quot;'코인' 일 중독이었던 내 20대 빗댄 곡…1위보다 롱런 더 중요...",
       'originallink': 'http://www.newsinside.kr/news/articleView.html?idxno=1105992',
       'link': 'http://www.newsinside.kr/news/articleView.html?idxno=1105992',
       'description': "사진=<b>아이유</b> '코인' 뮤직비디오 영상캡처 가수 <b>아이유</b>가 '라일락(Lilac)'과 더블 타이틀곡으로 선택하\xa0'코인(Coin)'에 담긴 의미를 설명했다. <b>아이유</b>는 30일 방송된 KBS 쿨FM '강한나의 볼륨의 높여요'에 출연해\xa04년만에... ",
       'pubDate': 'Tue, 30 Mar 2021 22:46:00 +0900'},
      {'title': "'볼륨' 강한나, <b>아이유</b> 폭풍 칭찬에 &quot;부끄러워&quot;",
       'originallink': 'https://hankookilbo.com/News/Read/A2021033021570003604?did=NA',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=469&aid=0000592847',
       'description': "배우 강한나가 가수 <b>아이유</b>의 폭풍 칭찬에 수줍어했다. 30일 방송된 KBS 쿨FM '강한나의 볼륨을 높여요'에는 <b>아이유</b>가 출연했다. 과거 드라마 '달의 연인 - 보보경심 려'에서 호흡을 맞췄던 <b>아이유</b>와 DJ 강한나는 이날... ",
       'pubDate': 'Tue, 30 Mar 2021 22:15:00 +0900'},
      {'title': "'볼륨' <b>아이유</b> &quot;'내 손을 잡아' 역주행, 기분 좋아&quot;",
       'originallink': 'https://hankookilbo.com/News/Read/A2021033021470001672?did=NA',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=469&aid=0000592842',
       'description': "가수 겸 배우 <b>아이유</b>가 '내 손을 잡아'의 역주행에 대한 생각을 밝혔다. 30일 방송된 KBS 쿨FM '강한나의 볼륨을 높여요'에는 <b>아이유</b>가 게스트로 출연했다. 이날 <b>아이유</b>는 청취자들과 활발하게 소통했다. 한 청취자는... ",
       'pubDate': 'Tue, 30 Mar 2021 21:57:00 +0900'},
      {'title': '뉴이스트 2집 ‘Romanticize’ 음반 판매순위 1위',
       'originallink': 'https://www.sisa-news.com/news/article.html?no=152527',
       'link': 'https://www.sisa-news.com/news/article.html?no=152527',
       'description': '2위는 <b>아이유</b> 5집 ‘LILAC’이, 3위는 아스트로 2집 ‘All Yours’가 올랐다. 4위는 손호영의 신보 ‘2021 호이력 HOI at HOME’이 올랐고, 백현 미니 3집 ‘Bambi’의 Photo Book ver.과 Jewel Case ver.이... ',
       'pubDate': 'Tue, 30 Mar 2021 21:55:00 +0900'},
      {'title': "<b>아이유</b> &quot;신곡 '코인', 일에 중독된 내 20대에 대한 곡&quot;[볼륨을 높여요]",
       'originallink': 'http://star.mt.co.kr/stview.php?no=2021033021203989371',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=108&aid=0002943517',
       'description': "가수 <b>아이유</b>가 새 앨범 타이틀곡 '코인'(Coin)에 담긴 의미를 밝혔다. 30일 방송된 KBS 쿨 FM '강한나의 볼륨의 높여요'에는 정규 5집 '라일락'(Lilac)을 발매한 <b>아이유</b>가 게스트로 출연했다. 이번 앨범은 <b>아이유</b>가 약 4년 만에... ",
       'pubDate': 'Tue, 30 Mar 2021 21:36:00 +0900'},
      {'title': "'유 퀴즈' <b>아이유</b>→뽀로로 성우, 100회 맞이 '무언가의 현실판' 특집",
       'originallink': 'http://www.topdaily.kr/news/articleView.html?idxno=100186',
       'link': 'http://www.topdaily.kr/news/articleView.html?idxno=100186',
       'description': "100회 동안 '유 퀴즈'의 BGM(배경음악)을 가장 많이 책임진 <b>아이유</b> 자기님을 시작으로 체스 드라마의 현실판인 최연소 체스 국가대표 김유빈 자기님, 어린이들의 영원한 최애 캐릭터 뽀로로의 성우 이선 자기님, 영화... ",
       'pubDate': 'Tue, 30 Mar 2021 21:19:00 +0900'},
      {'title': "'볼륨' <b>아이유</b> &quot;오랜만의 음악방송 어려워…요즘 아이돌 대단하다&quot;",
       'originallink': 'https://www.tvreport.co.kr/2065427',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=213&aid=0001182177',
       'description': "<b>아이유</b>가 오랜만의 음악방송 출연 소감을 전했다. 30일 오후 방송된 KBS Cool FM ‘강한나의 볼륨을 높여요’에는 가수 <b>아이유</b>가 출연했다. 이날 <b>아이유</b>는 SBS '달의 연인'에서 인연을 맺은 DJ 강한나를 보며 반가움을... ",
       'pubDate': 'Tue, 30 Mar 2021 20:59:00 +0900'},
      {'title': '티아라 지연, 각 잡고 춤추는 <b>아이유</b> 어떠냐는 질문에 &quot;또르르&quot;',
       'originallink': 'http://www.osen.co.kr/article/G1111548734',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=109&aid=0004377866',
       'description': '지연이 <b>아이유</b> 관련 질문에 절친다운 답변을 내놨다. 가수 겸 배우 티아라 지연은 30일 오후 자신의... 특히 한 팬은 &quot;<b>아이유</b> 님 이번엔 각 잡고 춤추시던데 어떠셨나요?&quot;라고 질문했고, 절친 지연은 &quot;워어...또르르... ',
       'pubDate': 'Tue, 30 Mar 2021 20:56:00 +0900'},
      {'title': "'유퀴즈' 31일 100회 특집…<b>아이유</b>부터 뽀로로 성우·협상 전문가까지",
       'originallink': 'https://sports.hankooki.com/lpage/entv/202103/sp20210330205150136660.htm?s_ref=nv',
       'link': 'https://sports.hankooki.com/lpage/entv/202103/sp20210330205150136660.htm?s_ref=nv',
       'description': "가수 <b>아이유</b>가 tvN '유 퀴즈 온 더 블럭' 100회에 출연한다. 31일 방송되는 '유 퀴즈 온 더 블럭'(이하 '유퀴즈... 이날 '유퀴즈' 100회 방송에서는 그동안 BGM(배경음악)을 가장 많이 책임진 <b>아이유</b>를 시작으로 최연소 체스... ",
       'pubDate': 'Tue, 30 Mar 2021 20:55:00 +0900'},
      {'title': '<b>아이유</b>, 얼굴로 나라세웠지은',
       'originallink': 'http://www.xportsnews.com/?ac=article_view&entry_id=1407065',
       'link': 'https://news.naver.com/main/read.nhn?mode=LSD&mid=sec&sid1=106&oid=311&aid=0001281573',
       'description': '<b>아이유</b>의 미모가 눈길을 끈다. 30일 SBS ‘인기가요’ 홈페이지에는 “[PD노트 미션 포토] <b>아이유</b>”라는 제목의 게시글이 게재됐다. 사진 속에는 ‘인기가요’ 무대 속 <b>아이유</b>의 모습이 담겨 있다. 그의 눈부신... ',
       'pubDate': 'Tue, 30 Mar 2021 19:53:00 +0900'}]}

```python
j.keys()
```

    dict_keys(['lastBuildDate', 'total', 'start', 'display', 'items'])

```python
data = pd.DataFrame(j['items'])
data
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>originallink</th>
      <th>link</th>
      <th>description</th>
      <th>pubDate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>'볼륨' &lt;b&gt;아이유&lt;/b&gt; &amp;quot;'코인' 일 중독이었던 내 20대 빗댄 곡…...</td>
      <td>http://www.newsinside.kr/news/articleView.html...</td>
      <td>http://www.newsinside.kr/news/articleView.html...</td>
      <td>사진=&lt;b&gt;아이유&lt;/b&gt; '코인' 뮤직비디오 영상캡처 가수 &lt;b&gt;아이유&lt;/b&gt;가 '...</td>
      <td>Tue, 30 Mar 2021 22:46:00 +0900</td>
    </tr>
    <tr>
      <th>1</th>
      <td>'볼륨' 강한나, &lt;b&gt;아이유&lt;/b&gt; 폭풍 칭찬에 &amp;quot;부끄러워&amp;quot;</td>
      <td>https://hankookilbo.com/News/Read/A20210330215...</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>배우 강한나가 가수 &lt;b&gt;아이유&lt;/b&gt;의 폭풍 칭찬에 수줍어했다. 30일 방송된 K...</td>
      <td>Tue, 30 Mar 2021 22:15:00 +0900</td>
    </tr>
    <tr>
      <th>2</th>
      <td>'볼륨' &lt;b&gt;아이유&lt;/b&gt; &amp;quot;'내 손을 잡아' 역주행, 기분 좋아&amp;quot;</td>
      <td>https://hankookilbo.com/News/Read/A20210330214...</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>가수 겸 배우 &lt;b&gt;아이유&lt;/b&gt;가 '내 손을 잡아'의 역주행에 대한 생각을 밝혔다...</td>
      <td>Tue, 30 Mar 2021 21:57:00 +0900</td>
    </tr>
    <tr>
      <th>3</th>
      <td>뉴이스트 2집 ‘Romanticize’ 음반 판매순위 1위</td>
      <td>https://www.sisa-news.com/news/article.html?no...</td>
      <td>https://www.sisa-news.com/news/article.html?no...</td>
      <td>2위는 &lt;b&gt;아이유&lt;/b&gt; 5집 ‘LILAC’이, 3위는 아스트로 2집 ‘All Y...</td>
      <td>Tue, 30 Mar 2021 21:55:00 +0900</td>
    </tr>
    <tr>
      <th>4</th>
      <td>&lt;b&gt;아이유&lt;/b&gt; &amp;quot;신곡 '코인', 일에 중독된 내 20대에 대한 곡&amp;q...</td>
      <td>http://star.mt.co.kr/stview.php?no=20210330212...</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>가수 &lt;b&gt;아이유&lt;/b&gt;가 새 앨범 타이틀곡 '코인'(Coin)에 담긴 의미를 밝혔...</td>
      <td>Tue, 30 Mar 2021 21:36:00 +0900</td>
    </tr>
    <tr>
      <th>5</th>
      <td>'유 퀴즈' &lt;b&gt;아이유&lt;/b&gt;→뽀로로 성우, 100회 맞이 '무언가의 현실판' 특집</td>
      <td>http://www.topdaily.kr/news/articleView.html?i...</td>
      <td>http://www.topdaily.kr/news/articleView.html?i...</td>
      <td>100회 동안 '유 퀴즈'의 BGM(배경음악)을 가장 많이 책임진 &lt;b&gt;아이유&lt;/b...</td>
      <td>Tue, 30 Mar 2021 21:19:00 +0900</td>
    </tr>
    <tr>
      <th>6</th>
      <td>'볼륨' &lt;b&gt;아이유&lt;/b&gt; &amp;quot;오랜만의 음악방송 어려워…요즘 아이돌 대단하...</td>
      <td>https://www.tvreport.co.kr/2065427</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>&lt;b&gt;아이유&lt;/b&gt;가 오랜만의 음악방송 출연 소감을 전했다. 30일 오후 방송된 K...</td>
      <td>Tue, 30 Mar 2021 20:59:00 +0900</td>
    </tr>
    <tr>
      <th>7</th>
      <td>티아라 지연, 각 잡고 춤추는 &lt;b&gt;아이유&lt;/b&gt; 어떠냐는 질문에 &amp;quot;또르르...</td>
      <td>http://www.osen.co.kr/article/G1111548734</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>지연이 &lt;b&gt;아이유&lt;/b&gt; 관련 질문에 절친다운 답변을 내놨다. 가수 겸 배우 티아...</td>
      <td>Tue, 30 Mar 2021 20:56:00 +0900</td>
    </tr>
    <tr>
      <th>8</th>
      <td>'유퀴즈' 31일 100회 특집…&lt;b&gt;아이유&lt;/b&gt;부터 뽀로로 성우·협상 전문가까지</td>
      <td>https://sports.hankooki.com/lpage/entv/202103/...</td>
      <td>https://sports.hankooki.com/lpage/entv/202103/...</td>
      <td>가수 &lt;b&gt;아이유&lt;/b&gt;가 tvN '유 퀴즈 온 더 블럭' 100회에 출연한다. 3...</td>
      <td>Tue, 30 Mar 2021 20:55:00 +0900</td>
    </tr>
    <tr>
      <th>9</th>
      <td>&lt;b&gt;아이유&lt;/b&gt;, 얼굴로 나라세웠지은</td>
      <td>http://www.xportsnews.com/?ac=article_view&amp;ent...</td>
      <td>https://news.naver.com/main/read.nhn?mode=LSD&amp;...</td>
      <td>&lt;b&gt;아이유&lt;/b&gt;의 미모가 눈길을 끈다. 30일 SBS ‘인기가요’ 홈페이지에는 ...</td>
      <td>Tue, 30 Mar 2021 19:53:00 +0900</td>
    </tr>
  </tbody>
</table>
</div>

## 3. AJAX (Asynchronous Javascript And XML)

AJAX는 JavaScript의 라이브러리 중 하나로, **JavaScript를 사용하여 브라우저와 서버가 데이터를 교환하는 통신 방식**입니다. 이때 AJAX는 비동기 방식에 해당되는데, 비동기 방식이란 웹 브라우저가 웹 서버와 통신할 때 웹 페이지 전체를 새로고침 하는 대신 웹 페이지의 일부만을 위한 데이터를 로드하는 기법을 말합니다.

웹 브라우저는 XMLHttpRequest 객체를 사용하여 요청을 보내고, 웹 서버는 요청에 대하여 XML 혹은 JSON 형태로 응답합니다.

### JSON (JavaScript Object Notation)

JSON은 **JavaScirpt를 사용하여 데이터를 교환할 때 사용되는 교환 형식입니다.** JSON은 순수 텍스트 데이터를 기반으로 하여 {Key:Value} 형식이 반복되는 형태로 구성됩니다. 이러한 JSON 데이터를 파이썬 내장 자료형(딕셔너리, 리스트, 튜플)으로 변환하는 작업을 JSON decoding이라고 합니다. 이를 위해 `json.loads()` 함수를 사용합니다. 반대로, 파이썬 내장 자료형(딕셔너리, 리스트, 튜플)을 JSON 데이터로 변환하는 작업을 JSON encoding이라고 합니다.

### Code

> 인기검색 top 10의 순위 & 이름 & 현재시가 크롤링 하기

```python
import requests
import json
```

```python
# Settings
custom_header = {
    'referer' : 'http://http://finance.daum.net/quotes/A048410#home',
    'user-agent' : 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36' 
}
url = 'http://finance.daum.net/api/search/ranks?limit=10' #해당 접속 사이트가 아닌 원본데이터가 오는 url 추적. network에서 가지고 온다.
```

```python
# HTTP Request & Response
req = requests.get(url, headers=custom_header)

if req.status_code == requests.codes.ok:    
    print(req.text)
```

    {"data":[{"rank":1,"rankChange":0,"symbolCode":"A035720","code":"KR7035720002","name":"카카오","tradePrice":493500.0,"change":"RISE","changePrice":6000.0,"changeRate":0.0123076923,"chartSlideImage":null,"isNew":false},{"rank":2,"rankChange":1,"symbolCode":"A011200","code":"KR7011200003","name":"HMM","tradePrice":28100.0,"change":"FALL","changePrice":2850.0,"changeRate":0.0920840065,"chartSlideImage":null,"isNew":false},{"rank":3,"rankChange":-1,"symbolCode":"A005930","code":"KR7005930003","name":"삼성전자","tradePrice":82200.0,"change":"RISE","changePrice":600.0,"changeRate":0.0073529412,"chartSlideImage":null,"isNew":false},{"rank":4,"rankChange":2,"symbolCode":"A004540","code":"KR7004540001","name":"깨끗한나라","tradePrice":8450.0,"change":"RISE","changePrice":1940.0,"changeRate":0.2980030722,"chartSlideImage":null,"isNew":false},{"rank":5,"rankChange":0,"symbolCode":"A041190","code":"KR7041190000","name":"우리기술투자","tradePrice":9640.0,"change":"UPPER_LIMIT","changePrice":2220.0,"changeRate":0.2991913747,"chartSlideImage":null,"isNew":false},{"rank":6,"rankChange":-2,"symbolCode":"A066570","code":"KR7066570003","name":"LG전자","tradePrice":152000.0,"change":"RISE","changePrice":11500.0,"changeRate":0.0818505338,"chartSlideImage":null,"isNew":false},{"rank":7,"rankChange":0,"symbolCode":"A068270","code":"KR7068270008","name":"셀트리온","tradePrice":323500.0,"change":"FALL","changePrice":7000.0,"changeRate":0.0211800303,"chartSlideImage":null,"isNew":false},{"rank":8,"rankChange":1,"symbolCode":"A004545","code":"KR7004541009","name":"깨끗한나라우","tradePrice":40950.0,"change":"UPPER_LIMIT","changePrice":9450.0,"changeRate":0.3,"chartSlideImage":null,"isNew":false},{"rank":9,"rankChange":1,"symbolCode":"A005380","code":"KR7005380001","name":"현대차","tradePrice":219500.0,"change":"RISE","changePrice":4000.0,"changeRate":0.0185614849,"chartSlideImage":null,"isNew":false},{"rank":10,"rankChange":0,"symbolCode":"A003530","code":"KR7003530003","name":"한화투자증권","tradePrice":4015.0,"change":"UPPER_LIMIT","changePrice":925.0,"changeRate":0.2993527508,"chartSlideImage":null,"isNew":true}]}

```python
# json
j = json.loads(req.text)
j
```

    {'data': [{'rank': 1,
       'rankChange': 0,
       'symbolCode': 'A035720',
       'code': 'KR7035720002',
       'name': '카카오',
       'tradePrice': 493500.0,
       'change': 'RISE',
       'changePrice': 6000.0,
       'changeRate': 0.0123076923,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 2,
       'rankChange': 1,
       'symbolCode': 'A011200',
       'code': 'KR7011200003',
       'name': 'HMM',
       'tradePrice': 28100.0,
       'change': 'FALL',
       'changePrice': 2850.0,
       'changeRate': 0.0920840065,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 3,
       'rankChange': -1,
       'symbolCode': 'A005930',
       'code': 'KR7005930003',
       'name': '삼성전자',
       'tradePrice': 82200.0,
       'change': 'RISE',
       'changePrice': 600.0,
       'changeRate': 0.0073529412,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 4,
       'rankChange': 2,
       'symbolCode': 'A004540',
       'code': 'KR7004540001',
       'name': '깨끗한나라',
       'tradePrice': 8450.0,
       'change': 'RISE',
       'changePrice': 1940.0,
       'changeRate': 0.2980030722,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 5,
       'rankChange': 0,
       'symbolCode': 'A041190',
       'code': 'KR7041190000',
       'name': '우리기술투자',
       'tradePrice': 9640.0,
       'change': 'UPPER_LIMIT',
       'changePrice': 2220.0,
       'changeRate': 0.2991913747,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 6,
       'rankChange': -2,
       'symbolCode': 'A066570',
       'code': 'KR7066570003',
       'name': 'LG전자',
       'tradePrice': 152000.0,
       'change': 'RISE',
       'changePrice': 11500.0,
       'changeRate': 0.0818505338,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 7,
       'rankChange': 0,
       'symbolCode': 'A068270',
       'code': 'KR7068270008',
       'name': '셀트리온',
       'tradePrice': 323500.0,
       'change': 'FALL',
       'changePrice': 7000.0,
       'changeRate': 0.0211800303,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 8,
       'rankChange': 1,
       'symbolCode': 'A004545',
       'code': 'KR7004541009',
       'name': '깨끗한나라우',
       'tradePrice': 40950.0,
       'change': 'UPPER_LIMIT',
       'changePrice': 9450.0,
       'changeRate': 0.3,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 9,
       'rankChange': 1,
       'symbolCode': 'A005380',
       'code': 'KR7005380001',
       'name': '현대차',
       'tradePrice': 219500.0,
       'change': 'RISE',
       'changePrice': 4000.0,
       'changeRate': 0.0185614849,
       'chartSlideImage': None,
       'isNew': False},
      {'rank': 10,
       'rankChange': 0,
       'symbolCode': 'A003530',
       'code': 'KR7003530003',
       'name': '한화투자증권',
       'tradePrice': 4015.0,
       'change': 'UPPER_LIMIT',
       'changePrice': 925.0,
       'changeRate': 0.2993527508,
       'chartSlideImage': None,
       'isNew': True}]}

```python
j.keys()
```

    dict_keys(['data'])

```python
for rank in j['data']:
    print(rank['rank'], rank['symbolCode'], rank['name'], rank['tradePrice'])
```

    1 A035720 카카오 493500.0
    2 A011200 HMM 28100.0
    3 A005930 삼성전자 82200.0
    4 A004540 깨끗한나라 8450.0
    5 A041190 우리기술투자 9640.0
    6 A066570 LG전자 152000.0
    7 A068270 셀트리온 323500.0
    8 A004545 깨끗한나라우 40950.0
    9 A005380 현대차 219500.0
    10 A003530 한화투자증권 4015.0

