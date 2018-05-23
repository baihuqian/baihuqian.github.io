---
layout: "post"
title: "Leetcode 20: Valid Parentheses"
date: "2018-05-20 16:50"
tags:
  - Leetcode
---

# Question
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
1. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.
```
Example 1:

Input: "()"
Output: true

Example 2:

Input: "()[]{}"
Output: true

Example 3:

Input: "(]"
Output: false

Example 4:

Input: "([)]"
Output: false

Example 5:

Input: "{[]}"
Output: true
```

# Solution
```python
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if len(s) == 0:
            return True

        stack = list()

        for char in s:
            if char in ('(', '[', '{'):
                stack.append(char)
            else:
                try:
                    c = stack.pop()
                    if char == ')' and c != '(':
                        return False
                    if char == ']' and c != '[':
                        return False
                    if char == '}' and c != '{':
                        return False
                except IndexError:
                    return False

        return len(stack) == 0

```
