---
layout: "post"
title: "Leetcode 373: Find K Pairs with Smallest Sums"
date: "2018-08-20 23:26"
tags:
  - Leetcode
  - Review
---

# Question
You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair (u,v) which consists of one element from the first array and one element from the second array.

Find the k pairs (u1,v1),(u2,v2) ...(uk,vk) with the smallest sums.

Example 1:
```
Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

Return: [1,2],[1,4],[1,6]

The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

Example 2:

```
Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

Return: [1,1],[1,1]

The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

Example 3:

```
Given nums1 = [1,2], nums2 = [3],  k = 3

Return: [1,3],[2,3]

All possible pairs are returned from the sequence:
[1,3],[2,3]
```

# Solution
We can use a heap to store `k` candidate pairs. For every numbers in `nums1`, its best partner (yields min sum) always starts from `nums2[0]` since arrays are all sorted; And for a specific number in `nums1`, its next candidate should be `[this specific number] + nums2[current_partner_index + 1]`, unless out of boundary for `nums2`.

```java
class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        k = Math.min(nums1.length * nums2.length, k);
        List<int[]> res = new ArrayList<>(k);
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> (a[0] + a[1]) - (b[0] + b[1]));
        int l = 0, r = 0;
        for(int i = 0; i < Math.min(k, nums1.length); i++)
            q.offer(new int[]{nums1[i], nums2[0], 0});
        for(int i = 0; i < k; i++) {
            int[] arr = q.poll();
            res.add(new int[]{arr[0], arr[1]});
            if(arr[2] < nums2.length - 1)
                q.offer(new int[]{arr[0], nums2[++arr[2]], arr[2]});
        }
        return res;
    }
}
```
