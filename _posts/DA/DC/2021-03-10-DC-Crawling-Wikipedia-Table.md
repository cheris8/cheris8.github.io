---
title: '[데이터 크롤링] 위키피디아 표 데이터 크롤링 하기'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Data Crawling

last_modified_at: 2021-03-10T08:06:00-05:00

classes: wide
---

이 글은 [위키피디아](https://ko.wikipedia.org/wiki/위키백과:대문)에 존재하는 표 형태의 데이터를 크롤링 하는 방법에 관한 기록입니다.

## 실습 Code

먼저 필요한 라이브러리를 불러옵니다.

```python
from bs4 import BeautifulSoup
from html_table_parser import parser_functions as parser
import pandas as pd
import requests
```

크롤링 하고자 하는 웹페이지 주소를 설정합니다. 아래의 경우 위키피디아에서 제공하는 2019년 대한민국 영화 목록을 크롤링 하고자 합니다.

```python
# 크롤링 하고자 하는 웹페이지 주소 설정
url = 'https://ko.wikipedia.org/wiki/2019년_대한민국의_영화_목록'
```

이제 해당 url에 대하여 HTTP request를 통해 HTML response를 받습니다.

```python
req = requests.get(url)
html = req.text
soup = BeautifulSoup(html, 'html.parser')
```

해당 웹페이지에서 크롤링 하고자 하는 표의 속성을 확인한 후 `find_all()` 함수를 통해 HTML에서 해당 표만을 추출합니다. 아래의 경우 HTML에서 `class`가 `sortable wikitable`인 `table` 태그만을 추출하고자 합니다.

```python
# 크롤링 하고자 하는 표 : <table class="sortable wikitable">
tables = soup.find_all('table', attrs={'class':'sortable wikitable'})
```

이제 변수 `tables`에 담겨있는 정보들을 데이터프레임 형태로 변환합니다.

```python
# 데이터프레임 생성
temp = []
for table in tables:
    table = parser.make2d(table)
    table = pd.DataFrame(table[1:])
    temp.append(table)
df = pd.concat(temp)
df
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1월</td>
      <td>1일</td>
      <td>〈언니〉</td>
      <td>(주)제이앤씨미디어그룹 , TCO(주)더콘텐츠온</td>
      <td>감독 임경택출연 이시영, 박세완, 이준혁, 최진호</td>
      <td>액션</td>
      <td>청소년 관람불가 (대한민국)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1월</td>
      <td>9일</td>
      <td>〈내안의 그놈〉</td>
      <td>(주)메리크리스마스</td>
      <td>감독 강효진출연 박성웅, 진영, 라미란</td>
      <td>코미디, 판타지</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1월</td>
      <td>9일</td>
      <td>〈말모이〉</td>
      <td>롯데 엔터테인먼트</td>
      <td>감독 엄유나출연 유해진, 윤계상</td>
      <td>드라마</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1월</td>
      <td>16일</td>
      <td>〈그대 이름은 장미〉</td>
      <td>(주)리틀빅픽쳐스</td>
      <td>감독 조석현출연 유호정, 박성웅, 오정세, 채수빈, 하연수</td>
      <td>코미디</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1월</td>
      <td>16일</td>
      <td>〈언더독〉</td>
      <td>(주)NEW</td>
      <td>감독 오성윤, 이춘백출연 도경수, 박소담, 박철민</td>
      <td>애니메이션</td>
      <td>전체 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>56</th>
      <td>12월</td>
      <td>4일</td>
      <td>〈너의 여자친구〉</td>
      <td>(주)스톰픽쳐스코리아, 와이드 릴리즈(주)</td>
      <td>감독 이장희 출연 이엘리야, 지일주, 허정민, 김기두, 이진이</td>
      <td>코미디, 멜로, 로맨스</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>57</th>
      <td>12월</td>
      <td>12일</td>
      <td>〈속물들〉</td>
      <td>(주)삼백상회 300 &amp; Co.</td>
      <td>감독 신아가, 이상철 출연 유다인, 심희섭, 송재림</td>
      <td>블랙코미디</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>58</th>
      <td>12월</td>
      <td>18일</td>
      <td>〈시동〉</td>
      <td>넥스트 엔터테인먼트 월드</td>
      <td>감독 최정열 출연 마동석, 박정민, 정해인, 염정아</td>
      <td>드라마</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>59</th>
      <td>12월</td>
      <td>19일</td>
      <td>〈백두산〉</td>
      <td>(주)CJ ENM</td>
      <td>감독 이해준, 김병서출연 이병헌, 하정우, 마동석, 전혜진, 배수지</td>
      <td>어드벤처, 드라마</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>60</th>
      <td>12월</td>
      <td>26일</td>
      <td>〈천문: 하늘에 묻는다〉</td>
      <td>롯데엔터테인먼트</td>
      <td>감독 허진호 출연 최민식, 한석규</td>
      <td>사극</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
  </tbody>
</table>
<p>121 rows × 7 columns</p>
</div>

데이터프레임의 간략한 정보를 확인합니다.

```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 121 entries, 0 to 60
    Data columns (total 7 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   0       121 non-null    object
     1   1       114 non-null    object
     2   2       114 non-null    object
     3   3       114 non-null    object
     4   4       114 non-null    object
     5   5       114 non-null    object
     6   6       114 non-null    object
    dtypes: object(7)
    memory usage: 7.6+ KB

크롤링 후처리를 합니다.

```python
df.columns = ['개봉월', '개봉일', '제목', '제작/배급', '출연/제작진', '장르', '등급'] # 컬럼명 변경
df.dropna(inplace=True) # 결측치 제거
df.reset_index(drop=True, inplace=True) # 인덱스 재설정
```

최종적인 데이터셋은 다음과 같습니다.

```python
df
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
      <th>개봉월</th>
      <th>개봉일</th>
      <th>제목</th>
      <th>제작/배급</th>
      <th>출연/제작진</th>
      <th>장르</th>
      <th>등급</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1월</td>
      <td>1일</td>
      <td>〈언니〉</td>
      <td>(주)제이앤씨미디어그룹 , TCO(주)더콘텐츠온</td>
      <td>감독 임경택출연 이시영, 박세완, 이준혁, 최진호</td>
      <td>액션</td>
      <td>청소년 관람불가 (대한민국)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1월</td>
      <td>9일</td>
      <td>〈내안의 그놈〉</td>
      <td>(주)메리크리스마스</td>
      <td>감독 강효진출연 박성웅, 진영, 라미란</td>
      <td>코미디, 판타지</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1월</td>
      <td>9일</td>
      <td>〈말모이〉</td>
      <td>롯데 엔터테인먼트</td>
      <td>감독 엄유나출연 유해진, 윤계상</td>
      <td>드라마</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1월</td>
      <td>16일</td>
      <td>〈그대 이름은 장미〉</td>
      <td>(주)리틀빅픽쳐스</td>
      <td>감독 조석현출연 유호정, 박성웅, 오정세, 채수빈, 하연수</td>
      <td>코미디</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1월</td>
      <td>16일</td>
      <td>〈언더독〉</td>
      <td>(주)NEW</td>
      <td>감독 오성윤, 이춘백출연 도경수, 박소담, 박철민</td>
      <td>애니메이션</td>
      <td>전체 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>109</th>
      <td>12월</td>
      <td>4일</td>
      <td>〈너의 여자친구〉</td>
      <td>(주)스톰픽쳐스코리아, 와이드 릴리즈(주)</td>
      <td>감독 이장희 출연 이엘리야, 지일주, 허정민, 김기두, 이진이</td>
      <td>코미디, 멜로, 로맨스</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>110</th>
      <td>12월</td>
      <td>12일</td>
      <td>〈속물들〉</td>
      <td>(주)삼백상회 300 &amp; Co.</td>
      <td>감독 신아가, 이상철 출연 유다인, 심희섭, 송재림</td>
      <td>블랙코미디</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>111</th>
      <td>12월</td>
      <td>18일</td>
      <td>〈시동〉</td>
      <td>넥스트 엔터테인먼트 월드</td>
      <td>감독 최정열 출연 마동석, 박정민, 정해인, 염정아</td>
      <td>드라마</td>
      <td>15세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>112</th>
      <td>12월</td>
      <td>19일</td>
      <td>〈백두산〉</td>
      <td>(주)CJ ENM</td>
      <td>감독 이해준, 김병서출연 이병헌, 하정우, 마동석, 전혜진, 배수지</td>
      <td>어드벤처, 드라마</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
    <tr>
      <th>113</th>
      <td>12월</td>
      <td>26일</td>
      <td>〈천문: 하늘에 묻는다〉</td>
      <td>롯데엔터테인먼트</td>
      <td>감독 허진호 출연 최민식, 한석규</td>
      <td>사극</td>
      <td>12세 관람가 (대한민국)</td>
    </tr>
  </tbody>
</table>
<p>114 rows × 7 columns</p>
</div>

이를 csv 파일로 저장합니다.

```python
# 데이터프레임 저장
df.to_csv('WikipediaTable.csv')
```

## 전체 Code

```python
### Crawling ###

# 위키피디아 표 데이터 크롤링 함수 #
def CrawlingWikipediaTable(url, tag, columns):
    """
    url -> str : 크롤링 하고자 하는 위키피디아 url
    tag -> str : 크롤링 하고자 하는 위키피디아 표 태그의 class
    columns -> list : 데이터프레임 컬럼명
    """
    
    import requests
    from bs4 import BeautifulSoup
    from html_table_parser import parser_functions as parser
    import pandas as pd

    req = requests.get(url)
    html = req.text
    soup = BeautifulSoup(html, 'html.parser')
    tables = soup.find_all('table', attrs={'class':tag})
    temp = []
    for table in tables:
        table = parser.make2d(table)
        table = pd.DataFrame(table[1:])
        temp.append(table)
    res = pd.concat(temp)

    res.columns = columns
    res.dropna(inplace=True)
    res.reset_index(drop=True, inplace=True)

    return res
```