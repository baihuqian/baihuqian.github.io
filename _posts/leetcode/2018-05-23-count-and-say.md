---
layout: "post"
title: "Leetcode 38: Count and Say"
date: "2018-05-23 22:14"
tags:
  - Leetcode
---

# Question
The count-and-say sequence is the sequence of integers with the first five terms as following:
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
1 is read off as "one 1" or 11.

11 is read off as "two 1s" or 21.

21 is read off as "one 2, then one 1" or 1211.

Given an integer n, generate the nth term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

Example 1:
```
Input: 1
Output: "1"
```

Example 2:
```
Input: 4
Output: "1211"
```

# Solution
```python
class Solution:
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        if n == 1:
            return "1"
        prev = "1"
        for i in range(1, n):
            prev = self.cAndS(i + 1, prev)

        return prev

    def cAndS(self, n, previous):
        res = []
        digit = previous[0]
        count = 1
        for c in previous[1:]:
            if c == digit:
                count += 1
            else:
                res.append(str(count))
                res.append(digit)
                digit = c
                count = 1
        res.append(str(count))
        res.append(digit)
        return ''.join(res)
```
