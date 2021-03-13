---
title: '[데이터 크롤링] 정적 웹 페이지 크롤링 with Python'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Data Crawling
  - Python

last_modified_at: 2021-03-12T08:06:00-05:00

classes: wide
---

이 글은 파이썬을 이용한 정적 크롤링에 관한 기록입니다.

## 1. Import library

정적 웹 페이지를 크롤링 하는 데에는 `requests` 라이브러리와 `Beautiful Soup` 라이브러리가 사용됩니다.

- `requests` : HTTP 관련 라이브러리
  - `requests.get()` : HTTP 요청
- `Beautiful Soup` = `bs4` : HTML/XML 파싱 관련 라이브러리
  - `BeautifulSoup()`: HTTP 응답 객체를 HTML로 변환
 
```python
import requests
from bs4 import BeautifulSoup
```

## 2. HTTP Request & HTTP Response

HTTP 요청에는 `requests` 라이브러리의 `get()` 함수를 사용합니다.

```python
req = requests.get("https://www.billboard.com/charts/hot-100")
```

이때 HTTP 응답 객체는 다음과 같은 속성을 가지고 있습니다.

- `req.status_code` : 응답 상태 코드(response status codes) 출력
- `req.headers` : 응답 헤더(response headers) 출력
- `req.encoding` : 응답 내용의 인코딩 방식 출력
- `req.text` : 응답 내용(response content) 출력
- `req.json()`

```python
req.status_code
```

    200

```python
req.headers
```

    {'Date': 'Sat, 13 Mar 2021 07:18:19 GMT', 'Content-Type': 'text/html; charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'Set-Cookie': '__cfduid=dbcff2350a762e60f08d441270f23e0e81615619899; expires=Mon, 12-Apr-21 07:18:19 GMT; path=/; domain=.billboard.com; HttpOnly; SameSite=Lax; Secure, PGMINFO=cc:kr-ip:182.228.175.191; Max-Age=3600; Path=/; Domain=.billboard.com, __cf_bm=2e0ada7307e3b1cbf7f27f793685660f9348a161-1615619899-1800-AYb5dGHxu5mMZYoPDlpkMSWyQ/x4dx9UgJ3aaL3yc1+zBP9nB6jIZizvQw9Yvyp2Hdwz8xhM06JF4VoOaQawEP12en9WryWJCs3gyC+RK6MN; path=/; expires=Sat, 13-Mar-21 07:48:19 GMT; domain=.billboard.com; HttpOnly; Secure; SameSite=None', 'CF-Ray': '62f37cd1b80112e6-ICN', 'Age': '45', 'Cache-Control': 'max-age=1, public, s-maxage=300', 'Vary': 'Accept-Encoding', 'CF-Cache-Status': 'HIT', 'cf-request-id': '08cc0c5715000012e6a5809000000001', 'Expect-CT': 'max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"', 'X-Trace': '2B8991BA12F1C995146D07597619C3CD6F657E644980ED377041F0B88D00', 'Server': 'cloudflare', 'Content-Encoding': 'gzip', 'alt-svc': 'h3-27=":443"; ma=86400, h3-28=":443"; ma=86400, h3-29=":443"; ma=86400'}

```python
req.encoding
```

    'UTF-8'

## 3. HTML Parsing

HTTP 응답 객체를 HTML로 변환하는 데에는 `bs4`의 `BeautifulSoup()` 함수를 사용합니다.

```python
html = req.text
soup = BeautifulSoup(html, 'html.parser')
```

`BeautifulSoup()` 함수에서 사용할 수 있는 파서는 다음과 같습니다.
- `html.parser`
- `lxml`
- `xml`
- `html5lib`

## 4. Extract & Preprocessing Data

CSS Selector를 사용하여 HTML로부터 수집하고자 하는 데이터를 추출하고 전처리 합니다.

```python
# 데이터 추출
rank = soup.select('li > button > span > span.chart-element__rank__number')
song = soup.select('li > button > span.chart-element__information > span.chart-element__information__song')
artist = soup.select('li > button > span.chart-element__information > span.chart-element__information__artist')
```

```python
# 데이터 전처리
top_chart = []
for e in zip(rank, song, artist):
    top_chart.append(
        {'rank':e[0].text,
         'song':e[1].text,
         'artist':e[2].text}
    )
```

```python
# 결과 확인
for i in top_chart:
    print(i)
```

    {'rank': '1', 'song': 'Drivers License', 'artist': 'Olivia Rodrigo'}
    {'rank': '2', 'song': 'Up', 'artist': 'Cardi B'}
    {'rank': '3', 'song': 'Blinding Lights', 'artist': 'The Weeknd'}
    {'rank': '4', 'song': '34+35', 'artist': 'Ariana Grande'}
    {'rank': '5', 'song': 'Go Crazy', 'artist': 'Chris Brown & Young Thug'}
    {'rank': '6', 'song': 'Save Your Tears', 'artist': 'The Weeknd'}
    {'rank': '7', 'song': 'Mood', 'artist': '24kGoldn Featuring iann dior'}
    {'rank': '8', 'song': 'Calling My Phone', 'artist': 'Lil Tjay Featuring 6LACK'}
    {'rank': '9', 'song': 'What You Know Bout Love', 'artist': 'Pop Smoke'}
    {'rank': '10', 'song': 'Levitating', 'artist': 'Dua Lipa Featuring DaBaby'}
    {'rank': '11', 'song': 'Positions', 'artist': 'Ariana Grande'}
    {'rank': '12', 'song': 'Therefore I Am', 'artist': 'Billie Eilish'}
    {'rank': '13', 'song': 'Back In Blood', 'artist': 'Pooh Shiesty Featuring Lil Durk'}
    {'rank': '14', 'song': 'For The Night', 'artist': 'Pop Smoke Featuring Lil Baby & DaBaby'}
    {'rank': '15', 'song': 'Beat Box', 'artist': 'SpotemGottem Featuring Pooh Shiesty Or DaBaby'}
    {'rank': '16', 'song': 'Dakiti', 'artist': 'Bad Bunny & Jhay Cortez'}
    {'rank': '17', 'song': 'You Broke Me First.', 'artist': 'Tate McRae'}
    {'rank': '18', 'song': 'Whoopty', 'artist': 'CJ'}
    {'rank': '19', 'song': "You're Mines Still", 'artist': 'Yung Bleu Featuring Drake'}
    {'rank': '20', 'song': 'Good Time', 'artist': 'Niko Moon'}
    {'rank': '21', 'song': "My Ex's Best Friend", 'artist': 'Machine Gun Kelly X blackbear'}
    {'rank': '22', 'song': 'Best Friend', 'artist': 'Saweetie Featuring Doja Cat'}
    {'rank': '23', 'song': 'Good Days', 'artist': 'SZA'}
    {'rank': '24', 'song': 'I Hope', 'artist': 'Gabby Barrett Featuring Charlie Puth'}
    {'rank': '25', 'song': 'On Me', 'artist': 'Lil Baby'}
    {'rank': '26', 'song': 'Streets', 'artist': 'Doja Cat'}
    {'rank': '27', 'song': 'Better Together', 'artist': 'Luke Combs'}
    {'rank': '28', 'song': 'Throat Baby (Go Baby)', 'artist': 'BRS Kash'}
    {'rank': '29', 'song': 'Holy', 'artist': 'Justin Bieber Featuring Chance The Rapper'}
    {'rank': '30', 'song': 'Put Your Records On', 'artist': 'Ritt Momney'}
    {'rank': '31', 'song': 'Anyone', 'artist': 'Justin Bieber'}
    {'rank': '32', 'song': 'Willow', 'artist': 'Taylor Swift'}
    {'rank': '33', 'song': 'Just The Way', 'artist': 'Parmalee x Blanco Brown'}
    {'rank': '34', 'song': 'Without You', 'artist': 'The Kid LAROI'}
    {'rank': '35', 'song': 'Bang!', 'artist': 'AJR'}
    {'rank': '36', 'song': 'Down To One', 'artist': 'Luke Bryan'}
    {'rank': '37', 'song': 'No More Parties', 'artist': 'Coi Leray Featuring Lil Durk'}
    {'rank': '38', 'song': 'Lemonade', 'artist': 'Internet Money & Gunna Featuring Don Toliver & NAV'}
    {'rank': '39', 'song': 'Telepatia', 'artist': 'Kali Uchis'}
    {'rank': '40', 'song': 'The Good Ones', 'artist': 'Gabby Barrett'}
    {'rank': '41', 'song': 'Lonely', 'artist': 'Justin Bieber & benny blanco'}
    {'rank': '42', 'song': 'Cry Baby', 'artist': 'Megan Thee Stallion Featuring DaBaby'}
    {'rank': '43', 'song': 'Dynamite', 'artist': 'BTS'}
    {'rank': '44', 'song': "What's Your Country Song", 'artist': 'Thomas Rhett'}
    {'rank': '45', 'song': 'My Head And My Heart', 'artist': 'Ava Max'}
    {'rank': '46', 'song': 'Damage', 'artist': 'H.E.R.'}
    {'rank': '47', 'song': 'Laugh Now Cry Later', 'artist': 'Drake Featuring Lil Durk'}
    {'rank': '48', 'song': 'Starting Over', 'artist': 'Chris Stapleton'}
    {'rank': '49', 'song': 'Astronaut In The Ocean', 'artist': 'Masked Wolf'}
    {'rank': '50', 'song': 'Time Today', 'artist': 'Moneybagg Yo'}
    {'rank': '51', 'song': "We're Good", 'artist': 'Dua Lipa'}
    {'rank': '52', 'song': 'Long Live', 'artist': 'Florida Georgia Line'}
    {'rank': '53', 'song': 'Beers And Sunshine', 'artist': 'Darius Rucker'}
    {'rank': '54', 'song': 'Heartbreak Anniversary', 'artist': 'Giveon'}
    {'rank': '55', 'song': 'Monsters', 'artist': 'All Time Low Featuring Demi Lovato & blackbear '}
    {'rank': '56', 'song': 'Body', 'artist': 'Megan Thee Stallion'}
    {'rank': '57', 'song': 'Buss It', 'artist': 'Erica Banks'}
    {'rank': '58', 'song': 'Goosebumps', 'artist': 'Travis Scott & HVME'}
    {'rank': '59', 'song': 'Golden', 'artist': 'Harry Styles'}
    {'rank': '60', 'song': 'Lady', 'artist': 'Brett Young'}
    {'rank': '61', 'song': 'Heat Waves', 'artist': 'Glass Animals'}
    {'rank': '62', 'song': 'Wasted On You', 'artist': 'Morgan Wallen'}
    {'rank': '63', 'song': 'La Noche de Anoche', 'artist': 'Bad Bunny & Rosalia'}
    {'rank': '64', 'song': 'AP', 'artist': 'Pop Smoke'}
    {'rank': '65', 'song': 'Forever After All', 'artist': 'Luke Combs'}
    {'rank': '66', 'song': 'Track Star', 'artist': 'Mooski'}
    {'rank': '67', 'song': "Momma's House", 'artist': 'Dustin Lynch'}
    {'rank': '68', 'song': 'Sand In My Boots', 'artist': 'Morgan Wallen'}
    {'rank': '69', 'song': 'Girl Like Me', 'artist': 'Black Eyed Peas X Shakira'}
    {'rank': '70', 'song': 'Hell Of A View', 'artist': 'Eric Church'}
    {'rank': '71', 'song': 'Lifestyle', 'artist': 'Jason Derulo Featuring Adam Levine'}
    {'rank': '72', 'song': 'Masterpiece', 'artist': 'DaBaby'}
    {'rank': '73', 'song': 'Somebody Like That', 'artist': 'Tenille Arts'}
    {'rank': '74', 'song': 'Only Wanna Be With You', 'artist': 'Post Malone'}
    {'rank': '75', 'song': 'One Too Many', 'artist': 'Keith Urban Duet With P!nk'}
    {'rank': '76', 'song': 'Neighbors', 'artist': 'Pooh Shiesty Featuring BIG30'}
    {'rank': '77', 'song': 'You Got It', 'artist': 'VEDO'}
    {'rank': '78', 'song': 'Tyler Herro', 'artist': 'Jack Harlow'}
    {'rank': '79', 'song': 'Finesse Out The Gang Way', 'artist': 'Lil Durk Featuring Lil Baby'}
    {'rank': '80', 'song': 'Opp Stoppa', 'artist': 'YBN Nahmir Featuring 21 Savage'}
    {'rank': '81', 'song': 'The Business', 'artist': 'Tiesto'}
    {'rank': '82', 'song': 'Bandido', 'artist': 'Myke Towers & Juhn'}
    {'rank': '83', 'song': 'Quicksand', 'artist': 'Morray'}
    {'rank': '84', 'song': 'Almost Maybes', 'artist': 'Jordan Davis'}
    {'rank': '85', 'song': 'Hello', 'artist': 'Pop Smoke Featuring A Boogie Wit da Hoodie'}
    {'rank': '86', 'song': "Still Trappin'", 'artist': 'Lil Durk & King Von'}
    {'rank': '87', 'song': 'Moonwalking In Calabasas', 'artist': 'DDG'}
    {'rank': '88', 'song': 'Made For You', 'artist': 'Jake Owen'}
    {'rank': '89', 'song': 'Hole In The Bottle', 'artist': 'Kelsea Ballerini'}
    {'rank': '90', 'song': "Drunk (And I Don't Wanna Go Home)", 'artist': 'Elle King & Miranda Lambert'}
    {'rank': '91', 'song': 'Glad You Exist', 'artist': 'Dan + Shay'}
    {'rank': '92', 'song': "Breaking Up Was Easy In The 90's", 'artist': 'Sam Hunt'}
    {'rank': '93', 'song': "Somebody's Problem", 'artist': 'Morgan Wallen'}
    {'rank': '94', 'song': 'Drankin N Smokin', 'artist': 'Future & Lil Uzi Vert'}
    {'rank': '95', 'song': 'Pick Up Your Feelings', 'artist': 'Jazmine Sullivan'}
    {'rank': '96', 'song': 'Nobody', 'artist': 'Dylan Scott'}
    {'rank': '97', 'song': 'Cover Me Up', 'artist': 'Morgan Wallen'}
    {'rank': '98', 'song': 'Like I Want You', 'artist': 'Giveon'}
    {'rank': '99', 'song': 'Gone', 'artist': 'Dierks Bentley'}
    {'rank': '100', 'song': 'Bichota', 'artist': 'Karol G'}

## 5. Save Data

데이터를 데이터프레임 형태로 변환하여 csv 파일로 저장합니다.

```python
import pandas as pd
df = pd.DataFrame(top_chart)
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
      <th>rank</th>
      <th>song</th>
      <th>artist</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Drivers License</td>
      <td>Olivia Rodrigo</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Up</td>
      <td>Cardi B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Blinding Lights</td>
      <td>The Weeknd</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>34+35</td>
      <td>Ariana Grande</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Go Crazy</td>
      <td>Chris Brown &amp; Young Thug</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>96</td>
      <td>Nobody</td>
      <td>Dylan Scott</td>
    </tr>
    <tr>
      <th>96</th>
      <td>97</td>
      <td>Cover Me Up</td>
      <td>Morgan Wallen</td>
    </tr>
    <tr>
      <th>97</th>
      <td>98</td>
      <td>Like I Want You</td>
      <td>Giveon</td>
    </tr>
    <tr>
      <th>98</th>
      <td>99</td>
      <td>Gone</td>
      <td>Dierks Bentley</td>
    </tr>
    <tr>
      <th>99</th>
      <td>100</td>
      <td>Bichota</td>
      <td>Karol G</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 3 columns</p>
</div>

```python
df.to_csv('df.csv')
```

