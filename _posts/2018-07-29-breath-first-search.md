---
layout: "post"
title: "Breath-First Search"
date: "2018-07-29 16:12"
tags:
  - Algorithm
---

Breadth-first search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. It uses the opposite strategy as depth-first search, which instead explores the highest-depth nodes first before being forced to backtrack and expand shallower nodes.

BFS can be used to
* Finding connected components,
* Finding shortest paths.

Breadth First Traversal (or Search) for a graph is similar to Breadth First Traversal of a tree. The only catch here is, unlike trees, graphs may contain cycles, so we may come to the same node again. To avoid processing a node more than once, we use a boolean visited array. For simplicity, it is assumed that all vertices are reachable from the starting vertex.

Time complexity is $$O(|V| + |E|)$$.

# Implementation
```python
from collections import defaultdict, deque

# This class represents a directed graph
# using adjacency list representation
class Graph:

    # Constructor
    def __init__(self):

        # default dictionary to store graph
        self.graph = defaultdict(list)

    # function to add an edge to graph
    def addEdge(self, u, v):
        self.graph[u].append(v)

    # Function to print a BFS of graph
    def BFS(self, s):

        # Mark all the vertices as not visited
        visited = [False] * len(self.graph)

        # Create a queue for BFS
        queue = deque()
        result = list()

        # Mark the source node as
        # visited and enqueue it
        queue.append(s)
        visited[s] = True

        while len(queue) > 0:
            v = queue.popleft()
            result.append(v)

            # Get all adjacent vertices of the
            # dequeued vertex v. If a adjacent
            # has not been visited, then enqueue it
            for w in self.graph[s]:
                if visited[w] == False:
                    visited[w] = True
                    queue.append(w)

        return result
```

# Shortest Path
Initialize `dist(v) = float('inf')`, and `dist(s) = 0` for the starting vertex `s`. When considering edge `(v, w)`, if `w` is not visited, then set `dist(w) = dist(v) + 1`.

# Connected Components
For undirected graph, finding the number of connected components can be done via BFS from different vertices until all of the vertices are visited.

For directed graph, we can compute strongly-connected components using Kosaraju's Two-Pass Algorithm, which is a DFS-based algorithm.

# Longest Path in an Undirected Tree
We can find longest path using two BFSs. The idea is based on the following fact: If we start BFS from any node x and find a node with the longest distance from x, it must be an end point of the longest path. It can be proved using contradiction. So our algorithm reduces to simple two BFSs. First BFS to find an end point of the longest path and second BFS from this end point to find the actual longest path.

Proof by contradiction:

Let `s`, `t` be a maximally distant pair. Let `u` be the arbitrary vertex. We have a schematic like

```
    u
    |
    |
    |
    x
   / \
  /   \
 /     \
s       t
```

where `x` is the junction of `s`, `t`, `u`.

Suppose that `v` is a vertex maximally distant from `u`. If the schematic now looks like

```
    u
    |
    |
    |
    x   v
   / \ /
  /   *
 /     \
s       t
```

then

`d(s, t) = d(s, x) + d(x, t) < d(s, x) + d(x, v) = d(s, v)`
where the inequality holds because `d(u, t) < d(u, v)`, `d(u, t) = d(u, x) + d(x, t)` and `d(u, v) = d(u, x) + d(x, v)`. Thus the contradiction as there exists another path which is longer than `s - t`.

There is a symmetric case where `v` attaches between `s` and `x` instead of between `x` and `t`.

The other case looks like

```
    u
    |
    *---v
    |
    x
   / \
  /   \
 /     \
s       t
```

Now,

```
d(u, s) <= d(u, v) = d(u, x) + d(x, v)
d(u, t) <= d(u, v) = d(u, x) + d(x, v)

d(s, t)  = d(s, x) + d(x, t)
         = d(u, s) + d(u, t) - 2 d(u, x)
        <= 2 d(x, v)

2d(s, t) <= d(s, t) + 2 d(x, v)
           = d(s, x) + d(x, v) + d(v, x) + d(x, t)
           = d(v, s) + d(v, t)
```

so max(d(v, s), d(v, t)) >= d(s, t) by an averaging argument, and v belongs to a maximally distant pair.
