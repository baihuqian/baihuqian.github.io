---
layout: "post"
title: "Leetcode 223: Rectangle Area"
date: "2018-07-08 22:05"
tags:
  - Leetcode
---

# Question
Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

![Rectangle Area](https://leetcode.com/static/images/problemset/rectangle_area.png)

Example:
```
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
```

Note:

Assume that the total area is never beyond the maximum possible value of int.

# Solution
```python
class Solution(object):
    def computeArea(self, A, B, C, D, E, F, G, H):
        """
        :type A: int
        :type B: int
        :type C: int
        :type D: int
        :type E: int
        :type F: int
        :type G: int
        :type H: int
        :rtype: int
        """

        if A < E < C < G:
            horizontal = C - E
        elif E < A < G < C:
            horizontal = G - A
        elif E >= A and C >= G:
            horizontal = G - E
        elif A >= E and G >= C:
            horizontal = C - A
        else:
            horizontal = 0

        if B < F < D < H:
            vertical = D - F
        elif F < B < H < D:
            vertical = H - B
        elif F >= B and D >= H:
            vertical = H - F
        elif B >= F and H >= D:
            vertical = D - B
        else:
            vertical = 0

        return (C - A) * (D - B) + (G - E) * (H - F) - horizontal * vertical
```
