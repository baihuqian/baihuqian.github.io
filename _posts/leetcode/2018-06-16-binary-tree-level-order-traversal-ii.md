---
layout: "post"
title: "Leetcode 107: Binary Tree Level Order Traversal II"
date: "2018-06-16 16:36"
---

# Question
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
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
    def levelOrderBottom(self, root):
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

        res.reverse()
        return res
```
