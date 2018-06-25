---
layout: "post"
title: "Leetcode 133: Clone Graph"
date: "2018-06-23 22:21"
tags:
  - Leetcode
---

# Question
Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

# Solution
```python
# Definition for a undirected graph node
# class UndirectedGraphNode:
#     def __init__(self, x):
#         self.label = x
#         self.neighbors = []

class Solution:
    # @param node, a undirected graph node
    # @return a undirected graph node
    def cloneGraph(self, node):
        if node is None:
            return None

        processed_nodes = dict()
        new_node = UndirectedGraphNode(node.label)
        self.clone(node, new_node, processed_nodes)
        return new_node


    def clone(self, node, new_node, processed_nodes):
        processed_nodes[node.label] = new_node
        for neighbor in node.neighbors:
            if neighbor.label in processed_nodes:
                new_node.neighbors.append(processed_nodes[neighbor.label])
            else:
                new_neighbor = UndirectedGraphNode(neighbor.label)
                new_node.neighbors.append(new_neighbor)
                self.clone(neighbor, new_neighbor, processed_nodes)
```
