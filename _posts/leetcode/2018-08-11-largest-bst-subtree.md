---
layout: "post"
title: "Leetcode 333: Largest BST Subtree"
date: "2018-08-11 16:50"
tags:
  - Leetcode
---

# Question
Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

Note:
A subtree must include all of its descendants.

Example:

```
Input: [10,5,15,1,8,null,7]

   10
   / \
  5  15
 / \   \
1   8   7

Output: 3
Explanation: The Largest BST Subtree in this case is the highlighted one.
             The return value is the subtree's size, which is 3.
```

Follow up:

Can you figure out ways to solve it with O(n) time complexity?

# Solution
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int maxSize = 1;

    public int largestBSTSubtree(TreeNode root) {
        if(root == null) return 0;
        isBST(root);
        return maxSize;
    }

    // [BST size, min, max]
    private int[] isBST(TreeNode root) {
        if(root.left == null && root.right == null)
            return new int[]{1, root.val, root.val};

        int[] left = null, right = null;

        if(root.left != null)
            left = isBST(root.left);
        if(root.right != null)
            right = isBST(root.right);
        int size = 1, min = root.val, max = root.val;
        if(left != null) {
            if(left[0] != -1 && root.val > left[2]) {
                size += left[0];
                min = left[1];
            } else return new int[]{-1, 0, 0};
        }
        if(right != null) {
            if(right[0] != -1 && root.val < right[1]) {
                size += right[0];
                max = right[2];
            } else return new int[]{-1, 0, 0};
        }
        maxSize = Math.max(maxSize, size);
        return new int[]{size, min, max};
    }
}
```
