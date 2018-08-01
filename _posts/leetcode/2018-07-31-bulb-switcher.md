---
layout: "post"
title: "Leetcode 319: Bulb Switcher"
date: "2018-07-31 21:02"
tags:
  - Leetcode
---

# Question
There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the i-th round, you toggle every i bulb. For the n-th round, you only toggle the last bulb. Find how many bulbs are on after n rounds.

Example:

```
Input: 3
Output: 1
Explanation:
At first, the three bulbs are [off, off, off].
After first round, the three bulbs are [on, on, on].
After second round, the three bulbs are [on, off, on].
After third round, the three bulbs are [on, off, off].

So you should return 1, because there is only one bulb is on.
```

# Solution
A bulb is on if it is toggled odd times. To be toggled odd times, there must be odd number of integers that can divide its number. Most numbers have even number of dividers, except perfect square numbers. This question is then become "how many perfect square numbers that are less than n".

```python
class Solution(object):
    def bulbSwitch(self, n):
        """
        :type n: int
        :rtype: int
        """
        i = 1
        count = 0
        while i * i <= n:
            count += 1
            i += 1

        return count
```
