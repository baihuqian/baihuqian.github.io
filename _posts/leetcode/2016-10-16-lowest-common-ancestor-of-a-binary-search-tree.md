---
layout: "post"
title: "Leetcode 235: Lowest Common Ancestor of a Binary Search Tree"
date: "2016-10-16 18:55"
tags: Leetcode
---

# Question

[Question link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow **a node to be a descendant of itself**).”

```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
```

For example, the lowest common ancestor (LCA) of nodes `2` and `8` is `6`. Another example is LCA of nodes `2` and `4` is `2`, since a node can be a descendant of itself according to the LCA definition.

# Solution

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        p_path = self.getPath(root, p)
        q_path = self.getPath(root, q)

        lca = root
        for i in range(1, min(len(p_path), len(q_path))):
            if p_path[i].val == q_path[i].val:
                lca = p_path[i]
            else:
                break
        return lca


    def getPath(self, root, p):
        if p.val == root.val:
            return [ root ]

        node_stack = []
        node_stack.append((root, [ root ]))
        while len(node_stack) != 0:
            (node, path) = node_stack[-1]
            if node.val == p.val:
                return path
            node_stack = node_stack[:-1]
            if node.left != None:
                node_stack.append((node.left, path + [ node.left ]))
            if node.right != None:
                node_stack.append((node.right, path + [ node.right ]))
```
