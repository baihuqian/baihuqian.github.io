---
layout: "post"
title: "Leetcode 210: Course Schedule II"
date: "2018-07-29 15:32"
tags:
 - Leetcode
 - TopologicalSort
 - Review
---

# Question
There are a total of `n` courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course `0` you have to first take course `1`, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example 1:

```
Input: 2, [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .

```

Example 2:
```
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```

Note:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

# Solution
Topological ordering.

```python
from collections import defaultdict

class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        class Graph(object):
            def __init__(self, vertices):
                self.graph = defaultdict(list)
                self.V = vertices
                self.in_degree = [0] * self.V

            def addEdge(self, u, v):
                self.graph[u].append(v)
                self.in_degree[v] += 1

            def topologicalSort(self):
                res = list()
                stack = [v for v, d in enumerate(self.in_degree) if d == 0]

                while len(stack) > 0:
                    v = stack.pop()
                    res.append(v)

                    for w in self.graph[v]:
                        self.in_degree[w] -= 1
                        if self.in_degree[w] == 0:
                            stack.append(w)

                return res if len(res) == self.V else []

        graph = Graph(numCourses)
        for p in prerequisites:
            u, v = p[1], p[0]
            graph.addEdge(u, v)

        return graph.topologicalSort()

```
