---
layout: "post"
title: "Leetcode 300: Longest Increasing Subsequence"
date: "2018-08-01 21:34"
tags:
  - Leetcode
  - Review
---

# Question
Given an unsorted array of integers, find the length of longest increasing subsequence.

Example:
```
Input: [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

Note:

* There may be more than one LIS combination, it is only necessary for you to return the length.
* Your algorithm should run in $$O(n^2)$$ complexity.

Follow up: Could you improve it to $$O(n log n)$$ time complexity?

# $$O(n^2)$$ DP Solution
```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0

        dp = [1] * len(nums)

        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)
```

# $$O(n log n)$$ Solution
In this approach, we scan the array from left to right. We also make use of a `dp` array initialized with all 0's. This `dp` array is meant to store the increasing subsequence up to and including the currently encountered element. For the element corresponding to the $$j^{th}$$ index `nums[j]`, we determine the correct position in the `dp` array, say $$i^{th}$$ index, such that `dp[:i+1]` forms a increasing subsequence including `nums[j]`. This can be achieved by binary search. Instead of inserting `nums[j]` into $$i^{th}$$ index of `dp` array, we override the existing value of `dp[i]`. We also keep track of the length of LIS so far, which would be the largest `i` plus 1. Therefore, after traversing through `nums`, we know the length of LIS, though the `dp` does not store the content of it.

The time complexity is $$O(nlogn)$$ because we perform $$n$$ binary searches. The space complexity is $$O(n)$$.

In Python, we can use `bisect` module to perform binary search. Note that it will return the index left to the same value if the value exists in the array, so we need to handle that.

```python
from bisect import bisect_left
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return len(nums)

        dp = [0] * len(nums)
        length = 0
        for n in nums:
            idx = bisect_left(dp, n, 0, length)
            if idx + 1 >= len(dp) or dp[idx + 1] != n:
                dp[idx] = n
            if idx == length:
                length += 1

        return length
```
