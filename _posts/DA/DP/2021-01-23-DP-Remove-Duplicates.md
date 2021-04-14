---
title: '[데이터 전처리] 데이터프레임 내 중복 데이터 제거하기'

categories:
  - Data Analysis
tags:
  - Data Preprocessing

last_modified_at: 2021-01-23T08:06:00-05:00

classes: wide
---

이 글은 판다스의 데이터프레임 내 중복 데이터를 확인하고 제거하는 방법에 관한 기록입니다.

## 주요 메소드

- `df.duplicated()` : 중복 데이터 확인
- `df.drop_duplicates()` : 중복 데이터 제거

## 중복 데이터 확인하기 : `df.duplicated()`

- `df.duplicated(subset=['COLUMN1', 'COLUMN2', ...], keep={‘first’, ‘last’, False})` : 중복 행을 나타내는 불리언 Series 반환
  - `subset=['COLUMN1', 'COLUMN2', ...]` : `COLUMN1`, `COLUMN2`, ...를 기준으로 중복 행을 확인
  - `keep{‘first’, ‘last’, False}` : 어떤 중복을 표시할 것인지를 결정
    - `keep=first` : 처음에 발생한 중복을 제외한 나머지 중복을 `True`로 표시 (default)
    - `keep=last` : 마지막에 발생한 중복을 제외한 나머지 중복을 `False`로 표시
    - `keep=False` : 모든 중복을 True로 표시

## 중복 데이터 제거하기 : `df.drop_duplicates()`

- `df.drop_duplicates(subset=['COLUMN1', 'COLUMN2', ...], keep={'first', 'last', False}, inplace={True, False})` : 중복 행이 제거된 데이터프레임 반환
  - `subset=['COLUMN1', 'COLUMN2', ...]` : `COLUMN1`, `COLUMN2`, ...을 기준으로 중복 행을 확인
  - `keep{‘first’, ‘last’, False}, default ‘first’` : 어떤 중복을 남길 것인지를 결정
    - `keep=first` : 처음에 발생한 중복을 제외하고 중복 제거 (default)
    - `keep=last` : 마지막에 발생한 중복을 제외하고 중복 제거
    - `keep=False` : 모든 중복을 제거

## 참고자료

[pandas documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.duplicated.html)  
[pandas documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop_duplicates.html)
