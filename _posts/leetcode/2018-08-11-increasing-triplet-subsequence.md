---
layout: "post"
title: "Leetcode 334: Increasing Triplet Subsequence"
date: "2018-08-11 17:07"
tags:
  - Leetcode
---

# Question
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

> Return true if there exists i, j, k
> such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.

Note: Your algorithm should run in O(n) time complexity and O(1) space complexity.

Example 1:
```
Input: [1,2,3,4,5]
Output: true
```

Example 2:
```
Input: [5,4,3,2,1]
Output: false
```

# Solution
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums == null || nums.length < 3) return false;
        int first = nums[0], second = 0, size = 1;
        for(int i = 1; i < nums.length; i++) {
            if(size == 1) {
                if(nums[i] > first) {
                    second = nums[i];
                    size++;
                } else if (nums[i] < first) {
                    first = nums[i];
                }
            } else if(size == 2) {
                if(nums[i] > second) return true;
                else if(nums[i] < first) {
                    first = nums[i];
                } else if (nums[i] > first && nums[i] < second) {
                    second = nums[i];
                }
            }
        }
        return false;
    }
}
```
