---
title: '[Python] 문자열 비교하기'

categories:
  - Python
tags:
  - Python
  - Data Preprocessing
  - Text Preprocessing
  - Data Analysis

last_modified_at: 2021-01-15T08:06:00-05:00

classes: wide
---

이 글은 문자열 완전 일치 / 부분 일치 / 전방 일치 / 후방 일치에 관한 기록입니다.

## 문자열 비교

두 문자열을 비교하는 데에는 다음과 같은 4가지 종류가 있습니다.

- 완전 일치 : `==` `!=`
- 부분 일치 : `in` `not in`
- 전방 일치 : `startswith`
- 후방 일치 : `endswith`

## 완전 일치

문자열이 완전하게 일치하는지 비교하는 데에는 `==`과 `!=`이 사용됩니다. `==`는 두 문자열이 완전히 일치하면 `True`, 일치하지 않으면 `False`를 반환합니다.`!=`는 두 문자열이 일치하지 않으면 `True`, 완전히 일치하면 `False`를 반환합니다.

```python
'ABC'=='ABC'
```

    True

```python
'ABC'=='abc'
```

    False

```python
'ABC'!='ABC'
```

    False

```python
'ABC'!='abc'
```

    True

## 부분 일치

문자열이 부분적으로 일치하는지 비교하는 데에는 `in`과 `not in`이 사용됩니다. `in`은 어떤 문자열이 다른 문자열에 부분적으로 존재하면 `True`, 존재하지 않으면 `False`를 반환합니다. `not in`은 어떤 문자열이 다른 문자열에 존재하지 않으면 `True`, 부분적으로 존재하면 `False`를 반환합니다.

```python
'Feat.' in '에잇(Prod.&Feat. SUGA of BTS)'
```

    True

```python
'Feat.' not in '에잇(Prod.&Feat. SUGA of BTS)'
```

    False

## 전방 일치

문자열이 전방에서 일치하는지 비교하는 데에는 `startswith`가 사용됩니다. `startswith`는 문자열이 해당 패턴으로 시작하면 `True`, 시작하지 않으면 `False`를 반환합니다. 패턴을 여러 개 지정할 경우 문자열이 이들과 하나라도 전방 일치 한다면 `True`를 반환합니다.

```python
phonenumber = '010-1234-5678'
phonenumber.startswith('010')
```

    True

```python
phonenumber = '010-1234-5678'
phonenumber.startswith('011')
```

    False

```python
phonenumber = '010-1234-5678'
phonenumber.startswith(('010', '011'))
```

    True

## 후방 일치

문자열이 전방에서 일치하는지 비교하는 데에는 `endswith`가 사용됩니다. `endswith`는 문자열이 해당 패턴으로 끝나면 `True`, 끝나지 않으면 `False`를 반환합니다. 패턴을 여러 개 지정할 경우 문자열이 이들과 하나라도 후방 일치 한다면 `True`를 반환합니다.

```python
name = '김대한 씨'
name.endswith('씨')
```

    True

```python
name = '김대한 씨'
name.endswith('님')
```

    False

```python
name = '김대한 씨'
name.endswith(('씨', '님'))
```

    True


