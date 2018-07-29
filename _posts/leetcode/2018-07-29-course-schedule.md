---
layout: "post"
title: "Leetcode 207: Course Schedule"
date: "2018-07-29 16:01"
tags:
  - Leetcode
  - TopologicalSort
---

# Question
There are a total of n courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Example 1:

```
Input: 2, [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0. So it is possible.
```

Example 2:

```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

Note:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

# Solution
Use DFS way of topological sort.

```python
from collections import defaultdict
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        class Graph(object):
            def __init__(self, vertices):
                self.graph = defaultdict(list)
                self.V = vertices

            def addEdge(self, u, v):
                self.graph[u].append(v)

            def tsUtil(self, v, temporary, permanent):
                if v not in temporary:
                    return False
                temporary.remove(v)
                for w in self.graph[v]:
                    if w in permanent:
                        if not self.tsUtil(w, temporary, permanent):
                            return False

                permanent.remove(v)
                return True

            def isTsPossible(self):
                temporary = set(range(self.V))
                permanent = set(range(self.V))

                while len(permanent) > 0:
                    if not self.tsUtil(next(iter(permanent)), temporary, permanent):
                        return False

                return True

        graph = Graph(numCourses)
        for p in prerequisites:
            u, v = p[1], p[0]
            graph.addEdge(u, v)

        return graph.isTsPossible()
```
