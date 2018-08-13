---
layout: "post"
title: "Leetcode 365: Water and Jug Problem"
date: "2018-08-13 21:17"
tags:
  - Leetcode
  - Review
---

# Question
You are given two jugs with capacities x and y litres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactly z litres using these two jugs.

If z liters of water is measurable, you must have z liters of water contained within one or both buckets by the end.

Operations allowed:

* Fill any of the jugs completely with water.
* Empty any of the jugs.
* Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.

Example 1: (From the famous "Die Hard" example)
```
Input: x = 3, y = 5, z = 4
Output: True
```

Example 2:

```
Input: x = 2, y = 6, z = 5
Output: False
```

# Solution
This is a pure Math problem. The basic idea is to use the property of Bézout's identity and check if z is a multiple of GCD(x, y) (greatest common divider).

> Bézout's identity — Let a and b be integers with greatest common divisor d. Then, there exist integers x and y such that ax + by = d. More generally, the integers of the form ax + by are exactly the multiples of d.

Assume $$f(a, b) = ax + by$$ to be total amount of water in both jugs, then the three operations become:
1. Increment a or b by 1 (fill jug with water);
2. Decrement a or b by 1 (empty any of the jugs);
3. Pour water from one jug to the other does not change the value of f.

Thus, if z is a multiple of GCD(a, b), then there exists a, b such that `ax + by == z`.

```python
class Solution:
    def canMeasureWater(self, x, y, z):
        """
        :type x: int
        :type y: int
        :type z: int
        :rtype: bool
        """
        if z == 0:
            return True
        if x + y < z:
            return False

        return z % self.gcd(x, y) == 0

    def gcd(self, a, b):
        if b > a:
            a, b = b, a
        while b != 0:
            tmp = b
            b = a % b
            a = tmp
        return a
```
