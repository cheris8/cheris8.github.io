---
title: '[데이터 전처리] 위키피디아 표 데이터 전처리 하기'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Data Preprocessing
  - Text Preprocessing
  - Python

last_modified_at: 2021-03-11T08:06:00-05:00

classes: wide
---

이 글은 [위키피디아](https://ko.wikipedia.org/wiki/위키백과:대문)에 존재하는 표 형태의 데이터를 전처리 하는 방법에 관한 기록입니다. [위키피디아](https://ko.wikipedia.org/wiki/위키백과:대문)에 존재하는 표 형태의 데이터를 크롤링 하는 방법에 관한 기록은 [이 곳]({{site.url}}/data%20analysis/DC-Crawling-Wikipedia-Table/)에서 볼 수 있습니다.

## 실습 Code

먼저 필요한 라이브러리를 불러옵니다.

```python
import pandas as pd
import re
```

그리고 일전에 저장해둔 데이터셋을 불러옵니다.

```python
df = pd.read_csv('WikipediaTable.csv', index_col=0)
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

데이터프레임의 간략한 정보를 확인합니다.

```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 114 entries, 0 to 113
    Data columns (total 7 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   개봉월     114 non-null    object
     1   개봉일     114 non-null    object
     2   제목      114 non-null    object
     3   제작/배급   114 non-null    object
     4   출연/제작진  114 non-null    object
     5   장르      114 non-null    object
     6   등급      114 non-null    object
    dtypes: object(7)
    memory usage: 7.1+ KB

### 제목 전처리

데이터셋의 제목 컬럼에 대한 전처리 함수를 선언합니다.

```python
def PreprocessTitle(string):
    import re
    p = re.compile('[^〈〉]+')
    return p.search(string).group()
```

이를 적용합니다.

```python
df['제목'] = df['제목'].apply(PreprocessTitle)
```

### 제작/배급 및 장르 전처리

데이터셋의 제작/배급 컬럼과 장르 컬럼에 대한 전처리 함수를 선언합니다.

```python
def StringtoList(string):
    ls = string.split(', ')
    for i in range(len(ls)):
        ls[i] = ls[i].strip()   
    return ls
```

이를 적용합니다.

```python
df['제작/배급'] = df['제작/배급'].apply(StringtoList)
df['장르'] = df['장르'].apply(StringtoList)
```

### 출연/제작진 전처리

데이터셋의 출연/제작진 컬럼에 대한 전처리 함수를 선언합니다.

```python
def PreprocessCrew(string):
    import re
    ls = string.split('출연 ')
    p = re.compile('(?<=감독 ).+')
    director = p.search(ls[0]).group().strip().split(', ')
    cast = ls[1].split(', ')
    return director, cast
```

이를 적용합니다.

```python
df['감독'] = [PreprocessCrew(crew)[0] for crew in df['출연/제작진']]
df['출연'] = [PreprocessCrew(crew)[1] for crew in df['출연/제작진']]
```

새롭게 감독 컬럼과 출연 컬럼을 생성했으므로 기존의 출연/제작진 컬럼은 삭제합니다.

```python
df.drop(['출연/제작진'], axis=1, inplace=True)
```

### 등급 전처리

데이터셋의 등급 컬럼에 대한 전처리 함수를 선언합니다.

```python
def PreprocessRating(string):
    
    import re
    
    p = re.compile('.+(?=관람)')
    result = p.match(string).group()
    result = result.strip()
    
    if result=='청소년':
        result = '19세'
    
    return result
```

이를 적용합니다.

```python
df['등급'] = df['등급'].apply(PreprocessRating)
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
      <th>장르</th>
      <th>등급</th>
      <th>감독</th>
      <th>출연</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1월</td>
      <td>1일</td>
      <td>언니</td>
      <td>[(주)제이앤씨미디어그룹, TCO(주)더콘텐츠온]</td>
      <td>[액션]</td>
      <td>19세</td>
      <td>[임경택]</td>
      <td>[이시영, 박세완, 이준혁, 최진호]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1월</td>
      <td>9일</td>
      <td>내안의 그놈</td>
      <td>[(주)메리크리스마스]</td>
      <td>[코미디, 판타지]</td>
      <td>15세</td>
      <td>[강효진]</td>
      <td>[박성웅, 진영, 라미란]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1월</td>
      <td>9일</td>
      <td>말모이</td>
      <td>[롯데 엔터테인먼트]</td>
      <td>[드라마]</td>
      <td>12세</td>
      <td>[엄유나]</td>
      <td>[유해진, 윤계상]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1월</td>
      <td>16일</td>
      <td>그대 이름은 장미</td>
      <td>[(주)리틀빅픽쳐스]</td>
      <td>[코미디]</td>
      <td>12세</td>
      <td>[조석현]</td>
      <td>[유호정, 박성웅, 오정세, 채수빈, 하연수]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1월</td>
      <td>16일</td>
      <td>언더독</td>
      <td>[(주)NEW]</td>
      <td>[애니메이션]</td>
      <td>전체</td>
      <td>[오성윤, 이춘백]</td>
      <td>[도경수, 박소담, 박철민]</td>
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
      <td>...</td>
    </tr>
    <tr>
      <th>109</th>
      <td>12월</td>
      <td>4일</td>
      <td>너의 여자친구</td>
      <td>[(주)스톰픽쳐스코리아, 와이드 릴리즈(주)]</td>
      <td>[코미디, 멜로, 로맨스]</td>
      <td>12세</td>
      <td>[이장희]</td>
      <td>[이엘리야, 지일주, 허정민, 김기두, 이진이]</td>
    </tr>
    <tr>
      <th>110</th>
      <td>12월</td>
      <td>12일</td>
      <td>속물들</td>
      <td>[(주)삼백상회 300 &amp; Co.]</td>
      <td>[블랙코미디]</td>
      <td>15세</td>
      <td>[신아가, 이상철]</td>
      <td>[유다인, 심희섭, 송재림]</td>
    </tr>
    <tr>
      <th>111</th>
      <td>12월</td>
      <td>18일</td>
      <td>시동</td>
      <td>[넥스트 엔터테인먼트 월드]</td>
      <td>[드라마]</td>
      <td>15세</td>
      <td>[최정열]</td>
      <td>[마동석, 박정민, 정해인, 염정아]</td>
    </tr>
    <tr>
      <th>112</th>
      <td>12월</td>
      <td>19일</td>
      <td>백두산</td>
      <td>[(주)CJ ENM]</td>
      <td>[어드벤처, 드라마]</td>
      <td>12세</td>
      <td>[이해준, 김병서]</td>
      <td>[이병헌, 하정우, 마동석, 전혜진, 배수지]</td>
    </tr>
    <tr>
      <th>113</th>
      <td>12월</td>
      <td>26일</td>
      <td>천문: 하늘에 묻는다</td>
      <td>[롯데엔터테인먼트]</td>
      <td>[사극]</td>
      <td>12세</td>
      <td>[허진호]</td>
      <td>[최민식, 한석규]</td>
    </tr>
  </tbody>
</table>
<p>114 rows × 8 columns</p>
</div>

## 전체 Code

```python
### Preprocessing ###

# 변수 제목 전처리 함수 #
def PreprocessTitle(string):
    import re
    p = re.compile('[^〈〉]+')
    return p.search(string).group()

# 변수 제작/배급, 장르 전처리 함수 #
def StringtoList(string):
    ls = string.split(', ')
    for i in range(len(ls)):
        ls[i] = ls[i].strip()
    return ls

# 변수 출연/제작진 전처리 함수 #
def PreprocessCrew(string):
    import re
    ls = string.split('출연 ')
    p = re.compile('(?<=감독 ).+')
    director = p.search(ls[0]).group().strip().split(', ')
    cast = ls[1].split(', ')
    return director, cast

# 변수 등급 전처리 함수 #
def PreprocessRating(string):
    import re
    p = re.compile('.+(?=관람)')
    result = p.match(string).group()
    result = result.strip()
    if result == '청소년':
        result = '19세'
    return result

# 전체 데이터셋 전처리 함수 #
def Preprocessor(data):
    data['제목'] = data['제목'].apply(PreprocessTitle)
    data['제작/배급'] = data['제작/배급'].apply(StringtoList)
    data['감독'] = [PreprocessCrew(crew)[0] for crew in data['출연/제작진']]
    data['출연'] = [PreprocessCrew(crew)[1] for crew in data['출연/제작진']]
    data['장르'] = data['장르'].apply(StringtoList)
    data['등급'] = data['등급'].apply(PreprocessRating)
    data.drop(['출연/제작진'], axis=1, inplace=True)
    return data
```