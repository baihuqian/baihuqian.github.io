---
layout: "post"
title: "Leetcode 14: Longest Common Prefix"
date: "2018-05-20 15:10"
tags:
  - Leetcode
---

# Question
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

Example 1:
```
Input: ["flower","flow","flight"]
Output: "fl"
```

Example 2:
```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

Note:

All given inputs are in lowercase letters `a-z`.

# Solution

```python
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if len(strs) == 0:
            return ""

        length = -1
        while True:
            i = length + 1
            if len(strs[0]) <= i:
                break
            char = strs[0][i]
            for str in strs:
                if len(str) <= i or str[i] != char:
                    return strs[0][:length+1] if length >= 0 else ""
            length += 1

        return strs[0][:length+1] if length >= 0 else ""
```
