---
layout: post
title: 'Leetcode 245: Shortest Word Distance III'
date: '2018-07-08 14:05'
tags:
  - Leetcode
---

# Question
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

Example:
Assume that `words = ["practice", "makes", "perfect", "coding", "makes"]`.

```
Input: word1 = “makes”, word2 = “coding”
Output: 1
Input: word1 = "makes", word2 = "makes"
Output: 3
```

Note:

You may assume word1 and word2 are both in the list.

# Solution
```python
class Solution(object):
    def shortestWordDistance(self, words, word1, word2):
        """
        :type words: List[str]
        :type word1: str
        :type word2: str
        :rtype: int
        """
        min_d = len(words)
        if word1 == word2:
            prev = -1
            for i, w in enumerate(words):
                if w == word1:
                    if prev != -1:
                        min_d = min(min_d, i - prev)
                    prev = i
        else:
            prev1, prev2 = -1, -1
            for i, w in enumerate(words):
                if w == word1:
                    if prev2 != -1:
                        min_d = min(min_d, i - prev2)
                    prev1 = i
                elif w == word2:
                    if prev1 != -1:
                        min_d = min(min_d, i - prev1)
                    prev2 = i

        return min_d
```
