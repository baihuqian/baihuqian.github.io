---
layout: "post"
title: "Leetcode 145: Binary Tree Postorder Traversal"
date: "2018-06-24 20:52"
tags:
  - Leetcode
---

# Question
Given a binary tree, return the postorder traversal of its nodes' values.

Example:

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3
```

Output: [3,2,1]
Follow up: Recursive solution is trivial, could you do it iteratively?

# Solution
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []

        stack = []
        res = []
        stack.append(root)

        while len(stack) > 0:
            node = stack.pop()
            if node.left is not None or node.right is not None:
                stack.append(TreeNode(node.val)) # put the node back w/o any child
                if node.right is not None:
                    stack.append(node.right)
                if node.left is not None:
                    stack.append(node.left)
            else:
                res.append(node.val)

        return res
```
