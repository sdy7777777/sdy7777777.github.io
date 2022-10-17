---
layout: single
title: "[Python] leetcode91-Decode Ways"
#subtitle: ""
date: 2022-10-16 17:00:00 +0900
last_modified_at: 2022-10-16 17:00:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: python 
tags:
  - python 
  - leetcode
#  - test
---

## leetcode91-Decode Ways

[leetcode91-Decode Ways](https://leetcode.com/problems/decode-ways/)

---  

## Approch 1 - DFS(Timeout)

![image](https://assets.leetcode.com/users/images/79bad860-3bf9-43ed-b950-cc0b02bb2754_1665915141.5535564.png)

**Basic Structure**
```pythpn
string s.
DFS(s)
  if s == '': 
      cnt += 1
	  return
  
  pick 1 num -> DFS(s[1:])
  pict 2 num -> DFS(s[2:])
```

## Approch 1 - 풀이 (Timeout)
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        def checker(sstr, sel): # (sstr: check string, sel: selected number)
            global cnt
            if sstr == '':                                
                #selects.append(sel[:]) # for checking 
                cnt += 1
                return
            
            if int(sstr[0]) == 0: return

            # Pick 1 num
            sel.append(sstr[0]) 
            checker(sstr[1:], sel)
            sel.pop()

            # Pick 2 nums
            if not 10 <= int(sstr[:2]) <= 26: return
            if len(sstr) >= 2:
                sel.append(sstr[:2])
                if len(sstr) == 2:
                    checker('', sel)
                else:
                    checker(sstr[2:], sel)
                sel.pop()

        #selects = [] # for checking
        global cnt
        cnt = 0 # get counts of case
        checker(s, []) # (val1: check string, val2: selected number)
        #print('ret:', selects) # for checking

        return cnt
```

=> `"111111111111111111111111111111111111111111111"` TC  Timeout...  OTL

---
---


## Approach 2 - DP(Success)
https://www.dalecoding.com/problems/decode-ways

**Basic Structure**
![image](https://assets.leetcode.com/users/images/17dbe29e-4b16-4c8e-9ef9-62e62b91ead5_1665919470.278059.png)
![image](https://assets.leetcode.com/users/images/c6050130-469b-4a35-88f3-e5459fb4fe7b_1665919489.426692.png)


## Approach 2 - 풀이(Success)

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        dp = [0] * len(s) + [1]
        for start in reversed(range(len(s))):
            if s[start] == "0":
                dp[start] = 0
            elif start + 1 < len(s) and int(s[start : start + 2]) < 27:
                dp[start] = dp[start + 1] + dp[start + 2]
            else:
                dp[start] = dp[start + 1]
        return dp[0]
```
