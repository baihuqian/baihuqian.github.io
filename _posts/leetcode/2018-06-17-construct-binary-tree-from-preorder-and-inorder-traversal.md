---
layout: post
title: 'Leetcode 105: Construct Binary Tree from Preorder and Inorder Traversal'
date: '2018-06-17 13:51'
tags:
  - Leetcode
---

# Question
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:

You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
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
Find the corresponding part for left tree and right tree, and build the tree recursively.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if len(preorder) == 0:
            return None
        if len(preorder) == 1:
            return TreeNode(preorder[0])

        root = preorder[0]
        rootNode = TreeNode(root)

        idx = 0
        while inorder[idx] != root:
            idx += 1
        inorder_left = inorder[:idx]
        preorder_left = preorder[1:idx + 1]
        left_node = self.buildTree(preorder_left, inorder_left)
        rootNode.left = left_node

        inorder_right = inorder[idx + 1:]
        preorder_right = preorder[idx + 1:]
        right_node = self.buildTree(preorder_right, inorder_right)
        rootNode.right = right_node

        return rootNode


```
