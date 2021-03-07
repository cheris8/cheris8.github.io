---
title: '[마크다운] Markdown 문법 정리'

categories:
  - ETC
tags:
  - Markdown
  - Git

last_modified_at: 2020-11-25T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 마크다운 문법에 관하여 정리한 기록입니다.

## 1. Headers

`#`을 사용합니다.

```
# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6
```

# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6

## 2. BlockQuote

`>` 를 사용합니다.

```
> This is a first blockqute.
```

> This is a first blockqute.

```
>> This is a second blockqute.
```

>> This is a second blockqute

```
>>> This is a third blockqute.
```

>>> This is a third blockqute.

```
> This is a first blockqute.
>> This is a second blockqute.
>>> This is a third blockqute.
```

> This is a first blockqute.
>> This is a second blockqute.
>>> This is a third blockqute.

## 3. 목록

### 순서 있는 목록 (번호)

숫자와 점을 사용합니다.

```
1. 첫번째
2. 두번째
3. 세번째
```

1. 첫번째
2. 두번째
3. 세번째

### 순서없는 목록 (기호)

`*`, `+`, `-` 을 사용합니다.

```
- 서울
- 파리
- 뉴욕

+ 서울
+ 파리
+ 뉴욕

* 서울
* 파리
* 뉴욕
```

- 서울
- 파리
- 뉴욕

+ 서울
+ 파리
+ 뉴욕

* 서울
* 파리
* 뉴욕

```
- 서울
  + 파리
    * 뉴욕
```

- 서울
  + 파리
    * 뉴욕

## 4. 강조

### 이탤릭체

```
*single asterisks*
_single underscores_
```

*single asterisks*  
_single underscores_

### 볼드체

```
**double asterisks**
__double underscores__
```

**double asterisks**  
__double underscores__

### 취소선

```
~~cancelline~~
```

~~cancelline~~

## 5. 줄바꿈

- 줄바꿈을 두번 하는 경우
- 문장 마지막에서 2칸 이상 띄어쓰기를 하는 경우
- `<br>` 태그를 사용하는 경우

줄바꿈이 가능합니다.

## 5. 수평선

```
- - -

* * *
```

- - -

* * *

## 6. 코드

` 를 사용합니다.

### 인라인(inline) 코드

` 를 양쪽에 한개씩 사용합니다.

```
`inline code`
```

`inline code`

### 블록(block) 코드

` 를 양쪽에 세개씩 사용합니다.

```
\```
block code
\```
```

```
block code
```

## 7. 링크

```
[Google](https://google.com)
```

[Google](https://google.com)  

```
<https://google.com>
```

<https://google.com>

## 8. 이미지

`![](/path/img.jpg)` 를 사용합니다.

```
![](./images/ETC/MD/decorating-christmas-tree.jpg)
```

![]({{site.url}}/assets/images/ETC/MD/decorating-christmas-tree.jpg)
