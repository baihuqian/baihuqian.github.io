---
layout: "post"
title: "Leetcode 255: Verify Preorder Sequence in Binary Search Tree"
date: "2018-07-26 22:12"
tags:
  - Leetcode
  - Review
---

# Question
Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

Consider the following binary search tree:

```
     5
    / \
   2   6
  / \
 1   3
```

Example 1:
```
Input: [5,2,6,1,3]
Output: false
```

Example 2:

```
Input: [5,2,1,3,6]
Output: true
```

Follow up:

Could you do it using only constant space complexity?

# Solution
Just like in-order traversal, we keep a stack of nodes (just their values) of which we're still in the left subtree. If the next number is smaller than the last stack value, then we're still in the left subtree of all stack nodes, so just push the new one onto the stack. If not, then we must now be in some right subtrees, so we pop all smaller ancestor values. When doing that, we know the lower bound of the next value, so we keep track of that.

To achieve constant space, we can abuse the original array by using it as the stack.

```python
class Solution(object):
    def verifyPreorder(self, preorder):
        """
        :type preorder: List[int]
        :rtype: bool
        """
        idx = -1
        low = float('-inf')
        for p in preorder:
            if p < low:
                return False
            while idx >= 0 and p > preorder[idx]:
                low = preorder[idx]
                idx -= 1
            idx += 1
            preorder[idx] = p
        return True
```
