---
title: '[Python] 판다스 (Pandas) - 데이터프레임 (DataFrame)'

categories:
  - Python
tags:
  - Python
  - Pandas

last_modified_at: 2021-03-22T08:06:00-05:00

classes: wide
---

이 글은 판다스의 데이터프레임을 다루는 방법에 관한 기록입니다.

- `pd.set_option('display.max_rows, None')` : 데이터프레임의 모든 행 출력
- `pd.set_option('display.max_columns, None')` : 데이터프레임의 모든 열 출력

## Attributes

- `df.index` : 데이터프레임의 인덱스(row label) 반환
- `df.columns` : 데이터프레임의 컬럼명(column label) 반환

- `df.ndim` : 축의 개수를 나타내는 int 반환
  - Series일 때 1 반환
  - DataFrame일 때 2 반환
- `df.size`: 데이터프레임의 요소들의 개수를 나타내는 int 반환
  - Series일 때 행의 개수 반환
  - DataFrame일 때 행의 개수 x 열의 개수 반환
- `df.shape` : 데이터프레임의 차원을 나타내는 tuple 반환 : `(행의 개수, 열의 개수)`

- `df.dtypes` : 데이터프레임 내 컬럼 별 데이터 타입 반환

- `df.values` : 데이터프레임에 대한 numpy 표현 즉 array 반환

- `df.iloc` : 위치를 나타내는 integer를 통해 데이터 프레임 인덱싱
- `df.loc` : Access a group of rows and columns by label(s) or a boolean array. label 혹은 boolean array를 통해 데이터프레임 인덱싱

## Methods

- `df.copy()` : `df`를 복사

- `df.head(n)` : 위에서부터 n개의 행을 반환
- `df.tail(n)` : 아래에서부터 n개의 행을 반환
- `df.info()` : 데이터프레임에 대한 간결한 요약 출력
- `df.nunique()` : 데이터프레임 내 unique한 값들의 개수 반환
- `df.unique()` : 데이터프레임 내 unique한 값들 반환

- `df.describe()` : 기초통계량
- `df.max()` : 최대값
- `df.mean()` : 평균값
- `df.median()` : 중앙값
- `df.min()` : 최소값

- `df.pivot()`
- `df.pivot_table()`

- `df.apply()`
- `df.agg()`
- `df.aggregate()`
- `df.grooupby()`

- `df.replace()`

- `df.sort_values(by, axis={0, 1}, ascending={True, False}, inplace={True, False})` : 데이터프레임 정렬
  - `axis=0` / `axis=index` 일 때 `by` : index levels 혹은 column labels
  - `axis=1` / `axis=columns` 일 때 `by` : column levels 혹은 index labels
  - `ascending=True` : 오름차순 정렬 (default)
  - `ascending=False` : 내림차순 정렬

- `df.value_counts()`

- `df.isna()` : 결측치 확인
- `df.isnull()` : 결측치 확인
- `df.notna()` : 결측치 확인
- `df.notnull()` : 결측치 확인
- `df.dropna(axis={0, 1}, inplace={True, False})` : 결측치 제거
- `df.fillna(value=None, axis=None, inplace={True, False})` : 결측치 대체

- `df.duplicated(subset=['COL1', 'COL2', ...], keep={‘first’, ‘last’, False})` : 중복 데이터 확인
- `df.drop_duplicates(subset=['COL1', 'COL2', ...], keep={'first', 'last', False}, inplace={True, False})` : 중복 데이터 제거

- `df.drop(labels=None, index=None, columns=None, axis={0, 1}, inplace={True, False})` : 행 또는 열을 제거
  - `labels=[ROW1, ROW2, ... ], axis=0` / `index=[ROW1, ROW2, ...]` : 행 제거
  - `labels=[COL1, COL2, ... ], axis=1` / `columns=[COL1, COL2, ...]` : 열 제거

- `df.rename(mapper=None, index=None, columns=None, axis={0, 1}, inplace={True, False})` : 축 이름 변경
  - `mapper={OLD1:NEW1, OLD2:NEW2, ...}, axis=0` : 인덱스(row label) 변경
  - `index={OLD1:NEW1, OLD2:NEW2, ...}` : 인덱스(row label) 변경
  - `mapper={OLD1:NEW1, OLD2:NEW2, ...}, axis=1` : 컬럼명(column label) 변경
  - `columns={OLD1:NEW1, OLD2:NEW2, ...}` : 컬럼명(column label) 변경

- `df.reset_index(drop=False, inplace=False)` : 인덱스 재설정
- `df.set_index(keys, drop=True, append=False, inplace=False)` : 인덱스 설정

- `df.to_pickle()` : pickle 형태로 저장
- `df.to_csv()` : csv 파일로 저장

## 참고자료

[pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)
