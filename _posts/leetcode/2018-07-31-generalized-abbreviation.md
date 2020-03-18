---
layout: post
title: 'Leetcode 320: Generalized Abbreviation'
date: '2018-07-31 22:12'
tags:
  - Leetcode
  - DP
  - Backtracking
published: true
---

# Question
Write a function to generate the generalized abbreviations of a word.

Note: The order of the output does not matter.

Example:

```
Input: "word"
Output:
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```

# DP Solution
```python
class Solution(object):
    def generateAbbreviations(self, word):
        """
        :type word: str
        :rtype: List[str]
        """
        if len(word) == 0:
            return [""]

        dp = [[]] * (len(word) - 1)
        dp.append(["1", word[-1]])

        for idx in range(len(word) - 2, -1, -1):
            suffixes = set()
            for l in range(1, len(word) - idx):
                for suffix in dp[idx + l]:
                    suffixes.add(word[idx:idx+l] + suffix)
                    if suffix[0].isalpha():
                        suffixes.add(str(l) + suffix)

            suffixes.add(str(len(word) - idx))
            dp[idx] = list(suffixes)

        return dp[0]

```

# Backtracking Solution
```python
class Solution(object):
    def generateAbbreviations(self, word):
        """
        :type word: str
        :rtype: List[str]
        """
        if len(word) == 0:
            return [""]
        res = []

        self.backtracking(word, res, "", 0, 0)
        return res

    def backtracking(self, word, res, partial, count, idx):
        if idx == len(word):
            if count == 0:
                res.append(partial)
            else:
                res.append(partial + str(count))
        else:
            if count != 0:
                self.backtracking(word, res, partial + str(count) + word[idx], 0, idx + 1)
            else:
                self.backtracking(word, res, partial + word[idx], 0, idx + 1)
            self.backtracking(word, res, partial, count + 1, idx + 1)
```
