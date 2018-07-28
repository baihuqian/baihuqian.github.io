---
layout: "post"
title: "Leetcode 267: Palindrome Permutation II"
date: "2018-07-28 17:30"
tags:
 - Leetcode
---

# Question
Given a string `s`, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

Example 1:

```
Input: "aabb"
Output: ["abba", "baab"]
```

Example 2:

```
Input: "abc"
Output: []
```

# Solution
```python
class Solution(object):
    def generatePalindromes(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        if len(s) <= 1:
            return [s]

        charCount = dict()
        for c in s:
            if c in charCount:
                charCount[c] += 1
            else:
                charCount[c] = 1

        odd = len(s) % 2 == 1
        mid = ""

        for c, co in charCount.items():
            if co % 2 == 1:
                if odd:
                    if co > 1:
                        charCount[c] = co // 2
                    else:
                        del charCount[c]
                    mid = c
                    odd = False
                else:
                    return []
            else:
                charCount[c] = co // 2

        permutations = list()
        self.__permutations(permutations, charCount, [])
        return [p + mid + p[::-1] for p in permutations]

    def __permutations(self, res, count, partial):
        if len(count) == 0:
            res.extend(partial)
            return
        for c in count:
            new_partial = list() if len(partial) > 0 else c
            if count[c] == 1:
                del count[c]
            else:
                count[c] -= 1
            for p in partial:
                new_partial.append(p + c)
            self.__permutations(res, count, new_partial)
            if c not in count:
                count[c] = 1
            else:
                count[c] += 1

```
