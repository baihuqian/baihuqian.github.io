---
layout: "post"
title: "Leetcode 230: Kth Smallest Element in a BST"
date: "2018-07-08 22:16"
tags:
  - Leetcode
---

# Question
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note:

You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

Example 1:
```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

Example 2:

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

Follow up:

What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

# Solution

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        count = 0
        stack = list()
        stack.append(root)
        while len(stack) > 0:
            node = stack.pop()
            if node.left is None and node.right is None:
                if count == k - 1:
                    return node.val
                else:
                    count += 1
            else:
                if node.right is not None:
                    stack.append(node.right)
                stack.append(TreeNode(node.val))
                if node.left is not None:
                    stack.append(node.left)
```
