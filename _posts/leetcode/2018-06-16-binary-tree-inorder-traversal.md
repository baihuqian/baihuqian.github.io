---
layout: "post"
title: "Leetcode 94: Binary Tree Inorder Traversal"
date: "2018-06-16 15:03"
---

# Question
Given a binary tree, return the inorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

Follow up: Recursive solution is trivial, could you do it iteratively?

# Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = list()
        stack = list()
        if root is None:
            return res

        stack.append(root)
        while len(stack) > 0:
            node = stack.pop()
            if node.left is not None or node.right is not None:
                left, right = node.left, node.right
                node.left = None
                node.right = None
                if right is not None:
                    stack.append(right)
                stack.append(node)
                if left is not None:
                    stack.append(left)
            else:
                res.append(node.val)

        return res
```
