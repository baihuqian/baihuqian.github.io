---
layout: "post"
title: "Leetcode 680: Valid Palindrome II"
date: "2018-06-20 21:00"
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

Example 1:
```
Input: "aba"
Output: True
```

Example 2:
```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

Note:
1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

# Solution
```python
class Solution(object):
    def validPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                if self.validate(s[left+1: right+1]) or \
                self.validate(s[left: right]):
                    return True
                else:
                    return False
            left += 1
            right -= 1
        return True

    def validate(self, s):
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1

        return True
```
