---
layout: "post"
title: "Leetcode 272: Closest Binary Search Tree Value II"
date: "2018-07-29 20:13"
tags:
 - Leetcode
 - Hard
---

# Question

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

Note:

* Given target value is a floating point.
* You may assume k is always valid, that is: k ≤ total nodes.
* You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

Example:

```
Input: root = [4,2,5,1,3], target = 3.714286, and k = 2

    4
   / \
  2   5
 / \
1   3

Output: [4,3]
```

Follow up:

Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

# Solution
To get an $$O(n)$$ solution, first perform a DFS of the BST to get sorted values. Then build the result.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def closestKValues(self, root, target, k):
        """
        :type root: TreeNode
        :type target: float
        :type k: int
        :rtype: List[int]
        """
        nodeVal = list()
        self.dfs(root, nodeVal)
        if target < nodeVal[0]:
            return nodeVal[:k]
        elif target > nodeVal[-1]:
            return nodeVal[-k:]
        else:
            i = 0
            while i < len(nodeVal) - 1:
                if nodeVal[i] <= target < nodeVal[i + 1]:
                    break
                else:
                    i += 1

            res = list()
            left, right = i, i + 1
            if target - nodeVal[left] <= nodeVal[right] - target:
                left_residue = 0
            else:
                left_residue = 1
            idx = 0
            while idx < k:
                if (idx % 2 == left_residue and left >= 0) or right == len(nodeVal):
                    res.append(nodeVal[left])
                    left -= 1
                else:
                    res.append(nodeVal[right])
                    right += 1
                idx += 1

            return res


    def dfs(self, root, res):
        if root is None:
            return
        if root.left is not None:
            self.dfs(root.left, res)
        res.append(root.val)
        if root.right is not None:
            self.dfs(root.right, res)
```
