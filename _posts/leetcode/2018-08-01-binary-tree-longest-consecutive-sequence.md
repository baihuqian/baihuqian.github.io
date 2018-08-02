---
layout: "post"
title: "Leetcode 298: Binary Tree Longest Consecutive Sequence"
date: "2018-08-01 20:48"
tags:
  - Leetcode
---

# Question

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

Example 1:
```
Input:

   1
    \
     3
    / \
   2   4
        \
         5

Output: 3

Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
```

Example 2:

```
Input:

   2
    \
     3
    /
   2    
  /
 1

Output: 2

Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
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
    def longestConsecutive(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0

        stack = list()
        stack.append((root, 1))
        maxLen = 1
        while len(stack) > 0:
            node, pathLen = stack.pop()
            if node.left is not None:
                if node.val + 1 == node.left.val:
                    stack.append((node.left, pathLen + 1))
                    maxLen = max(maxLen, pathLen + 1)
                else:
                    stack.append((node.left, 1))
            if node.right is not None:
                if node.val + 1 == node.right.val:
                    stack.append((node.right, pathLen + 1))
                    maxLen = max(maxLen, pathLen + 1)
                else:
                    stack.append((node.right, 1))

        return maxLen
```
