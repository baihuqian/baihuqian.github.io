---
layout: post
title: 'Leetcode 49: Group Anagrams'
date: '2018-06-06 22:21'
tags:
  - Leetcode
---

# Question
Given an array of strings, group anagrams together.

Example:
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

Note:

* All inputs will be in lowercase.
* The order of your output does not matter.

# Solution
```python
class Solution:
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        res = dict()
        for s in strs:
            sorted_s = ''.join(sorted(s))
            if not sorted_s in res:
                res[sorted_s] = list()

            res[sorted_s].append(s)

        return [v for k, v in res.items()]
```
