---
title: '[Python] 리스트 메소드 (List Method)'

categories:
  - Python
tags:
  - Python
  - Data Preprocessing

last_modified_at: 2021-01-17T08:06:00-05:00

classes: wide
---

이 글은 리스트 메소드에 관한 기록입니다.

## 리스트 메소드 (List Method)

- `list.append(x)` : 리스트 `list`에 요소 `x`를 추가 (반환 X)
    - `list[len(list):] = [x]`와 동일

* * *

- `list.extend(x)` : 리스트 `list`의 끝에 iterable 객체 `x`를 덧붙여 확장 (반환 X)
    - `list[len(list):] = x`와 동일

* * *

- `list.insert(i, x)` : 리스트 `list`의 인덱스 `i`에 즉 `i+1`번째에 요소 `x`를 삽입 (반환 X)
    - `list.insert(0, x)` : 리스트 `list`의 처음에 요소 `x`를 삽입
    - `list.insert(len(a), x)` : 리스트 `list`의 마지막에 요소 `x`를 삽입
        - `list.append(x)`와 동일

* * *

- `list.remove(x)` : 리스트 `list`에서 값이 `x`인 첫번째 요소를 삭제 (반환 X)
    - 리스트 `list`에서 값이 `x`인 요소가 없는 경우 ValueError 발생

* * *

- `list.pop(i)` : 리스트 `list`에서 인덱스 `i`에 위치하는 요소를 삭제하고 그 결과를 반환
    - `list.pop()` : 리스트 `list`의 마지막 요소를 삭제하고 그 결과를 반환

* * *

- `list.clear()` : 리스트 `list`의 모든 요소 삭제 (반환 X)
    - `del list[:]`와 동일

* * *

- `list.index(x)` : 리스트 `list`에서 요소 `x`의 인덱스 반환
    - 리스트 `list`에 요소 `x`가 없는 경우 ValueError 발생

* * *

- `list.count(x)` : 리스트 `list`에서 요소 `x`가 등장한 횟수 반환
  - _참고)_ `len(list)` : 리스트의 길이 반환

* * *

- `list.sort(reverse=True/False)` : 리스트 `list`의 요소들을 정렬 (반환 X)
    - `reverse=False`이면 오름차순 정렬 (default)
    - `reverse=True`이면 내림차순 정렬

* * *

- `list.reverse()` : 리스트 `list`의 요소들을 반대로 뒤집어 정렬 (반환 X)

* * *

- `list.copy()` : 리스트 `list`의 사본 반환 
    - `list[:]`와 동일

* * *

## 참고자료

[Python Documentation](https://docs.python.org/ko/3/tutorial/datastructures.html#more-on-lists)

