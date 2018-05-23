---
layout: "post"
title: "Leetcode 17: Letter Combinations of a Phone Number"
date: "2018-05-21 22:52"
tags:
  - Leetcode
---

# Question
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

```
{
    "2": ["a", "b", "c"],
    "3": ["d", "e", "f"],
    "4": ["g", "h", "i"],
    "5": ["j", "k", "l"],
    "6": ["m", "n", "o"],
    "7": ["p", "q", "r", "s"],
    "8": ["t", "u", "v"],
    "9": ["w", "x", "y", "z"]
}
```

Example:

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
# Solution

```python
class Solution:
    def __init__(self):
        self.lookup_table = {
            "2": ["a", "b", "c"],
            "3": ["d", "e", "f"],
            "4": ["g", "h", "i"],
            "5": ["j", "k", "l"],
            "6": ["m", "n", "o"],
            "7": ["p", "q", "r", "s"],
            "8": ["t", "u", "v"],
            "9": ["w", "x", "y", "z"]
        }

    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if len(digits) == 0:
            return []

        if len(digits) == 1 or digits in self.lookup_table:
            return self.lookup_table[digits]

        left, right = digits[:len(digits) // 2], digits[len(digits) // 2:]
        comb_left = self.letterCombinations(left)
        comb_right = self.letterCombinations(right)
        res = []
        if len(comb_left) == 0:
            res = comb_right
        elif len(comb_right) == 0:
            res = comb_left
        else:
            for l in comb_left:
                for r in comb_right:
                    res.append(l + r)

        self.lookup_table[digits] = res
        return res

```
