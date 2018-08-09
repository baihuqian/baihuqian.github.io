---
layout: "post"
title: "Leetcode 305: Number of Islands II"
date: "2018-08-02 22:37"
tags:
  - Leetcode
  - Hard
  - DSU
---

# Question
A 2d grid map of `m` rows and `n` columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example:

```
Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
```

Explanation:

Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
```
0 0 0
0 0 0
0 0 0
```

Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.
```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.
```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.
```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```

Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.
```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```

Follow up:

Can you do it in time complexity O(k log mn), where k is the length of the positions?

# Solution
```python
class Solution(object):
    def numIslands2(self, m, n, positions):
        """
        :type m: int
        :type n: int
        :type positions: List[List[int]]
        :rtype: List[int]
        """
        class DSU(object):
            def __init__(self, length):
                self.par = list(range(length))
                self.isLand = [False] * length
                self.rank = [1] * length
                self.count = 0

            def find(self, x):
                if self.par[x] != x:
                    self.par[x] = self.find(self.par[x])
                return self.par[x]

            def union(self, x, y):
                px, py = self.find(x), self.find(y)
                if px == py:
                    return False
                if self.rank[px] > self.rank[py]:
                    self.par[py] = px
                elif self.rank[px] < self.rank[py]:
                    self.par[px] = py
                else:
                    self.par[py] = px
                    self.rank[py] += 1
                self.count -= 1
                return True

            def addLand(self, x):
                self.isLand[x] = True
                self.count += 1

        def index(x, y):
            return x * n + y

        dsu = DSU(m * n)
        res = list()
        for p in positions:
            x, y = p[0], p[1]
            dsu.addLand(index(x, y))
            if x > 0 and dsu.isLand[index(x - 1, y)]:
                dsu.union(index(x, y), index(x - 1, y))
            if x < m - 1 and dsu.isLand[index(x + 1, y)]:
                dsu.union(index(x, y), index(x + 1, y))
            if y > 0 and dsu.isLand[index(x, y - 1)]:
                dsu.union(index(x, y), index(x, y - 1))
            if y < n - 1 and dsu.isLand[index(x, y + 1)]:
                dsu.union(index(x, y), index(x, y + 1))
            res.append(dsu.count)
        return res
```
