---
layout: "post"
title: "Leetcode 261: Graph Valid Tree"
date: "2018-07-28 16:38"
tags:
  - Leetcode
  - DSU
---

# Question
Given n nodes labeled from 0 to n-1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

Example 1:

```
Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
```

Example 2:

```
Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
```

Note: you can assume that no duplicate edges will appear in `edges`. Since all `edges` are undirected, `[0,1]` is the same as `[1,0]` and thus will not appear together in `edges`.

# Solution
This problem is to determine whether the given `edges` connect all `n` nodes into a fully connected, acyclic graph. Use DSU.

```python
class Solution(object):
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        class DSU(object):
            def __init__(self, n):
                self.par = [i for i in range(n)]
                self.rank = [1] * n
                self.parts = n

            def find(self, k):
                if self.par[k] != k:
                    self.par[k] = self.find(self.par[k])
                return self.par[k]

            def union(self, x, y):
                xr, yr = self.find(x), self.find(y)
                if xr == yr:
                    return False

                if self.rank[xr] < self.rank[yr]:
                    self.par[xr] = yr
                elif self.rank[yr] < self.rank[xr]:
                    self.par[yr] = xr
                else:
                    self.par[xr] = yr
                    self.rank[yr] += 1
                self.parts -= 1
                return True

        dsu = DSU(n)
        for e in edges:
            if not dsu.union(e[0], e[1]):
                return False

        return dsu.parts == 1
```
