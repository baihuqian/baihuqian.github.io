---
layout: post
title: 'Leetcode 222: Count Complete Tree Nodes'
date: '2018-07-08 21:28'
tags:
  - Leetcode
  - Review
---

# Question
Given a complete binary tree, count the number of nodes.

Note:

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Example:

```
Input:
    1
   / \
  2   3
 / \  /
4  5 6

Output: 6
```

# DFS Solution
DFS until find the rightmost element in the last row. $$O(n)$$.
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0

        stack = list()
        node = root
        max_lvl = 1
        while node.left is not None:
            if node.right is None:
                return 2 ** max_lvl
            else:
                stack.append((max_lvl + 1, node.right))
            node = node.left
            max_lvl += 1

        lvl_count = 1
        while len(stack) > 0:
            lvl, node = stack.pop()
            if lvl == max_lvl:
                lvl_count += 1
            elif lvl == max_lvl - 1:
                if node.left is None:
                    return 2 ** (max_lvl - 1) - 1 + lvl_count
                else:
                    lvl_count += 1
                if node.right is None:
                    return 2 ** (max_lvl - 1) - 1 + lvl_count
                else:
                    lvl_count += 1
            else:
                stack.append((lvl + 1, node.right))
                stack.append((lvl + 1, node.left))

        return 2 ** max_lvl - 1
```

# Divide-and-Conquer Solution

The height of a tree can be found by just going left. Let a single node tree have height 1. Find the height h of the whole tree. If the whole tree is empty, i.e., has height 0.

Check whether the height of the right subtree is just one less than that of the whole tree, meaning left and right subtree have the same height.

* If yes, then the last node on the last tree row is in the right subtree and the left subtree is a full tree of height `h-1`. So we take the $$2^{h-1}-1$$ nodes of the left subtree plus the 1 root node plus recursively the number of nodes in the right subtree.
* If no, then the last node on the last tree row is in the left subtree and the right subtree is a full tree of height `h-2`. So we take the $$2^{h-2}-1$$ nodes of the right subtree plus the 1 root node plus recursively the number of nodes in the left subtree.

Since I halve the tree in every recursive step, I have $$O(log(n))$$ steps. Finding a height costs $$O(log(n))$$. So overall $$O(log(n)^2)$$.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0

        tree_height = self.height(root)
        if tree_height == 1:
            return 1

        if self.height(root.right) == tree_height - 1:
            return (1 << (tree_height - 1)) + self.countNodes(root.right)
        else:
            return (1 << (tree_height - 2)) + self.countNodes(root.left)

        return res

    def height(self, root):
        lvl = 0
        while root is not None:
            lvl += 1
            root = root.left
        return lvl
```
