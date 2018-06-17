---
layout: post
title: 'Leetcode 102: Binary Tree Level Order Traversal'
date: '2018-06-16 16:20'
tags:
  - Leetcode
---

# Question
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
```
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
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
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        res = []
        node_q = list()
        if root is not None:
            node_q.append(root)

            while True:
                new_node_q = list()
                level = list()
                for n in node_q:
                    level.append(n.val)
                    if n.left is not None:
                        new_node_q.append(n.left)
                    if n.right is not None:
                        new_node_q.append(n.right)

                res.append(level)
                node_q = new_node_q

                if len(node_q) == 0:
                    break

        return res
```
