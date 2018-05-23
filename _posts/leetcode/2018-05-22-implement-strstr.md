---
layout: post
title: 'Leetcode 28: Implement strStr()'
date: '2018-05-22 22:11'
tags:
  - Leetcode
---

# Question
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:
```
Input: haystack = "hello", needle = "ll"
Output: 2
```

Example 2:
```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

# Solution
```python
class Solution:
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        ln = len(needle)
        lh = len(haystack)
        if ln == 0:
            return 0
        if lh < ln:
            return -1

        i = 0

        while i <= lh - ln:
            if haystack[i] == needle[0] and haystack[i:i+ln] == needle:
                return i
            i += 1
        return -1

```
