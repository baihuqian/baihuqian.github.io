---
layout: "post"
title: "Leetcode 104: Maximum Depth of Binary Tree"
date: "2016-02-05 19:06"
tags: Leetcode
---

# Question
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

# Solution

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0
        q = list()
        depth = 1
        max_depth = 1
        q.append((root, depth))
        while True:
            try:
                (node, d) = q[0]
                del q[0]
                if d > max_depth:
                    max_depth = d
                if node.left != None:
                    q.append((node.left, d + 1))
                if node.right != None:
                    q.append((node.right, d + 1))
            except IndexError:
                break
        return max_depth
```
