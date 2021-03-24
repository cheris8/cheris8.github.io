---
title: '[텍스트 마이닝] 문서 유사도 - 유클리드 거리 & 코사인 유사도 & 자카드 유사도'

categories:
  - Data Analysis
tags:
  - Text Mining

last_modified_at: 2020-09-10T08:06:00-05:00

classes: wide
use_math: true
---

문서들 간의 유사도를 측정하기 위하여 **유사도 척도**를 사용합니다. 유사도 척도는 크게 **거리 계수**와 **유사 계수**로 구분합니다.

- **거리 계수**는 두 문서 $X$, $Y$ 간의 거리를 측정하는 것으로, 유클리드 거리가 대표적입니다.
- **유사 계수**는 두 문서 $X$, $Y$ 간의 유사성을 측정하는 것으로, 코사인 계수, 자카드 계수 등이 대표적입니다.

## 1. 유클리드 거리 (Euclidean distance)

다차원 공간에서 두개의 점 $p$와 $q$가 각각 $p = \left( p_1, p_2, p_3, ..., p_n \right)$과 $q = \left( q_1, q_2, q_3, ..., q_n \right)$의 좌표를 가질 때 두 점 사이의 거리를 계산하는 유클리드 거리 공식은 다음과 같습니다.

$ \sqrt{\left( q_1 - p_1 \right) ^2 + \left( q_2 - p_2 \right) ^2 + \cdots \left( q_n - p_n \right) ^2} = \sqrt{\sum_{i=1}^n \left( q_i - p_i \right) ^2} $

문서 간의 유사성을 나타내는 유사 계수와는 달리, 유클리드 거리는 문서 간의 거리를 나타내는 거리 계수입니다. 따라서 유클리드 거리의 값이 작을수록 문서 간의 유사도는 높은 반면 유클리드 거리의 값이 클수록 문서 간의 유사도는 낮습니다.

유클리드 거리를 사용하여 여러 문서들 간의 유사도를 구한다는 것은 $n$차원 공간($n$: 단어 전체 개수)에 문서들을 배치하는 것을 의미합니다.

예를 들어 아래와 같은 DTM이 있다고 가정해봅시다.

|-|바나나|사과|저는|좋아요|
|-|----|---|---|----|
|문서1|2|3|0|1|
|문서2|1|2|3|1|
|문서3|2|1|2|2|

이때 아래와 같은 문서Q에 대하여 문서1, 문서2, 문서3 중 가장 유사한 문서를 찾아내고자 합니다.

|-|바나나|사과|저는|좋아요|
|-|----|---|---|----|
|문서Q|1|1|0|1|

DTM에서 단어의 개수가 4개이므로, 유클리드 거리를 사용할 때에는 4차원 공간이 될 것입니다. 다시 말해, 4차원 공간에 문서1, 문서2, 문서3을 배치한 후 문서Q 또한 다른 문서들과 마찬가지로 4차원 공간에 배치하여 4차원 공간에서 문서Q와 각각의 문서들과의 유클리드 거리를 계산함으로써 문서 유사도를 구할 수 있습니다.

이를 파이썬 코드로 구현해보면 다음과 같습니다.

```python
import numpy as np
```

```python
# 유클리드 거리를 계산하는 함수
def dist(x,y):
    return np.sqrt(np.sum((x-y)**2))
```

```python
# document representation
doc1 = np.array((2,3,0,1))
doc2 = np.array((1,2,3,1))
doc3 = np.array((2,1,2,2))
docQ = np.array((1,1,0,1))
```

```python
# 문서 유사도 출력
print(dist(docQ, doc1))
print(dist(docQ, doc2))
print(dist(docQ, doc3))
```

    2.23606797749979
    3.1622776601683795
    2.449489742783178

문서Q와 가장 유사한 문서는 문서1임을 확인할 수 있습니다.

## 2. 코사인 유사도 (Cosine Similarity)

코사인 유사도(Cosine Similarity)는 두 벡터 간의 코사인 각도를 이용하여 구하는 두 벡터의 유사도를 의미합니다. 두 벡터 $A, B$에 대한 코사인 유사도는 다음과 같습니다.

$ similarity = \cos \left( \Theta \right) = \frac{A \centerdot B}{\lVert A \rVert \lVert B \rVert} = \frac{\sum_{i=1}^n A_i \times B_i}{\sqrt{\sum_{i=1}^n \left( A_i \right) ^2} \sqrt{\sum_{i=1}^n \left( B_i \right) ^2}} $

코사인 유사도는 -1 이상 1 이하의 값을 갖습니다. 이때 코사인 유사도 값이 1에 가까울수록 유사도가 높다고 판단하고, 코사인 유사도 값이 -1에 가까울수록 유사도가 낮다고 판단합니다.

직관적으로, 코사인 유사도는 두 벡터가 가리키는 방향이 얼마나 유사한지를 의미합니다.

![]({{site.url}}/assets/images/DA/TM/코사인유사도.PNG)

- 두 벡터가 $0^\circ$의 각을 이루면 1을 값으로 갖습니다. (두 벡터가 같은 방향일 경우)
- 두 벡터가 $90^\circ$의 각을 이루면 0을 값으로 갖습니다.
- 두 벡터가 $180^\circ$의 각을 이루면 -1의 값을 갖습니다. (두 벡터가 반대 방향일 경우)

DTM 혹은 TF-IDF 행렬을 바탕으로 문서 유사도를 구하는 경우, DTM 혹은 TF-IDF 행렬이 각각의 특징 벡터 $A, B$가 됩니다. 아래와 같이 세 문서가 있다고 가정해봅시다.

문서1 : 저는 사과 좋아요  
문서2 : 저는 바나나 좋아요  
문서3 : 저는 바나나 좋아요 저는 바나나 좋아요  

세 문서에 대해서 DTM을 만들면 아래와 같습니다.

|-|바나나|사과|저는|좋아요|
|-|----|---|---|----|
|문서1|0|1|1|1|
|문서2|1|0|1|1|
|문서3|2|0|2|2|

파이썬에는 코사인 유사도를 구하는 여러가지 방법이 있습니다. 이번에는 `numpy` 패키지를 사용하여 코사인 유사도를 계산하는 코드를 구현해보겠습니다.

```python
import numpy as np
from numpy import dot
from numpy.linalg import norm
```

```python
# 코사인 유사도를 계산하는 함수
def cos_sim(A, B):
    return dot(A, B)/(norm(A)*norm(B))
```

```python
# document representation
doc1 = np.array([0,1,1,1])
doc2 = np.array([1,0,1,1])
doc3 = np.array([2,0,2,2])
```

```python
# 문서 유사도 출력
print(cos_sim(doc1, doc2)) #문서1과 문서2의 코사인 유사도
print(cos_sim(doc1, doc3)) #문서1과 문서3의 코사인 유사도
print(cos_sim(doc2, doc3)) #문서2과 문서3의 코사인 유사도
```

    0.6666666666666667
    0.6666666666666667
    1.0000000000000002

주목해야 할 점은 문서2와 문서3의 코사인 유사도 값이 1이라는 점입니다. 앞서 언급했듯이, 코사인 유사도가 1을 값으로 갖는다는 것은 두 벡터의 방향이 완전히 동일하다는 것을 의미합니다. DTM을 살펴보면, 문서3은 문서2에서 모든 단어의 빈도 수가 1씩 증가한 것임을 볼 수 있습니다. 다시 말해 특정 문서 내의 모든 단어의 빈도 수가 동일하게 증가한 문서의 경우에는 해당 문서와의 코사인 유사도의 값이 1이라는 것입니다. **이는 코사인 유사도가 벡터의 크기가 아니라 벡터의 방향(패턴)에 초점을 둔다는 점에 기인합니다.** 코사인 유사도를 사용하지 않고 문서 A와의 유사도를 구한다고 가정해봅시다. 다른 문서들이 거의 동일한 패턴을 가지는 문서임에도 불구하고, 문서 B가 단지 다른 문서들보다 원문 길이가 긴 문서라는 이유로 유사도가 더 높게 나올 수 있습니다. **결론적으로 코사인 유사도는 각 문서들의 길이가 다른 상황에서 비교적 공정한 비교를 할 수 있도록 도와줍니다.**

## 3. 자카드 유사도(Jaccard similarity)

자카드 유사도(Jaccard similarity)는 두 집합 A와 B가 있다고 할 때 합집합에서의 교집합의 비율을 의미합니다. 두 집합이 동일할 때 1의 값을 갖고 두 집합이 공통으로 갖는 원소가 없을 때 0의 값을 갖습니다.

자카드 유사도를 구하는 함수를 $J$라고 할 때, 자카드 유사도 함수 $J$는 다음과 같습니다.

$ J \left( A, B \right) = \frac{\left\vert A \cap B \right\vert}{\left\vert A \cup B \right\vert} = \frac{\left\vert A \cap B \right\vert}{\left\vert A \right\vert + \left\vert B \right\vert - \left\vert A \cap B \right\vert} $

두 문서 $d_1$와 $d_2$가 있다고 할 때, $d_1$와 $d_2$의 문서 유사도를 구하기 위한 자카드 유사도는 다음과 같습니다.

$ J \left( d_1, d_2 \right) = \frac{d_1 \cap d_2}{d_1 \cup d_2} $

즉, 두 문서 $d_1$, $d_2$ 사이의 자카드 유사도 $J \left( d_1, d_2 \right)$는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의됩니다. 

파이썬 코드를 통해 간단하게 이해해보도록 하겠습니다.

```python
doc1 = "apple banana everyone like likey watch card holder"
doc2 = "apple banana coupon passport love you"
```

```python
# tokenization
tokenized_doc1 = doc1.split()
tokenized_doc2 = doc2.split()
```

```python
# tokenization 결과 출력
print(tokenized_doc1)
print(tokenized_doc2)
```

    ['apple', 'banana', 'everyone', 'like', 'likey', 'watch', 'card', 'holder']
    ['apple', 'banana', 'coupon', 'passport', 'love', 'you']

```python
# 합집합
union = set(tokenized_doc1).union(set(tokenized_doc2))
print(union)
```

    {'apple', 'watch', 'love', 'passport', 'coupon', 'likey', 'you', 'like', 'everyone', 'banana', 'card', 'holder'}

```python
# 교집합
intersection = set(tokenized_doc1).intersection(set(tokenized_doc2))
print(intersection)
```

    {'banana', 'apple'}

```python
# 문서 유사도 출력
print(len(intersection)/len(union))
```

    0.16666666666666666

이 값은 자카드 유사도이자, 두 문서의 전체 단어 집합에서 두 문서에서 공통적으로 등장한 단어의 비율이기도 합니다.

## 4. 실습 Code : 유사도를 이용한 추천 시스템 구현하기

캐글의 영화 데이터셋을 바탕으로 영화 추천 시스템을 만들어보겠습니다. TF-IDF와 코사인 유사도를 활용하여 영화의 줄거리에 기반하여 영화를 추천하는 추천 시스템을 만들 수 있습니다. 영화 제목을 입력하면, 줄거리가 해당 영화와 유사한 영화를 찾아 추천해줍니다.

다운로드 링크 : <https://www.kaggle.com/rounakbanik/the-movies-dataset>  
파일명 : movies_metadata.csv

```python
import pandas as pd
```

```python
# 데이터 불러오기
data = pd.read_csv('./data/movies_metadata.csv', low_memory=False)
data.head()
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
      <th>adult</th>
      <th>belongs_to_collection</th>
      <th>budget</th>
      <th>genres</th>
      <th>homepage</th>
      <th>id</th>
      <th>imdb_id</th>
      <th>original_language</th>
      <th>original_title</th>
      <th>overview</th>
      <th>...</th>
      <th>release_date</th>
      <th>revenue</th>
      <th>runtime</th>
      <th>spoken_languages</th>
      <th>status</th>
      <th>tagline</th>
      <th>title</th>
      <th>video</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>{'id': 10194, 'name': 'Toy Story Collection', ...</td>
      <td>30000000</td>
      <td>[{'id': 16, 'name': 'Animation'}, {'id': 35, '...</td>
      <td>http://toystory.disney.com/toy-story</td>
      <td>862</td>
      <td>tt0114709</td>
      <td>en</td>
      <td>Toy Story</td>
      <td>Led by Woody, Andy's toys live happily in his ...</td>
      <td>...</td>
      <td>1995-10-30</td>
      <td>373554033.0</td>
      <td>81.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>NaN</td>
      <td>Toy Story</td>
      <td>False</td>
      <td>7.7</td>
      <td>5415.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>NaN</td>
      <td>65000000</td>
      <td>[{'id': 12, 'name': 'Adventure'}, {'id': 14, '...</td>
      <td>NaN</td>
      <td>8844</td>
      <td>tt0113497</td>
      <td>en</td>
      <td>Jumanji</td>
      <td>When siblings Judy and Peter discover an encha...</td>
      <td>...</td>
      <td>1995-12-15</td>
      <td>262797249.0</td>
      <td>104.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}, {'iso...</td>
      <td>Released</td>
      <td>Roll the dice and unleash the excitement!</td>
      <td>Jumanji</td>
      <td>False</td>
      <td>6.9</td>
      <td>2413.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>{'id': 119050, 'name': 'Grumpy Old Men Collect...</td>
      <td>0</td>
      <td>[{'id': 10749, 'name': 'Romance'}, {'id': 35, ...</td>
      <td>NaN</td>
      <td>15602</td>
      <td>tt0113228</td>
      <td>en</td>
      <td>Grumpier Old Men</td>
      <td>A family wedding reignites the ancient feud be...</td>
      <td>...</td>
      <td>1995-12-22</td>
      <td>0.0</td>
      <td>101.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>Still Yelling. Still Fighting. Still Ready for...</td>
      <td>Grumpier Old Men</td>
      <td>False</td>
      <td>6.5</td>
      <td>92.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>NaN</td>
      <td>16000000</td>
      <td>[{'id': 35, 'name': 'Comedy'}, {'id': 18, 'nam...</td>
      <td>NaN</td>
      <td>31357</td>
      <td>tt0114885</td>
      <td>en</td>
      <td>Waiting to Exhale</td>
      <td>Cheated on, mistreated and stepped on, the wom...</td>
      <td>...</td>
      <td>1995-12-22</td>
      <td>81452156.0</td>
      <td>127.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>Friends are the people who let you be yourself...</td>
      <td>Waiting to Exhale</td>
      <td>False</td>
      <td>6.1</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>{'id': 96871, 'name': 'Father of the Bride Col...</td>
      <td>0</td>
      <td>[{'id': 35, 'name': 'Comedy'}]</td>
      <td>NaN</td>
      <td>11862</td>
      <td>tt0113041</td>
      <td>en</td>
      <td>Father of the Bride Part II</td>
      <td>Just when George Banks has recovered from his ...</td>
      <td>...</td>
      <td>1995-02-10</td>
      <td>76578911.0</td>
      <td>106.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>Just When His World Is Back To Normal... He's ...</td>
      <td>Father of the Bride Part II</td>
      <td>False</td>
      <td>5.7</td>
      <td>173.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>

코드가 돌아가는 데에 걸리는 시간을 고려하여 20000개의 샘플만을 활용합니다. 위 데이터셋 중 영화 제목에 해당하는 title 열과 영화 줄거리에 해당하는 overview 열을 사용합니다.

```python
data = data.head(20000)
```

이후에 `TfidfVectorizer()`를 사용하여 TF-IDF를 구하려면, 데이터에 결측치가 있어서는 안 됩니다. 따라서 결측치가 존재하는지 확인합니다.

```python
# NA 확인
data['overview'].isnull().sum()
```

    135

결측치가 135개 존재하므로 이를 빈칸으로 대체합니다.

```python
# NA imputation
data['overview'] = data['overview'].fillna('')
```

```python
# NA 확인
data['overview'].isnull().sum()
```

    0

```python
# tf-idf
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf = TfidfVectorizer(stop_words='english') # tf-idf 선언
tfidf_matrix = tfidf.fit_transform(data['overview']) # tf-idf 적합
print(tfidf_matrix.shape)
```

    (20000, 47487)

`tfidf_matrix`가 20000개의 문서(즉 영화)와 47487개의 단어로 이루어진 DTM임을 볼 수 있습니다.

```python
# 코사인 유사도
from sklearn.metrics.pairwise import linear_kernel
cosine_sim = linear_kernel(tfidf_matrix, tfidf_matrix)
print(cosine_sim.shape)
```

    (20000, 20000)

영화 제목을 입력하면 인덱스를 반환할 수 있도록 영화 제목과 인덱스를 갖는 테이블을 생성합니다.

```python
# 테이블 생성
indices = pd.Series(data.index, index=data['title']).drop_duplicates()
print(indices)
```

    title
    Toy Story                                                                       0
    Jumanji                                                                         1
    Grumpier Old Men                                                                2
    Waiting to Exhale                                                               3
    Father of the Bride Part II                                                     4
                                                                                ...  
    Rebellion                                                                   19995
    Versailles                                                                  19996
    Two in the Wave                                                             19997
    Lotte Reiniger: Homage to the Inventor of the Silhouette Film               19998
    RKO Production 601: The Making of 'Kong, the Eighth Wonder of the World'    19999
    Length: 20000, dtype: int64

```python
# 확인
idx = indices['Father of the Bride Part II']
print(idx)
```

    4

이제 영화의 제목을 입력받아 입력받은 영화와 overview의 관점에서 코사인 유사도를 기준으로 가장 유사도가 높은 상위 10개 영화의 제목을 반환하는 함수를 선언합니다.

```python
# overview를 바탕으로 가장 유사도가 높은 상위 10개 영화를 추천하는 함수 선언
def get_recommendations(title, cosine_sim=cosine_sim):
    
    # 해당 영화 제목에 대응되는 인덱스를 저장
    idx = indices[title]

    # 모든 영화에 대하여 해당 영화와의 유사도를 계산
    sim_scores = list(enumerate(cosine_sim[idx])) # (인덱스, 유사도) 를 원소로 갖는 리스트

    # 유사도를 기준으로 정렬
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True) 

    # 유사도 기준 상위 10개 영화 선택 
    sim_scores = sim_scores[1:11] # (인덱스, 유사도) 를 원소로 갖는 리스트

    # 상위 10개 영화 인덱스를 저장
    movie_indices = [i[0] for i in sim_scores]

    # 가장 유사도가 높은 상위 10개의 영화 제목을 반환
    return data['title'].iloc[movie_indices]
```

영화 'The Dark Knight Rises'와 줄거리가 유사한 영화들을 찾아보겠습니다.

```python
get_recommendations('The Dark Knight Rises')
```

    12481                            The Dark Knight
    150                               Batman Forever
    1328                              Batman Returns
    15511                 Batman: Under the Red Hood
    585                                       Batman
    9230          Batman Beyond: Return of the Joker
    18035                           Batman: Year One
    19792    Batman: The Dark Knight Returns, Part 1
    3095                Batman: Mask of the Phantasm
    10122                              Batman Begins
    Name: title, dtype: object

영화 'The Dark Knight Rises'와 줄거리가 가장 유사한 영화는 영화 'The Dark Knight'입니다. 이외에 추천받은 영화들 모두 배트맨 영화임을 볼 수 있습니다.

### 5. 참고자료
[딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/book/2155)

