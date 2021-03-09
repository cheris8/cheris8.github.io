---
title: '[Python] 문자열/데이터프레임에서 공백 제거하기'

categories:
  - Data Analysis
tags:
  - Python
  - Pandas
  - Data Preprocessing
  - Text Preprocessing
  - Data Analysis

last_modified_at: 2021-01-10T08:06:00-05:00

classes: wide
---

이 글은 문자열 및 데이터프레임에서 공백을 제거하는 방법에 관한 기록입니다.

## 문자열에서 공백 제거하기

문자열에서 공백을 제거할 때에는 `strip()` 메소드와 `replace()` 메소드를 사용할 수 있습니다.

### `strip()` 메소드

`strip()` 메소드는 문자열에서 공백을 제거하는 기능을 합니다.

- `string.lstrip()` : 문자열 `string`의 왼쪽에 있는 공백 제거
- `string.rstrip()` : 문자열 `string`의 오른쪽에 있는 공백 제거
- `string.strip()` : 문자열 `string`의 양쪽에 있는 공백 제거

`strip()` 메소드에 어떠한 문자열을 인자로 넣으면, 그 문자열 내 문자들의 모든 조합을 제거할 수 있습니다.

- `string.lstrip('chars')` : 문자열 `string`의 왼쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거
- `string.rstrip('chars')` : 문자열 `string`의 으론쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거
- `string.strip('chars')` : 문자열 `string`의 양쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거

### `replace()` 메소드

`replace()` 메소드는 문자열에서 어떠한 문자열을 다른 문자열로 대체하는 기능을 합니다.

- `string.replace('old', 'new', n)` : 문자열 `string`에서 `old`를 찾아 `new`로 `n`번 대체

### `sub()` 함수

`re` 패키지의 `sub` 함수는 문자열에서 특정 패턴을 다른 패턴으로 대체하는 기능을 합니다.

- `re.sub(pattern, repl, string, count=0, flags=0)` : 문자열 `string`에서 `pattern`을 찾아 `repl`로 대체

## 데이터프레임에서 공백 제거하기

- `df['ColumnName'].str.strip()` : 데이터프레임 `df`의 특정 열 `ColumnName`에 대하여 공백 제거
- `df.apply` : 데이터프레임 `df` 전체에 대하여 공백 제거
 
## 참고자료

[Python Documentation](https://docs.python.org/3/library/stdtypes.html)

