---
layout: "post"
title: "Leetcode 187: Repeated DNA Sequences"
date: "2018-07-03 21:21"
tags:
  - Leetcode
---

# Question
All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

Example:

```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```

# Solution
Use a dict to count the number of occurrences of 10-letter sequences.

```python
class Solution(object):
    def findRepeatedDnaSequences(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        if len(s) < 10:
            return []

        d = dict()

        for i in range(len(s) - 9):
            substr = s[i:i+10]
            if substr in d:
                d[substr] += 1
            else:
                d[substr] = 1

        return [substr for substr in d if d[substr] > 1]
```
