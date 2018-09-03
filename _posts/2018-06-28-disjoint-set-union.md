---
layout: post
title: Disjoint Set Union
date: '2018-06-28 22:23'
tags:
  - Algorithm
  - DataStructure
  - DSU
published: true
---

A Disjoint Set Union (DSU) data structure can be used to maintain knowledge of the connected components of a graph, and query for them quickly. In particular, we would like to support two operations:

* `dsu.find(node x)`, which outputs a unique id so that two nodes have the same id if and only if they are in the same connected component.
* `dsu.union(node x, node y)`, which draws an edge `(x, y)` in the graph, connecting the components with id `find(x)` and `find(y)` together.

To achieve this, we keep track of `parent`, which remembers the id of a smaller node in the same connected component. If the node is it's own parent, we call this the leader of that connected component.

A naive implementation of a DSU structure would look something like this:

```python
class DSU(object):
    def __init__(self):
        self.parent = range(1001) # maximum number of entries

    def find(self, x):
      while self.parent[x] != x: # While x isn't the leader
        x = self.parent[x]
      return x

    def union(x, y):
      self.parent[find(x)] = find(y)
```

We use two techniques to improve the run-time complexity: path compression, and union-by-rank.

Path compression involves changing the `x = parent[x]` in the `find` function to `parent[x] = find(parent[x])`. Basically, as we compute the correct leader for x, we should remember our calculation. After path compression, subsequent `find` will return the id in two steps.

Union-by-rank involves distributing the workload of find across leaders evenly. Whenever we `dsu.union(x, y)`, we have two leaders `xr`, `yr` and we have to choose whether we want `parent[x] = yr` or `parent[y] = xr`. We choose the leader that has a higher following to pick up a new follower. Specifically, the meaning of rank is that there are less than `2 ^ rank[x]` followers of `x`. This strategy can be shown to give us better bounds for how long the recursive loop in `dsu.find` could run for.

```python
class DSU(object):
    def __init__(self):
        self.par = range(1001)
        self.rnk = [0] * 1001

    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]

    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        elif self.rnk[xr] < self.rnk[yr]:
            self.par[xr] = yr
        elif self.rnk[xr] > self.rnk[yr]:
            self.par[yr] = xr
        else: # if both leader have the same rank, increase the rank by 1
            self.par[yr] = xr
            self.rnk[xr] += 1
        return True
```

# Complexity
Using both path compression, splitting, or halving and union by rank or size ensures that the amortized time per operation is only $$O(\alpha (n))$$, which is optimal, where $$\alpha (n)$$ is the inverse Ackermann function. This function has a value $$ \alpha (n)<5$$ for any value of n that can be written in this physical universe, so the disjoint-set operations take place in essentially **constant time**.

The space complexity is $$O(n)$$ because of the extra space used by `parent` and `rank`. For more information, see [wikipedia](https://en.wikipedia.org/wiki/Disjoint-set_data_structure#Time_complexity).
