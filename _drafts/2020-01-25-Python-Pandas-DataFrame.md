---
title: '[Python] 판다스 (Pandas) - 데이터프레임 (DataFrame)'

categories:
  - Python
tags:
  - Python
  - Pandas

last_modified_at: 2020-03-20T08:06:00-05:00

classes: wide
---

- `pd.set_option('display.max_rows, None')` : 데이터프레임의 모든 행 출력
- `pd.set_option('display.max_columns, None')` : 데이터프레임의 모든 열 출력

### 

- `df.index` : 데이터프레임의 인덱스(row label) 반환
- `df.columns` : 데이터프레임의 컬럼명(column label) 반환

- `df.dtypes` : 데이터프레임의 데이터 타입 반환
- `df.info()` : 데이터프레임에 대한 간결한 요약 출력 : index, dtype, columns, non-null values, memory usage
- `df.ndim` : 차원을 나타내는 int 반환 (Series일 때 1 반환 / DataFrame일 때 2 반환)
- `df.size` : 

- 객체에 있는 요소들의 개수를 나타내는 int 반환
  - Series일 째 행의 개수 반환
  - 데이터프레임일 때 행의 개수 x 열의 개수 반환 


- `df.shape` : 데이터프레임의 차원을 나타내는 튜플 반환

- `df.astype(dtype, copy=True, errors='raise')`

DataFrame.astype(dtype, copy=True, errors='raise')
Cast a pandas object to a specified dtype dtype.

Parameters
dtypedata type, or dict of column name -> data type
Use a numpy.dtype or Python type to cast entire pandas object to the same type. Alternatively, use {col: dtype, …}, where col is a column label and dtype is a numpy.dtype or Python type to cast one or more of the DataFrame’s columns to column-specific types.
copybool, default True
Return a copy when copy=True (be very careful setting copy=False as changes to values then may propagate to other pandas objects).
errors{‘raise’, ‘ignore’}, default ‘raise’
Control raising of exceptions on invalid data for provided dtype.
raise : allow exceptions to be raised
ignore : suppress exceptions. On error return original object.
Returns
castedsame type as caller

- `df.copy()` : `df`를 복사

- `df.head(n)` : 위에서부터 n개의 행을 반환
- `df.tail(n)` : 아래에서부터 n개의 행을 반환




- `df.apply()`
- `df.agg()`
- `df.aggregate()`
- `df.grooupby()`


---

## 통계

- `df.describe()`
- `df.max()`
- `df.mean()`
- `df.median()`
- `df.min()`


### df.nunique()

### 

### df.value_counts



---
- df.drop

DataFrame.drop(labels=None, axis=0, index=None, columns=None, level=None, inplace=False, errors='raise')[source]
Drop specified labels from rows or columns.

Remove rows or columns by specifying label names and corresponding axis, or by specifying directly index or column names. When using a multi-index, labels on different levels can be removed by specifying the level.


### df.reaname

- `df.reset_index(level=None, drop=False, inplace=False, col_level=0, col_fill='')`
Reset the index, or a level of it.

Reset the index of the DataFrame, and use the default one instead. If the DataFrame has a MultiIndex, this method can remove one or more levels.


- `df.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False)` : 

Set the DataFrame index using existing columns.

Set the DataFrame index (row labels) using one or more existing columns or arrays (of the correct length). The index can replace the existing index or expand on it.
## 결측치 관련

- `df.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)` : 결측치 제거
- `df.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None)` : 결측치 대체

- `df.isna`
- `df.isnull`
- `df.notna`
- `df.notnll`
- `df.pad`
- `df.replace`


### df.pivot

### df.pivot_table

### df.sort_values

### df.squeeze

### df.transpose




## 저장


### df.to_pickle

### df.to_csv