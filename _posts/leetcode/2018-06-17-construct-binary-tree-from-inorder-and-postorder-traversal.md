---
layout: "post"
title: "Leetcode 106: Construct Binary Tree from Inorder and Postorder Traversal"
date: "2018-06-17 13:56"
tags:
  - Leetcode
---

# Question
Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
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
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        if len(postorder) == 0:
            return None
        if len(postorder) == 1:
            return TreeNode(postorder[0])

        root = postorder[-1]
        root_node = TreeNode(root)

        idx = 0
        while inorder[idx] != root:
            idx += 1

        inorder_left = inorder[:idx]
        postorder_left = postorder[:idx]
        left_node = self.buildTree(inorder_left, postorder_left)
        root_node.left = left_node

        inorder_right = inorder[idx + 1:]
        postorder_right = postorder[idx:-1]
        right_node = self.buildTree(inorder_right, postorder_right)
        root_node.right = right_node

        return root_node

```
