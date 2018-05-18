---
layout: "post"
title: "Leetcode 5: Longest Palindromic Substring"
date: "2018-05-17 21:52"
tags: Leetcode
---

# Question

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
Example 2:
```
Input: "cbbd"
Output: "bb"
```

# Solution
Expand from center. There are $$2n-1$$ centers (each character and between two characters) that the palindrome can be positioned. Check the immediate left and right character are the same, and expand from there. It gives $$O(n^2)$$ time complexity and constant space complexity.
```python
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        if len(s) <= 1:
            return s

        len_s = len(s)
        pali = ""
        length = 0

        for pos in range(0, 2 * len_s):
            if pos % 2 == 0:
                left, right = pos // 2, pos // 2
            else:
                left, right = pos // 2 + 1, pos // 2

            i = 0
            while left - i - 1 >= 0 and right + i + 1 < len_s and s[left-i-1] == s[right+i+1]:
                i += 1

            right = right + i
            left = left - i
            if left <= right and right - left + 1 > length:
                pali = s[left:right+1]
                length = right - left + 1

        return pali
```
