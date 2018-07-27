---
layout: post
title: 'Leetcode 254: Factor Combinations'
date: '2018-07-26 21:33'
tags:
  - Leetcode
  - DP
---

# Question
Numbers can be regarded as product of its factors. For example,
```
8 = 2 x 2 x 2;
  = 2 x 4.
```

Write a function that takes an integer n and return all possible combinations of its factors.

Note:

You may assume that n is always positive.
Factors should be greater than 1 and less than n.

Example 1:

```
Input: 1
Output: []
```

Example 2:

```
Input: 37
Output:[]
```

Example 3:

```
Input: 12
Output:
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
```

Example 4:

```
Input: 32
Output:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]
```

# Solution
```python
from math import sqrt

class Solution(object):
    def __init__(self):
        self.factors = dict()

    def getFactors(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        if n == 1:
            return []
        elif n in self.factors:
            return self.factors[n]

        else:
            res = list()
            for f in range(2, int(sqrt(n)) + 1):
                if n % f == 0:
                    residue = n // f
                    res.append([f, residue])
                    for l in self.getFactors(residue):
                        if l[0] >= f:
                            res.append([f] + l)
            return res
```
