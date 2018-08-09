---
layout: post
title: 'Leetcode 306: Additive Number'
date: '2018-08-08 22:20'
tags:
  - Leetcode
  - Backtracking
---

# Question
Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits `'0'-'9'`, write a function to determine if it's an additive number.

Note: Numbers in the additive sequence cannot have leading zeros, so sequence `1`, `2`, `03` or `1`, `02`, `3` is invalid.

Example 1:

```
Input: "112358"
Output: true
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8.
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

Example 2:

```
Input: "199100199"
Output: true
Explanation: The additive sequence is: 1, 99, 100, 199.
             1 + 99 = 100, 99 + 100 = 199
```

Follow up:

How would you handle overflow for very large input integers?

# Solution
```python
class Solution(object):
    def isAdditiveNumber(self, num):
        """
        :type num: str
        :rtype: bool
        """
        if len(num) <= 2:
            return False
        return self.backtracking(num, None, None)

    def backtracking(self, remaining, prev1, prev2):
        if prev1 is not None and prev2 is not None:
            if len(remaining) == 0:
                return True
            if remaining[0] == '0':
                if prev1 + prev2 != 0:
                    return False
                else:
                    return self.backtracking(remaining[1:], prev2, 0)

            max_remaining = int(remaining)
            if max_remaining == prev1 + prev2:
                return True
            elif max_remaining < prev1 + prev2:
                return False
            else:
                digits = 0
                while max_remaining > prev1 + prev2:
                    max_remaining //= 10
                    digits += 1
                    if max_remaining == prev1 + prev2:
                        return self.backtracking(remaining[-digits:], prev2, max_remaining)
                return False

        # Populate the 1st number
        elif prev1 is None:
            if remaining[0] == '0':
                return self.backtracking(remaining[1:], 0, None)

            # The length must be shorter or equal to half of the string length
            for i in range(1, len(remaining) // 2 + 1):
                if self.backtracking(remaining[i:], int(remaining[:i]), None):
                    return True
            return False

        # Populate the 2nd number
        else:
            if remaining[0] == '0':
                return self.backtracking(remaining[1:], prev1, 0)

            # The length of the remaining number must be larger or equal to the 1st number
            for i in range(1, len(remaining) - len(str(prev1)) + 1):
                if self.backtracking(remaining[i:], prev1, int(remaining[:i])):
                    return True
            return False
```
