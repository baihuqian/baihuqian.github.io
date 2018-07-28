---
layout: post
title: 'Leetcode 30: Substring with Concatenation of All Words'
date: '2018-05-24 22:32'
tags:
  - Leetcode
  - SlidingWindow
  - Hard
  - Review
---

# Question
You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

Example 1:
```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

# Solution
Use a sliding window. At each position, find the occurrence for each word in `words` and compare it to that in `words`. If matches, then it is a valid solution.

```python
from collections import Counter
class Solution:
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        cnt = len(words)
        if cnt == 0:
            return []
        size = len(words[0])

        if size == 0 or len(s) < cnt * size:
            return []

        hist = dict(Counter(words))
        res = []

        for idx in range(0, len(s) - cnt * size + 1):
            curr_hist = dict()
            for j in range(0, cnt):
                w = s[idx + j*size: idx + (j+1)*size]
                if w in hist:
                    if w in curr_hist:
                        curr_hist[w] += 1
                    else:
                        curr_hist[w] = 1
                else:
                    break
            if curr_hist == hist:
                res.append(idx)

        return res

```
