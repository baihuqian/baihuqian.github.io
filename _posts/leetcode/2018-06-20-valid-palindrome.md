---
layout: "post"
title: "Leetcode 125: Valid Palindrome"
date: "2018-06-20 20:55"
tags:
  - Leetcode
---

# Question
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.

Example 1:
```
Input: "A man, a plan, a canal: Panama"
Output: true
```

Example 2:
```
Input: "race a car"
Output: false
```

# Solution
```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        left, right = 0, len(s) - 1

        while left < right:
            while not s[left].isalnum() and left < right:
                left += 1
            while not s[right].isalnum() and left < right:
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            else:
                left += 1
                right -= 1

        return True
```
