---
layout: "post"
title: "Leetcode 437: Path Sum III"
date: "2018-06-17 18:21"
tags:
  - Leetcode
---

# Question
You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
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
        :rtype: int
        """
        if root is None:
            return 0
        else:
            res = 0
            res += self.pathFromRoot(sum, root)
            if root.left is not None:
                res += self.pathSum(root.left, sum)
            if root.right is not None:
                res += self.pathSum(root.right, sum)
            return res

    # Find number of paths from root
    def pathFromRoot(self, residual, root):
        if root is None:
            if residual == 0:
                return 1
            else:
                return 0

        else:
            if root.left is None and root.right is None:
                if residual == root.val:
                    return 1
                else:
                    return 0
            else:
                res = 0 if residual != root.val else 1
                if root.left is not None:
                    res += self.pathFromRoot(residual - root.val, root.left)
                if root.right is not None:
                    res += self.pathFromRoot(residual - root.val, root.right)

                return res

```
