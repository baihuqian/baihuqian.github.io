---
layout: post
title: 'Leetcode 93: Restore IP Addresses'
date: '2018-06-16 14:50'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

Example:

```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

# Solution
```python
class Solution:
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        res = []
        if len(s) < 4:
            return res

        self.backtracking(s, len(s), 0, [], res)
        return res

    def backtracking(self, s, length, idx, partial, res):
        if idx == length:
            if len(partial) == 4:
                res.append(".".join(partial))
            return
        else:
            if len(partial) == 4:
                return
            if s[idx] == '1':
                self.backtracking(s, length, idx + 1, partial + [s[idx]], res)
                if idx + 1 < length:
                    self.backtracking(s, length, idx + 2, partial + [s[idx:idx + 2]], res)
                if idx + 2 < length:
                    self.backtracking(s, length, idx + 3, partial + [s[idx:idx + 3]], res)
            elif s[idx] == '2':
                self.backtracking(s, length, idx + 1, partial + [s[idx]], res)
                if idx + 1 < length:
                    self.backtracking(s, length, idx + 2, partial + [s[idx:idx + 2]], res)
                    if idx + 2 < length:
                        if s[idx + 1] < '5' or (s[idx + 1] == '5' and s[idx + 2] <= '5'):
                            self.backtracking(s, length, idx + 3, partial + [s[idx:idx + 3]], res)
            elif '3' <= s[idx] <= '9':
                self.backtracking(s, length, idx + 1, partial + [s[idx]], res)
                if idx + 1 < length:
                    self.backtracking(s, length, idx + 2, partial + [s[idx:idx + 2]], res)
            else:
                self.backtracking(s, length, idx + 1, partial + [s[idx]], res)
```
