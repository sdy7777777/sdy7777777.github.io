---
layout: single
title: "[Python] leetcode(리트코드)45-Jump Game II"
#subtitle: ""
date: 2022-10-04 14:00:00 +0900
last_modified_at: 2022-10-04 14:15:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: Python
tags:
  - Python 
  - leetcode
#  - test
---

# leetcode(리트코드)45-Jump Game II

[leetcode(리트코드)45-Jump Game II](https://leetcode.com/problems/jump-game-ii/)

|-|-|-|-|-|-|
|---|---|---|---|---|---|
|nums|2|3|1|1|4|
|cntlist|0|float('inf')|float('inf')|float('inf')|float('inf')|

최소 jump 수를 구하기 위한 문제이므로 cntlist에는 최대 값인 inf 로 초기화 해 준다.

---

|-|-|-|-|-|-|-|
|---|---|---|---|---|---|---|
| | |cur|nums[cur+1]|nums[cur+2]| | |
|cur = 0|nums[cur]->2|<span style="color:red">0</span>|<span style="background-color:#fff5b1">2</span>|<span style="background-color:#fff5b1">2</span>|inf|inf|
| | | |cur|nums[cur+1]|nums[cur+2]|nums[cur+3]|
|cur = 1|nums[cur]->3|0|<span style="color:red">2</span>|<span style="background-color:#fff5b1">2</span>|<span style="background-color:#fff5b1">2</span>|<span style="background-color:#fff5b1">2</span>|
| | | | |cur|nums[cur+1]| |
|cur = 2|nums[cur]->1|0|2|<span style="color:red">2</span>|<span style="background-color:#fff5b1">2</span>|2|
| | | | | |cur|nums[cur+1]|
|cur = 3|nums[cur]->1|0|2|2|<span style="color:red">2</span>|2|
| | | | | | |cur|
|cur = 4|nums[cur]->4|0|2|2|2|<span style="color:red">2</span>|

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
