---
layout: "post"
title: "Leetcode 402: Remove K Digits"
date: "2018-09-09 13:51"
tags:
  - Leetcode
---

# Question
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

Note:
* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

Example 1:
```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```

Example 2:

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```

Example 3:

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```
# Solution
From left to right, remove digits that are larger than its right one. If less than k of such digits can be found (i.e. the sequence becomes nondecreasing), remove from right until k digits are removed. Remove any leading 0s.

```python
from collections import deque
class Solution:
    def removeKdigits(self, num, k):
        """
        :type num: str
        :type k: int
        :rtype: str
        """
        if k <= 0:
            return num
        if len(num) == k:
            return "0"

        stack = deque()
        stack.append(num[0])

        for c in num[1:]:
            while k > 0 and stack and c < stack[-1]:
                stack.pop()
                k -= 1

            stack.append(c)

        while k > 0:
            stack.pop()
            k -= 1

        while stack and stack[0] == '0':
            stack.popleft()

        if not stack:
            stack.append('0')

        return ''.join(stack)
```
