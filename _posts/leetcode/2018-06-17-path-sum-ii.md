---
layout: "post"
title: "Leetcode 113: Path Sum II"
date: "2018-06-17 17:53"
tags:
  - Leetcode
---

# Question
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.

Example:

Given the below binary tree and `sum = 22`,
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

Return:

```
[
   [5,4,11,2],
   [5,8,4,5]
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
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        if root is None:
            return []

        res = []
        self.backtracking(res, [], sum, root)
        return res

    def backtracking(self, res, path, residual, root):
        if root is None and residual == 0:
            res.append(list(path))

        new_path = path + [root.val]
        if root.left is None and root.right is None and root.val == residual:
            res.append(new_path)

        if root.left is not None:
            self.backtracking(res, new_path, residual - root.val, root.left)

        if root.right is not None:
            self.backtracking(res, new_path, residual - root.val, root.right)
```
