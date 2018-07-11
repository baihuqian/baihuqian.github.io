---
layout: post
title: 'Leetcode 224: Basic Calculator'
date: '2018-07-09 21:40'
tags:
  - Leetcode
---

# Question
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

Example 1:

```
Input: "1 + 1"
Output: 2
```

Example 2:

```
Input: " 2-1 + 2 "
Output: 3
```

Example 3:

```
Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

Note:
* You may assume that the given expression is always valid.
* Do not use the eval built-in library function.

# Solution
```python
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) == 0:
            return 0

        stack = list()
        curr_num = 0
        in_number = False
        for c in s:
            if c.isdigit():
                curr_num = curr_num * 10 + int(c)
                in_number = True
            elif c == '+' or c == '-':
                if in_number:
                    stack.append(curr_num)
                    in_number = False
                    curr_num = 0
                stack.append(c)

            elif c == '(':
                if in_number:
                    stack.append(curr_num)
                    curr_num = 0
                    in_number = False
                stack.append(c)
            elif c == ')':
                if in_number:
                    stack.append(curr_num)
                    curr_num = 0
                    in_number = False

                res = 0
                curr = 0
                while stack[-1] != '(':
                    v = stack.pop()
                    if v == '+':
                        res += curr
                    elif v == '-':
                        res -= curr
                    else:
                        curr = v
                stack.pop() # remove corresponding (
                res += curr
                stack.append(res)
            if c == ' ':
                if in_number:
                    stack.append(curr_num)
                    curr_num = 0
                    in_number = False

        if in_number:
            stack.append(curr_num)

        res = 0
        curr = 0
        while len(stack) > 0:
            v = stack.pop()
            if v == '+':
                res += curr
            elif v == '-':
                res -= curr
            else:
                curr = v
        res += curr
        return res
```
