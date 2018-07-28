---
layout: post
title: 'Leetcode 32: Longest Valid Parentheses'
date: '2018-06-02 22:51'
tags:
  - Leetcode
  - DP
  - Hard
  - Review
---

# Question
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

Example 1:
```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

Example 2:
```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

# Solution 1: Dynamic Programming

We can use Dynamic Programming to calculate the longest valid parentheses end at every index. All valid parentheses end with ')'. Thus, for every index `i` of ')', there are two conditions:

1. If `i-1` is '(', then it forms a valid parentheses. The longest parentheses ending at `i` is that ending at `i-2` plus 2.
2. If `i-1` is ')', then if the character preceding longest valid parentheses ending at `i-1`, indexed at `j`, is '(', then it forms a valid parentheses. The length is that ending at `i-1` plus 2 plus that ending at `j-1`.

```python
class Solution:
    def longestValidParentheses(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) <= 1:
            return 0

        dp = [0] * len(s)

        for idx in range(1, len(s)):
            if s[idx] == ')':
                if s[idx - 1] == '(':
                    if idx >= 2:
                        dp[idx] = dp[idx - 2] + 2
                    else:
                        dp[idx] = 2
                else:
                    matching_idx = idx - 1 - dp[idx - 1]
                    if matching_idx >= 0 and s[matching_idx] == '(':
                        if matching_idx >= 1:
                            dp[idx] = dp[idx - 1] + 2 + dp[matching_idx - 1]
                        else:
                            dp[idx] = dp[idx - 1] + 2

        return max(dp)

```

# Solution 2: Use a stack
Instead of finding every possible string and checking its validity, we can make use of stack while scanning the given string to check if the string scanned so far is valid, and also the length of the longest valid string. In order to do so, we start by pushing −1 onto the stack.

For every `(` encountered, we push its index onto the stack.

For every `)` encountered, we pop the topmost element and subtract the current element's index from the top element of the stack, which gives the length of the currently encountered valid string of parentheses. If while popping the element, the stack becomes empty, we push the current element's index onto the stack. In this way, we keep on calculating the lengths of the valid substrings, and return the length of the longest valid string at the end.

```python
class Solution:
    def longestValidParentheses(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) <= 1:
            return 0

        stack = list()
        stack.append(-1)
        max_len = 0

        for idx, c in enumerate(s):
            if c == '(':
                stack.append(idx)
            else:
                stack.pop()
                if len(stack) == 0:
                    stack.append(idx)
                length = idx - stack[-1]
                if length > max_len:
                    max_len = length

        return max_len
```
