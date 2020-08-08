---
layout: "post"
title: "Leetcode 378: Kth Smallest Element in a Sorted Matrix"
date: "2018-08-26 16:35"
tags:
  - Leetcode
  - Review
---


# Question
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

Note:

You may assume k is always valid, 1 ≤ k ≤ $$n^2$$.

# Solution
Like [Leetcode 373: Find K Pairs with Smallest Sums]({{ site.baseurl }}{% link _posts/leetcode/2018-08-20-find-k-pairs-with-smallest-sums.md %}), we can use a min heap to store "candidates" from each row, and pop the first `k - 1` candidates.

Time complexity $$O((n + k)logn)$$, space complexity $$O(n)$$.

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for(int i = 0; i < n; i++)
            queue.offer(new int[] {matrix[i][0], i, 0});
        for(int i = 0; i < k - 1; i++) {
            int[] p = queue.poll();
            if(p[2] != n - 1) queue.offer(new int[]{matrix[p[1]][++p[2]], p[1], p[2]});
        }
        return queue.poll()[0];
    }
}
```
