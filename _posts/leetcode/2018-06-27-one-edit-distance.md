---
layout: post
title: 'Leetcode 161: One Edit Distance'
date: '2018-06-27 21:26'
tags:
  - Leetcode
---

# Question
Given two strings s and t, determine if they are both one edit distance apart.

Note:

There are 3 possiblities to satisify one edit distance apart:

1. Insert a character into s to get t
1. Delete a character from s to get t
1. Replace a character of s to get t

Example 1:
```
Input: s = "ab", t = "acb"
Output: true
Explanation: We can insert 'c' into s to get t.
```

Example 2:
```
Input: s = "cab", t = "ad"
Output: false
Explanation: We cannot get t from s by only one step.
```

Example 3:
```
Input: s = "1203", t = "1213"
Output: true
Explanation: We can replace '0' with '1' to get t.
```

# Solution
```python
class Solution(object):
    def isOneEditDistance(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) == len(t):
            if s == t:
                return False
            left = 0
            while s[left] == t[left] and left < len(s):
                left += 1
            right = len(s) - 1
            while s[right] == t[right] and right >= 0:
                right -= 1

            return left == right

        elif not -1 <= len(s) - len(t) <= 1:
            return False
        else:
            if len(t) > len(s):
                s, t = t, s
            # s is longer than t by 1
            left = 0
            while left < len(t) and s[left] == t[left]:
                left += 1
            if left == len(t):
                return True

            right = len(t) - 1
            while right >= 0 and s[right + 1] == t[right]:
                right -= 1
            if right == -1:
                return True
            return left == right + 1
```
