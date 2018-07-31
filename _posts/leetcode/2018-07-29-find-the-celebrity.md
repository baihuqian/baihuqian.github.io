---
layout: "post"
title: "Leetcode 277: Find the Celebrity"
date: "2018-07-29 21:10"
tags:
  - Leetcode
---

# Question
Suppose you are at a party with `n` people (labeled from `0` to `n - 1`) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`, your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.

# Solution
If `knows(a, b) == True`, then `a` is not celebrity; else then `b` is not celebrity. Use this to narrow down to a single candidate, and then verify whether it is the celebrity.

```python
# The knows API is already defined for you.
# @param a, person a
# @param b, person b
# @return a boolean, whether a knows b
# def knows(a, b):

class Solution(object):
    def findCelebrity(self, n):
        """
        :type n: int
        :rtype: int
        """
        candidates = set(range(n))

        while len(candidates) > 1:
            it = iter(candidates)
            a = next(it)
            b = next(it)
            if knows(a, b):
                candidates.remove(a)
            else:
                candidates.remove(b)

        c = next(iter(candidates))
        for i in range(n):
            if c != i:
                if not knows(i, c) or knows(c, i):
                    return -1

        return c
```
