---
layout: post
title: Topological Sorting
date: '2018-07-29 15:00'
tags:
  - Algorithm
  - TopologicalSort
published: true
---

Topological sorting for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge `(u, v)`, vertex `u` comes before `v` in the ordering. Topological Sorting for a graph is not possible if the graph is not a DAG. The first vertex in topological sorting is always a vertex with in-degree as 0 (a vertex with no incoming edges).

# Algorithm 1
1. Identify vertices that have no incoming edge (in-degree is 0).
2. Delete this vertex of in-degree 0 and all its outgoing edges from the graph, place it in the output.
3. Repeat 1 and 2 until graph is empty.

We can store each vertex's in-degree in an array. While there are vertices remaining in the array, find one with in-degree zero and output it, and reduce in-degrees of all vertices adjacent to it by 1, and then delete the vertex (or mark its in-degree to -1).

Time complexity:
* Initialize in-degree array: $$O(E)$$;
* Find vertex with in-degree 0: $$O(V)$$ to search for one. It performs $$V$$ times for a total of $$O(V^2)$$.
* Reduce in-degree of all vertices adjacent to a vertex: $$O(E)$$;
* Output and mark vertex: $$O(V)$$

For a total of $$O(V^2 + E)$$. To improve the time complexity, we can memorize the vertices with in-degree of 0 in a queue or stack while decrementing in-degrees of adjacent vertices. This way we can reduce the time complexity to $$O(V + E)$$.

```python
from collections import defaultdict

class Graph:
    def __init__(self, vertices):
      self.graph = defaultdict(list) #dictionary containing adjacency List
      self.V = vertices #No. of vertices
      self.in_degree = [0] * self.V

    # function to add an edge to graph
    def addEdge(self, u, v):
        self.graph[u].append(v)
        self.in_degree[v] += 1

    def topologicalSort(self):
        stack = [v for v, d in enumerate(self.in_degree) if d == 0]
        while len(stack) > 0:
          v = stack.pop()
          print(v)
          self.in_degree[v] = -1
          for w in self.graph[v]:
            self.in_degree[w] -= 1
            if self.in_degree[w] == 0:
              stack.append(w)

```

# Topological Sorting via Depth First Search (DFS)
In DFS, we print a vertex and then recursively call DFS for its adjacent vertices. In topological sorting, we need to print a vertex before its adjacent vertices. In DFS, we start from a vertex, we first print it and then recursively call DFS for its adjacent vertices. In topological sorting, we use a temporary stack. We don’t print the vertex immediately, we first recursively call topological sorting for all its adjacent vertices, then push it to a stack. Finally, print contents of stack. Note that a vertex is pushed to stack only when all of its adjacent vertices (and their adjacent vertices and so on) are already in stack.

# Implementation
This implementation assumes the input graph is DAG, otherwise it will not work. It can be fixed by having two marks: one temporary and one permanent. If a node is marked as temporarily visited and is visited again, it means the graph is not DAG.

```python
#Python program to print topological sorting of a DAG
from collections import defaultdict

#Class to represent a graph
class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list) #dictionary containing adjacency List
        self.V = vertices #No. of vertices

    # function to add an edge to graph
    def addEdge(self, u, v):
        self.graph[u].append(v)

    # A recursive function used by topologicalSort
    def topologicalSortUtil(self, v, visited, stack):

        # Mark the current node as visited.
        visited[v] = True

        # Recur for all the vertices adjacent to this vertex
        for i in self.graph[v]:
            if visited[i] == False:
                self.topologicalSortUtil(i, visited, stack)

        # Push current vertex to stack which stores result
        stack.append(v)

    # The function to do Topological Sort. It uses recursive
    # topologicalSortUtil()
    def topologicalSort(self):
        # Mark all the vertices as not visited
        visited = [False] * self.V
        stack =[]

        # Call the recursive helper function to store Topological
        # Sort starting from all vertices one by one
        for i in range(self.V):
            if visited[i] == False:
                self.topologicalSortUtil(i, visited, stack)

        # Print contents of the stack
        print(stack[::-1])
```
