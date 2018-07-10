---
layout: "post"
title: "Leetcode 243: Shortest Word Distance"
date: "2018-07-08 13:06"
tags:
  - Leetcode
---

# Question

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

Example:
Assume that `words = ["practice", "makes", "perfect", "coding", "makes"]`.

```
Input: word1 = “coding”, word2 = “practice”
Output: 3
Input: word1 = "makes", word2 = "coding"
Output: 1
```

Note:

You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

# Solution
```python
class Solution(object):
    def shortestDistance(self, words, word1, word2):
        """
        :type words: List[str]
        :type word1: str
        :type word2: str
        :rtype: int
        """
        prev_w1, prev_w2 = -1, -1
        min_distance = len(words)

        for i in range(len(words)):
            if words[i] == word1:
                prev_w1 = i
                if prev_w2 != -1:
                    min_distance = min(min_distance, i - prev_w2)
            elif words[i] == word2:
                prev_w2 = i
                if prev_w1 != -1:
                    min_distance = min(min_distance, i - prev_w1)

        return min_distance
```
