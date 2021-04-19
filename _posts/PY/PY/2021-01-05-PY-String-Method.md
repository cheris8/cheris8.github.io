---
title: '[Python] 문자열 메소드 (String Method)'

categories:
  - Python
tags:
  - Python

last_modified_at: 2021-01-05T08:06:00-05:00

classes: wide
---

이 글은 문자열 메소드에 관한 기록입니다.

## 문자열 내 문자들의 개수 구하기

- `string.count()` : 문자열 `string` 내 unique한 문자들의 개수 반환
  - _참고)_ `len(string)` : 문자열의 길이 반환

## 문자열 내 특정 문자열 찾기

- `string.startswith('prefix')`
  - 문자열 `string`이 `prefix`로 시작하면 `True` 반환
  - 문자열 `string`이 `prefix`로 시작하지 않으면 `False` 반환
- `string.endswith('suffix')`
  - 문자열 `string`이 `suffix`로 끝나면 `True` 반환
  - 문자열 `string`이 `suffix`로 끝나지 않으면 `False` 반환
- `string.find('substring')`
  - 문자열 `string`에서 `substring`이 가장 먼저 발견된 위치의 인덱스 반환
  - 문자열 `string`에서 `substring`이 발견되지 않으면 `-1` 반환
- `string.rfind('substring')`
  - 문자열 `string`의 오른쪽에서부터 시작하여 `substring`이 가장 먼저 발견된 위치의 인덱스 반환
  - 문자열 `string`에서 `substring`이 발견되지 않으면 `-1` 반환
- `string.index('substring')`
  - 문자열 `string`에서 `substring`이 가장 먼저 발견된 위치의 인덱스 반환
  - 문자열 `string`에서 `substring`이 발견되지 않으면 `ValueError` 발생
- `string.rindex('substring')`
  - 문자열 `string`의 오른쪽에서부터 시작하여 `substring`이 가장 먼저 발견된 위치의 인덱스 반환
  - 문자열 `string`에서 `substring`이 발견되지 않으면 `ValueError` 발생

## 문자열에서 공백 제거하기

- `string.lstrip()` : 문자열 `string`의 왼쪽 공백 제거
- `string.rstrip()` : 문자열 `string`의 오른쪽 공백 제거
- `string.strip()` : 문자열 `string`의 양쪽 공백 제거

## 문자열에서 특정 문자열 제거하기

- `string.lstrip('chars')` : 문자열 `string`의 왼쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거
- `string.rstrip('chars')` : 문자열 `string`의 으론쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거
- `string.strip('chars')` : 문자열 `string`의 양쪽에서 `chars`의 문자들로 만들 수 있는 모든 조합을 제거
- `string.removeprefix('prefix')`
  - 만약 문자열 `string`이 `prefix`로 시작하면, 문자열 `string`에서 `prefirx`를 제거한 문자열을 반환
  - 만약 문자열 `string`이 `prefix`로 시작하지 않으면, 문자열 `string`을 반환
- `string.removesuffix('suffix')`
  - 만약 문자열 `string`이 `suffix`로 끝나면, 문자열 `string`에서 `suffix`를 제거한 문자열을 반환
  - 만약 문자열 `string`이 `suffix`로 끝나지 않으면, 문자열 `string`을 반환

## 문자열 대체하기

- `string.capitalize()` : 문자열 `string` 내 각 단어의 첫번째 문자를 대문자로 나머지 문자들을 소문자로 바꾼 문자열을 반환
- `string.lower()` : 문자열 `string` 내 모든 문자들을 소문자로 바꾼 문자열을 반환
- `string.upper()` : 문자열 `string` 내 모든 문자들을 대문자로 바꾼 문자열을 반환
- `string.replace('old', 'new', count)` : 문자열 `string`에서 `old`를 찾아 `new`로 `count`번 대체
- `string.translate(table)`

## 문자열 연결하기

- `string.join(iterable)`

## 문자열 분리하기

- `string.split(sep=None, maxsplit=-1)`

## 참고자료

[Python Documentation](https://docs.python.org/3/library/stdtypes.html)

