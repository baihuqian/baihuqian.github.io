---
layout: post
title: 'Leetcode 266: Palindrome Permutation'
date: '2018-07-28 17:31'
tags:
  - Leetcode
published: true
---


# Question
Given a string, determine if a permutation of the string could form a palindrome.

Example 1:

```
Input: "code"
Output: false
```
Example 2:

```
Input: "aab"
Output: true

```

Example 3:

```
Input: "carerac"
Output: true
```

# Solution
```python
from collections import Counter
class Solution(object):
    def canPermutePalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if len(s) <= 1:
            return True

        charCount = Counter(list(s))
        canHaveOdd = len(s) % 2 == 1
        for count in charCount.values():
            if count % 2 == 1:
                if canHaveOdd:
                    canHaveOdd = False
                else:
                    return False

        return True
```
