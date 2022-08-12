---
layout: single
title: "[Python] Binary Search / bisect_left, bisect_right"
#subtitle: ""
date: 2022-08-12 19:48:00 +0900
lastmod: 2022-08-12 19:48:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: python
#tags:
#  - test
---

Python에서는 Binary Serch시 bisect 모듈을 사용 할 수 있다.
Binary Search 필수 조건인 정렬 된 리스트가 존재 할 경우 사용 가능하다.

직접 binary search 를 구현하는 방법도 있지만, python 모듈을 통해 제공되는 특성상 좀 더 빠른 속도를 제공한다.

한데... 매번 헷갈린다.;;;;;;

## Binary Search 기본 포멧
```python
def Binary_Search(s, e, d):
    while s<=e:
        m = (s+e)//2
        if num[m] == d: return m
        elif num[m] > d: e = m - 1
        else: s = m + 1
    return -1
```

## bisect 모듈 사용 포멧
```python
import bisect
...
def Solve_Bisect():
    sol = []
    for q in query:
        ret = bisect.bisect_left(num,q,lo=1)
        if ret != len(num) and num[ret] == q: sol.append(ret)
        else: sol.append(0)
    return sol

```

보통 bisect_left / bisect_right를 많이 사용한다.
- bisect_left(list_a, x, lo=0, hi=len(list_a))
  - x를 삽입 가능한 가장 왼쪽 index를 반환한다.
  - 리스트에 찾는 값이 존재할때, 반환된 index는 해당 값이 들어 있다.
  
- bisect_right(list_a, x, lo=0, hi=len(list_a))
  - x를 삽입 가능한 가장 오른쪽 index를 반환한다.
  - 리스트에 찾는 값이 존재할때, 반환된 index는 해당 값이 들어 있지 않다.  (찾는 값의 오른쪽`index+1` index를 반환한다.)

*a_list = [1,3,3,3,5]*
*len(a_list) -> 5*

|index|0|1|2|3|4|5|
|-----|-|-|-|-|-|-|
|value | 1 | 3| 3| 3|5| N/A - out of scope |


|bisect_left| 결과 | bisect_right | 결과|
|-----------|------|--------------|----|
|bisect_left(a, 0) => 0| 리스트에 0 없음. <br /> 삽입 가능한 위치 index 0 반환 | bisect_right(a, 0) => 0	| 리스트에 0 없음. <br /> 삽입 가능한 위치 index 0 반환 |
|bisect_left(a, 1) => 0| 리스트에 1 있음. <br /> 삽입 가능한 위치 index 0 반환 | bisect_right(a, 1) => 1	| 리스트에 1 있음. <br /> 삽입 가능한 위치 index 1 반환 |
|bisect_left(a, 2) => 1| 리스트에 2 없음. <br /> 삽입 가능한 위치 index 1 반환 | bisect_right(a, 2) => 1	| 리스트에 2 없음. <br /> 삽입 가능한 위치 index 1 반환 |
|bisect_left(a, 3) => 1| 리스트에 3 있음. <br /> 삽입 가능한 위치 index 1 반환 | bisect_right(a, 3) => 4	| 리스트에 3 있음. <br /> 삽입 가능한 위치 index 4 반환 |
|bisect_left(a, 4) => 4| 리스트에 4 없음. <br /> 삽입 가능한 위치 index 4 반환 | bisect_right(a, 4) => 4	| 리스트에 4 없음. <br /> 삽입 가능한 위치 index 4 반환 |
|bisect_left(a, 5) => 4| 리스트에 5 있음. <br /> 삽입 가능한 위치 index 4 반환 | bisect_right(a, 5) => 5	| 리스트에 5 있음. <br /> 삽입 가능한 위치 index 5 반환 (N/A)|
|bisect_left(a, 6) => 5| 리스트에 6 없음. <br /> 삽입 가능한 위치 index 5 반환 (N/A) | bisect_right(a, 6) => 5	| 리스트에 6 없음. <br /> 삽입 가능한 위치 index 5 반환 (N/A)|
