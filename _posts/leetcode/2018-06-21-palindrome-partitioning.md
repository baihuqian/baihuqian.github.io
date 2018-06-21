---
layout: "post"
title: "Leetcode 131: Palindrome Partitioning"
date: "2018-06-21 21:20"
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example:

```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

# Solution
```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        res = []
        self.backtracking(s, len(s), 0, res, [])
        return res

    def backtracking(self, s, length, index, res, partial):
        if index == length:
            res.append(partial)
        else:
            for new_index in range(index + 1, length + 1):
                if self.isPalin(s[index:new_index]):
                    self.backtracking(s, length, new_index, res, partial + [s[index:new_index]])

    def isPalin(self, s):
        return s == s[::-1]

# Indexed Palindrome testing is slower
#     def isPalin(self, s):
#         if len(s) <= 1:
#             return True
#         start, end = 0, len(s) - 1
#         while start < end:
#             if s[start] != s[end]:
#                 return False
#             start += 1
#             end -= 1

#         return True
```
