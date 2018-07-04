---
layout: "post"
title: "Leetcode 204: Count Primes"
date: "2018-07-04 20:27"
tags:
  - Leetcode
---

# Question
Count the number of prime numbers less than a non-negative number, n.

Example:

```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

# Solution
Time complexity:

The inner loop runs $$\frac{n}{2} + \frac{n}{3} + \frac{n}{5} + \dots $$ times, so the overall time complexity is $$O(nloglogn)$$.
```python
class Solution(object):
    def countPrimes(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n < 2:
            return 0

        isPrime = [True] * n
        isPrime[0], isPrime[1] = False, False

        count = 0
        for i in range(2, n):
            if isPrime[i]:
                count += 1
                if i * i < n:
                  j = 2
                  while i * j < n:
                      isPrime[i * j] = False
                      j += 1

        return count
```
