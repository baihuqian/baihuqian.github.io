---
layout: "post"
title: "Leetcode 279: Perfect Squares"
date: "2018-07-30 20:28"
tags:
  - Leetcode
  - BFS
  - Review
---

# Question
Given a positive integer n, find the least number of perfect square numbers (for example, `1, 4, 9, 16, ...`) which sum to n.

Example 1:

```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```

Example 2:

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

# BFS Solution
Populate numbers that can be summed with 1, 2, 3, ... perfect square numbers. It also uses an array to store results for 1, 2, ..., n so it's sort of a DP solution too.

```python
from collections import deque
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        perfectSqrs = list()
        i = 0
        while i * i <= n:
            perfectSqrs.append(i * i)
            i += 1

        counts = [0] * (n + 1)
        q = deque()
        count = 1

        for sqr in perfectSqrs:
            counts[sqr] = count
            q.append(sqr)

        if counts[-1] == 1:
            return 1

        while True:
            newQ = deque()
            count += 1
            while len(q) > 0:
                m = q.popleft()
                for sqr in perfectSqrs:
                    s = m + sqr
                    if s < n and counts[s] == 0:
                        counts[s] = count
                        newQ.append(s)
                    elif s == n:
                        return count
                    elif s > n:
                        break
            q = newQ
```
