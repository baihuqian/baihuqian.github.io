---
layout: post
title: 'Leetcode 58: Length of Last Word'
date: '2018-05-31 23:32'
tags:
  - Leetcode
published: true
---

# Question

Given a string s consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

Example:
```
Input: "Hello World"
Output: 5
```

# Solution

```python
class Solution:
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) == 0:
            return 0

        idx = len(s) - 1
        while idx >= 0:
            if s[idx] == ' ':
                idx -= 1
            else:
                break

        r = idx
        while idx >= 0:
            if s[idx] != ' ':
                idx -= 1
            else:
                break

        l = idx + 1
        return r - l + 1
```
