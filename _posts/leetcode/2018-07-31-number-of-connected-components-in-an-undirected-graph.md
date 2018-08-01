---
layout: "post"
title: "Leetcode 323: Number of Connected Components in an Undirected Graph"
date: "2018-07-31 22:29"
tags:
  - Leetcode
  - BFS
---

# Question
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:
```
Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4

Output: 2
```

Example 2:

```
Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
```

Note:

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.

# Solution
```python
from collections import defaultdict, deque
class Solution(object):
    def countComponents(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: int
        """
        class Graph(object):
            def __init__(self, vertices):
                self.graph = defaultdict(list)
                self.V = vertices

            def addEdge(self, u, v):
                self.graph[u].append(v)
                self.graph[v].append(u)

            def connectedComponents(self):
                unvisited = set(range(self.V))
                queue = deque()

                count = 0
                while len(unvisited) > 0:
                    count += 1
                    v = next(iter(unvisited))
                    unvisited.remove(v)
                    queue.append(v)
                    while len(queue) > 0:
                        v = queue.popleft()
                        for w in self.graph[v]:
                            if w in unvisited:
                                unvisited.remove(w)
                                queue.append(w)

                return count

        graph = Graph(n)
        for e in edges:
            graph.addEdge(e[0], e[1])

        return graph.connectedComponents()
```
