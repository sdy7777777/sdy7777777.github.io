---
layout: single
title: "[Python] leetcode(리트코드)45-Jump Game II"
#subtitle: ""
date: 2022-10-04 14:00:00 +0900
last_modified_at: 2022-10-04 14:15:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: python 
tags:
  - python 
  - leetcode
#  - test
---

# leetcode(리트코드)45-Jump Game II

[leetcode(리트코드)45-Jump Game II](https://leetcode.com/problems/jump-game-ii/)

|-|idx 0|idx 1|idx 2|idx 3|idx 4|
|---|---|---|---|---|---|
|nums|2|3|1|1|4|
|cntlist|0|float('inf')|float('inf')|float('inf')|float('inf')|

> **cntlist**  
> - 최소 jump 카운트를 저장하는 리스트
> - 최소 jump 수를 구하기 위한 문제이므로 cntlist에는 최대 값인 inf (양의 무한대) 로 초기화 해 준다.  


---
  
아래 Table은 cur index의 변화에 따른 cntlist(최소 jump 카운트)의 값 변화를 보여 준다.

|-|-|idx 0|idx 1|idx 2|idx 3|idx 4|
|---|---|---|---|---|---|---|
|---|nums|2|3|1|1|4|
|---|cntlist|0|float('inf')|float('inf')|float('inf')|float('inf')|
| **cntlist의 값 변화** |---|---|---|---|---|---|
| | |cur|cntlist[cur+1]|cntlist[cur+2]| | |
|cur = 0|nums[cur]->2|<span style="color:red">0</span>|<span style="background-color:#fff5b1">1</span>|<span style="background-color:#fff5b1">1</span>|inf|inf|
| | | |cur|cntlist[cur+1]|cntlist[cur+2]|cntlist[cur+3]|
|cur = 1|nums[cur]->3|0|<span style="color:red">1</span>|<span style="background-color:#fff5b1">1</span>|<span style="background-color:#fff5b1">2</span>|<span style="background-color:#fff5b1">2</span>|
| | | | |cur|cntlist[cur+1]| |
|cur = 2|nums[cur]->1|0|1|<span style="color:red">1</span>|<span style="background-color:#fff5b1">2</span>|2|
| | | | | |cur|cntlist[cur+1]|
|cur = 3|nums[cur]->1|0|1|1|<span style="color:red">2</span>|<span style="background-color:#fff5b1">2</span>|
| | | | | | |cur|
|cur = 4|nums[cur]->4|0|1|1|2|<span style="color:red">2</span>|

최종 결과 Return
```python
return cntlist[-1]
```

nums[cur+n] 컬럼 로직
```python
if cntlist[cur] +1 < cntlist[cur+1]:	
    cntlist[cur+1] = cntlist[cur] +1	
```


## 풀이
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        
        cntl = [float('inf')] *len(nums)
        cntl[0] = 0
        
        for cur in range(len(nums)):
            for j in range(1, nums[cur]+1):
                #print(cur,j)
                if cur + j >= len(nums): continue
                if cntl[cur] + 1 < cntl[cur+j]:
                    cntl[cur+j] = cntl[cur] + 1
        
        return cntl[-1]
    
```
