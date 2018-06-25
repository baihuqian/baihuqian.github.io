---
layout: "post"
title: "Leetcode 260: Single Number III"
date: "2018-06-24 14:38"
tags:
  - Leetcode
---

# Question
Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

Example:
```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```

Note:

1. The order of the result is not important. So in the above example, [5, 3] is also correct.
1. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

# Solution
Assuming the two numbers are `a` and `b`, we can get `a xor b` by `xor`ing all numbers together. Because `a` and `b` are distinct, there must be at least a bit in `a xor b` is set to 1. Thus, we can group the numbers into two groups, one with this bit set and the other with this bit not set, and `a` and `b` will belong to different groups. Thus, we can XOR numbers in the same group together to get them, or we can get one, say `a`, and get `b` by `(a xor b) xor a`.

To get a set bit in `a xor b`, denoted as `diff` we could get the least set bit by `diff & ~(diff - 1)`. Why? `diff - 1` will unset the rightmost set bit while setting all bits to its right and keeping all bits to its left unchanged:

```
**..**10_00..00
after -1:
**..**01_11..11
```

Then the not operation flips the bit to 1 and all 1s to its right back to 0 while changing all bits to its left to be the opposite, thus only the set bit becomes one in the `and` operation. But why is `~(diff - 1) == -diff`? `~(diff - 1) = -(diff - 1) - 1 = -diff`.

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        res = 0
        for n in nums:
            res ^= n

        mask = res & -res

        one = 0
        for n in nums:
            if n & mask == 0: # Bit not set
               one ^= n

        two = res ^ one
        return [one, two]
```
