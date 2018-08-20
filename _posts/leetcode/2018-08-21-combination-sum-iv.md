---
layout: "post"
title: "Leetcode 377: Combination Sum IV"
date: "2018-08-21 23:33"
tags:
  - Leetcode
---

# Question
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

Example:

```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

Follow up:

What if negative numbers are allowed in the given array?

How does it change the problem?

What limitation we need to add to the question to allow negative numbers?

# Solution
It is simple to figure out a backtracking solution:
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        Arrays.sort(nums);
        return backtracking(nums, target);
    }

    private int backtracking(int[] nums, int remaining) {
        if(remaining == 0) return 1;
        else {
            int count = 0;
            for(int n: nums) {
                if(n <= remaining) {
                    count += backtracking(nums, remaining - n);
                } else break;
            }
            return count;
        }
    }
}
```
However, it hits TLE quite quick. The main reason is that it counts the number of combinations for the partial solutions where `remaining < target` many times, and we can use a DP array to cache them.

```java
class Solution {
    private int[] dp;
    public int combinationSum4(int[] nums, int target) {
        Arrays.sort(nums);
        dp = new int[target + 1];
        Arrays.fill(dp, -1); // Use -1 as a notion that the value is not computed
        return helper(nums, target);
    }

    private int helper(int[] nums, int target) {
        if(dp[target] != -1)
            return dp[target];
        else if (target == 0) {
            dp[0] = 1;
            return 1;
        } else {
            int count = 0;
            for(int n: nums) {
                if(n <= target) count += helper(nums, target - n);
                else break;
            }
            dp[target] = count;
            return count;
        }
    }
}
```

We can convert the recursive version to an iterative version by computing each `i` (`1 <= i <= target`) iteratively:

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] comb = new int[target + 1];
        comb[0] = 1;
        for(int i = 1; i <= target; i++) {
            for(int j = 0; j < nums.length; j++) {
                if(i - nums[j] >= 0)
                    comb[i] += comb[i - nums[j]];
            }
        }
        return comb[target];
    }
}
```
