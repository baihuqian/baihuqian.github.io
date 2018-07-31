---
layout: "post"
title: "Leetcode 286: Walls and Gates"
date: "2018-07-30 22:26"
tags:
  - Leetcode
  - BFS
---

# Question
You are given a m x n 2D grid initialized with these three possible values.

1. -1 - A wall or an obstacle.
2. 0 - A gate.
3. INF - Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

Example:

Given the 2D grid:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

After running your function, the 2D grid should be:
```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4

```

# Solution
Do a breath-first search.

```python
class Solution(object):
    def wallsAndGates(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: void Do not return anything, modify rooms in-place instead.
        """
        if len(rooms) == 0 or len(rooms[0]) == 0:
            return

        currIndexes = list()
        empty = set()
        x, y = len(rooms), len(rooms[0])
        for i in range(x):
            for j in range(y):
                if rooms[i][j] == 2147483647:
                    empty.add((i, j))
                elif rooms[i][j] == 0:
                    currIndexes.append((i, j))

        dist = 1
        while len(empty) > 0:
            if len(currIndexes) == 0:
                break

            curr = list()
            for i, j in currIndexes:
                self.process(i + 1, j, rooms, dist, empty, curr)
                self.process(i - 1, j, rooms, dist, empty, curr)
                self.process(i, j + 1, rooms, dist, empty, curr)
                self.process(i, j - 1, rooms, dist, empty, curr)
            dist += 1
            currIndexes = curr

    def process(self, i, j, rooms, dist, empty, curr):
        if (i, j) in empty:
            rooms[i][j] = dist
            empty.remove((i, j))
            curr.append((i, j))
```
