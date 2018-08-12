---
layout: post
title: 'Leetcode 325: Maximum Size Subarray Sum Equals k'
date: '2018-08-11 14:38'
tags:
  - Leetcode
  - Review
  - DP
---

# Question
Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Note:

The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

Example 1:

```
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

Example 2:

```
Input: nums = [-2, -1, 2, 1], k = 1
Output: 2
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

Follow Up:

Can you do it in O(n) time?

# Solution
Use DP. Store the sum from 0 to `i` inclusive as map key, and `i` as value. For `i+1`, if the sum from 0 to `i+1` is `k`, then return. Otherwise, find if `sum - k` is in the map. If so, then the range `j+1` (where `j` is the value of key `sum-k`) to `i` sums to `k`.

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        int sum = 0, max = 0;
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            sum = sum + nums[i];
            if (sum == k) max = i + 1;
            else if (map.containsKey(sum - k)) max = Math.max(max, i - map.get(sum - k));
            if (!map.containsKey(sum)) map.put(sum, i);
        }
        return max;
    }
}
```
