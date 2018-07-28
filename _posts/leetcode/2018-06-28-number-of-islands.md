---
layout: post
title: 'Leetcode 200: Number of Islands'
date: '2018-06-28 22:53'
tags:
  - Leetcode
  - Hard
  - DSU
  - Review
---

# Question
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:

```
Input:
11110
11010
11000
00000

Output: 1
```

Example 2:
```
Input:
11000
11000
00100
00011

Output: 3
```

# Solution
Use Disjoint Set Union.

```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        class UnionFind(object):
            def __init__(self, grid):
                self.count = 0

                self.x, self.y = len(grid), len(grid[0])

                self.parent = [0 for _ in range(x * y)]
                self.rank = [0 for _ in range(x * y)]

                for i in range(self.x):
                    for j in range(self.y):
                        if grid[i][j] == '1':
                            index = self._index(i, j)
                            self.parent[index] = index
                            self.rank[index] = 1
                            self.count += 1

            def _index(self, i, j):
                return i * self.y + j

            def find(self, index):
                if self.parent[index] != index:
                    self.parent[index] = self.find(self.parent[index])
                return self.parent[index]

            def union(self, i, j, m, n):
                xr, yr = self.find(self._index(i, j)), self.find(self._index(m, n))
                if xr == yr:
                    return
                if self.rank[xr] > self.rank[yr]:
                    self.parent[yr] = xr
                elif self.rank[xr] < self.rank[yr]:
                    self.parent[xr] = yr
                else:
                    self.parent[xr] = yr
                    self.rank[yr] += 1
                self.count -= 1

        if len(grid) == 0 or len(grid[0]) == 0:
            return 0

        x, y = len(grid), len(grid[0])
        unionFind = UnionFind(grid)

        for i in range(x):
            for j in range(y):
                if grid[i][j] == '1':
                    if i > 0 and grid[i - 1][j] == '1':
                        unionFind.union(i, j, i - 1, j)
                    if i < x - 1 and grid[i + 1][j] == '1':
                        unionFind.union(i, j, i + 1, j)
                    if j > 0 and grid[i][j - 1] == '1':
                        unionFind.union(i, j, i, j - 1)
                    if j < y - 1 and grid[i][j + 1] == '1':
                        unionFind.union(i, j, i, j + 1)

        return unionFind.count
```
