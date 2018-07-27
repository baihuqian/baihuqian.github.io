---
layout: "post"
title: "Leetcode 257: Binary Tree Paths"
date: "2018-07-26 22:52"
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a binary tree, return all root-to-leaf paths.

Note: A leaf is a node with no children.

Example:

Input:

```
   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3

```

# Solution
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        if root is None:
            return []
        res = []
        self.backtracking(res, [], root)
        return res


    def backtracking(self, res, partial, root):
        new_partial = partial + [str(root.val)]
        if root.left is None and root.right is None:
            res.append("->".join(new_partial))
        else:
            if root.left is not None:
                self.backtracking(res, new_partial, root.left)
            if root.right is not None:
                self.backtracking(res, new_partial, root.right)
```
