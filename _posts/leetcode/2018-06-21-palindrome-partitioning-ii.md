---
layout: post
title: 'Leetcode 132: Palindrome Partitioning II'
date: '2018-06-21 21:42'
tags:
  - Leetcode
  - DP
  - Hard
---

# Question
Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

Example:
```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

# Solution
Use Dynamic Programming to store the minimum cuts needed up to an index. Evaluate strings centered at each index to minimize number of passes needed for palindromes.

```python
class Solution(object):
    def minCut(self, s):
        """
        :type s: str
        :rtype: int
        """
        length = len(s)
        cuts = [i for i in range(length)]

        for i in range(length):
            # Odd size palindrome
            j = 0 # one-character palindrome when j == 0
            while i - j >= 0 and i + j < length and s[i - j] == s[i + j]:
                prev_cuts = cuts[i - j - 1] if i - j - 1 >= 0 else -1
                cuts[i + j] = min(cuts[i + j], prev_cuts + 1)
                j += 1

            j = 0
            while i - j >= 0 and i + j + 1 < length and s[i - j] == s[i + j + 1]:
                prev_cuts = cuts[i - j - 1] if i - j - 1 >= 0 else -1
                cuts[i + j + 1] = min(cuts[i + j + 1], prev_cuts + 1)
                j += 1

        return cuts[-1]
```
