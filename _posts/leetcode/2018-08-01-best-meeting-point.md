---
layout: "post"
title: "Leetcode 296: Best Meeting Point"
date: "2018-08-01 20:33"
tags:
  - Leetcode
  - Hard
---

# Question
A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance `(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

Example:

```
Input:

1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

Output: 6

Explanation: Given three people living at (0,0), (0,4), and (2,2):
             The point (0,2) is an ideal meeting point, as the total travel distance
             of 2+2+2=6 is minimal. So return 6.
```

# Solution
The meeting point is medium of all 1s. Time complexity is $$O(mnlog(mn))$$ due to sorting. The space complexity is $$O(mn)$$.
```python
class Solution(object):
    def minTotalDistance(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if len(grid) == 0 or len(grid[0]) == 0:
            return 0

        homes = list()
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    homes.append((i, j))

        if len(homes) == 0:
            return 0

        x = sorted([p[0] for p in homes])
        y = sorted([p[1] for p in homes])

        if len(homes) % 2 == 1:
            X, Y = x[len(homes) // 2], y[len(homes) // 2]
        else:
            xm = sum(x[len(homes) // 2 - 1:len(homes) // 2 + 1]) / 2
            ym = sum(y[len(homes) // 2 - 1:len(homes) // 2 + 1]) / 2

            def round(a):
                floor = int(a)
                if a - floor < floor + 1 - a:
                    return floor
                else:
                    return floor + 1

            X, Y = round(xm), round(ym)

        return sum([abs(X - p[0]) + abs(Y - p[1]) for p in homes])
```

However, we can get rid of the sorting by collecting `x` and `y` in order. This gives us a time complexity of $$O(mn)$$.

```python
class Solution(object):
    def minTotalDistance(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if len(grid) == 0 or len(grid[0]) == 0:
            return 0

        x = list()
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    x.append(i)

        if len(x) == 0:
            return 0

        size = len(x)
        y = list()
        for j in range(len(grid[0])):
            for i in range(len(grid)):
                if grid[i][j] == 1:
                    y.append(j)

        if size % 2 == 1:
            X, Y = x[size // 2], y[size // 2]
        else:
            xm = sum(x[size // 2 - 1:size // 2 + 1]) / 2
            ym = sum(y[size // 2 - 1:size // 2 + 1]) / 2

            def round(a):
                floor = int(a)
                if a - floor < floor + 1 - a:
                    return floor
                else:
                    return floor + 1

            X, Y = round(xm), round(ym)

        return sum([abs(X - x_) for x_ in x]) + sum([abs(Y - y_) for y_ in y])
```
