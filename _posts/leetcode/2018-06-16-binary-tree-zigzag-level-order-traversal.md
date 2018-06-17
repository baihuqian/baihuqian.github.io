---
layout: "post"
title: "Leetcode 103: Binary Tree Zigzag Level Order Traversal"
date: "2018-06-16 16:33"
tags:
  - Leetcode
---

# Question
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:

```
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

# Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        res = []
        if root is not None:
            level = 1
            nodes = list()
            nodes.append(root)
            while True:
                vals = [n.val for n in nodes]
                res.append(vals)
                new_nodes = list()

                if level % 2 == 1:
                    for i in range(len(nodes) - 1, -1, -1):
                        if nodes[i].right is not None:
                            new_nodes.append(nodes[i].right)
                        if nodes[i].left is not None:
                            new_nodes.append(nodes[i].left)
                else:
                    for i in range(len(nodes) - 1, -1, -1):
                        if nodes[i].left is not None:
                            new_nodes.append(nodes[i].left)
                        if nodes[i].right is not None:
                            new_nodes.append(nodes[i].right)

                nodes = new_nodes
                level += 1

                if len(nodes) == 0:
                    break

        return res
```
