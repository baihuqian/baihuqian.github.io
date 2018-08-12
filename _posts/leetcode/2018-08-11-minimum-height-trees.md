---
layout: "post"
title: "Leetcode 310: Minimum Height Trees"
date: "2018-08-11 23:05"
tags:
  - Leetcode
  - BFS
---

# Question
For a undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

Format
The graph contains `n` nodes which are labeled from `0` to `n - 1`. You will be given the number n and a list of undirected edges (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in edges.

Example 1 :

```
Input: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3

Output: [1]
```

Example 2 :

```
Input: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5

Output: [3, 4]
```

Note:

* According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
* The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

# Solution
This problem becomes finding the mid-point(s) in the longest path in this tree. We can use two BFS to find the longest path in the tree. First, find the other end, say `e`, of the longest path from any node; second, find the longest path from `e`.

```java
class Solution {
    class Node {
        int id;
        List<Integer> neighbors;
        int dist;
        int prev;

        public Node(int id) {
            this.id = id;
            this.neighbors = new LinkedList<>();
            this.dist = -1;
            this.prev = -1;
        }

        public void addNode(int id) {
            this.neighbors.add(id);
        }

        public boolean visit(int dist, int prev) {
            if(this.dist == -1) {
                this.dist = dist;
                this.prev = prev;
                return true;
            } else return false;
        }

        public void clear() {
            this.dist = -1;
            this.prev = -1;
        }
    }
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if(n == 1) return Arrays.asList(0);

        Node[] nodes = new Node[n];
        for(int i = 0; i < n; i++) {
            nodes[i] = new Node(i);
        }
        for(int i = 0; i < edges.length; i++) {
            int from = edges[i][0], to = edges[i][1];
            nodes[from].addNode(to);
            nodes[to].addNode(from);
        }
        List<Integer> path = longestPath(nodes, 0);
        int end = path.get(0);
        for(int i = 0; i < n; i++) nodes[i].clear();
        path = longestPath(nodes, end);
        if(path.size() % 2 == 1)
            return path.subList(path.size() / 2, path.size() / 2 + 1);
        else
            return path.subList(path.size() / 2 - 1, path.size() / 2 + 1);
    }

    private List<Integer> longestPath(Node[] nodes, int start) {
        int distance = 0, furtherest = -1;
        nodes[start].visit(distance, -1);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        while(queue.size() > 0) {
            int node = queue.poll();
            distance++;
            for(int n: nodes[node].neighbors) {
                if(nodes[n].visit(distance, node)) {
                    queue.offer(n);
                    furtherest = n;
                }
            }
        }
        List<Integer> path = new LinkedList<>();
        path.add(furtherest);
        Node node = nodes[furtherest];
        while(node.dist != 0) {
            path.add(node.prev);
            node = nodes[node.prev];
        }
        return path;
    }    
}
```
