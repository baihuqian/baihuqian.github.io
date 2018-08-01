---
layout: "post"
title: "Leetcode 672: Bulb Switcher II"
date: "2018-07-31 21:27"
tags:
  - Leetcode
---

# Question
There is a room with n lights which are turned on initially and 4 buttons on the wall. After performing exactly m unknown operations towards buttons, you need to return how many different kinds of status of the n lights could be.

Suppose n lights are labeled as number [1, 2, 3 ..., n], function of these 4 buttons are given below:

1. Flip all the lights.
1. Flip lights with even numbers.
1. Flip lights with odd numbers.
1. Flip lights with (3k + 1) numbers, k = 0, 1, 2, ...

Example 1:

```
Input: n = 1, m = 1.
Output: 2
Explanation: Status can be: [on], [off]
```

Example 2:
```
Input: n = 2, m = 1.
Output: 3
Explanation: Status can be: [on, off], [off, on], [off, off]
```

Example 3:
```
Input: n = 3, m = 1.
Output: 4
Explanation: Status can be: [off, on, off], [on, off, on], [off, off, off], [off, on, on].
```

Note: n and m both fit in range [0, 1000].

# Solution
As before, the first 6 lights uniquely determine the rest of the lights. This is because every operation that modifies the x-th light also modifies the $$(x+6)$$-th light, so the x-th light is always equal to the $$(x+6)$$-th light.

Actually, the first 3 lights uniquely determine the rest of the sequence, as shown by the table below for performing the operations a, b, c, d:

* Light 1 = 1 + a + c + d
* Light 2 = 1 + a + b
* Light 3 = 1 + a + c
* Light 4 = 1 + a + b + d
* Light 5 = 1 + a + c
* Light 6 = 1 + a + b

So that (modulo 2):

* Light 4 = (Light 1) + (Light 2) + (Light 3)
* Light 5 = Light 3
* Light 6 = Light 2

The above justifies taking $$n = min(n, 3)$$ without loss of generality. The rest is now casework.

Let's denote the state of lights by the tuple $$(a, b, c)$$. The transitions are to XOR by (1, 1, 1), (0, 1, 0), (1, 0, 1), or (1, 0, 0).

When m = 0, all the lights are on, and there is only one state (1, 1, 1). The answer in this case is always 1.

When m = 1, we could get states (0, 0, 0), (1, 0, 1), (0, 1, 0)(, or (0, 1, 1). The answer in this case is either 2, 3, 4 for n = 1, 2, 3 respectively.

When m = 2, we can manually check that we can get 7 states: all of them except for (0, 1, 1). The answer in this case is either 2, 4, 7 for n = 1, 2, 3 respectively.

When m = 3, we can get all 8 states. The answer in this case is either 2, 4, 8 for n = 1, 2, 3 respectively.

```python
class Solution(object):
    def flipLights(self, n, m):
        """
        :type n: int
        :type m: int
        :rtype: int
        """
        n = min(n, 3)
        if m == 0: return 1
        if m == 1: return [2, 3, 4][n-1]
        if m == 2: return [2, 4, 7][n-1]
        return [2, 4, 8][n-1]
```
