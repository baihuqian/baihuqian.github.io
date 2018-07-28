---
layout: post
title: 'Leetcode 91: Decode Ways'
date: '2018-06-16 14:12'
tags:
  - Leetcode
  - DP
  - Hard
  - Review
---

# Question
A message containing letters from A-Z is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

Example 2:

```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

# Solution
Use DP from the end to front.
```python
class Solution:
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s[0] == '0':
            return 0

        p, pp, curr = 1, 0, 0
        for i in range(len(s) - 1, -1, -1):
            curr = 0 if s[i] == '0' else p

            if i < len(s) - 1 and (s[i] == '1' or (s[i] == '2' and s[i + 1] < '7')):
                curr += pp
            pp = p
            p = curr

        return curr
```
