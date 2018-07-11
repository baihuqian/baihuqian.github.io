---
layout: "post"
title: "Leetcode 227: Basic Calculator II"
date: "2018-07-09 22:18"
tags:
  - Leetcode
---

# Question
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, `+`, `-`, `*`, `/` operators and empty spaces` `. The integer division should truncate toward zero.

Example 1:

```
Input: "3+2*2"
Output: 7
```

Example 2:
```
Input: " 3/2 "
Output: 1
```

Example 3:
```
Input: " 3+5 / 2 "
Output: 5
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
        should_eval = False

        s = s.replace(' ', '') # remove spaces
        for c in s:
            if c.isdigit():
                curr_num = curr_num * 10 + int(c)
            else:
                stack.append(curr_num)
                curr_num = 0
                if should_eval:
                    v1 = stack.pop()
                    op = stack.pop()
                    v2 = stack.pop()
                    if op == '*':
                        stack.append(v1 * v2)
                    else:
                        stack.append(v2 // v1)

                stack.append(c)
                if c == '+' or c == '-':
                    should_eval = False
                else:
                    should_eval = True

        stack.append(curr_num)
        if should_eval:
            v1 = stack.pop()
            op = stack.pop()
            v2 = stack.pop()
            if op == '*':
                stack.append(v1 * v2)
            else:
                stack.append(v2 // v1)

        res = 0
        while len(stack) > 1:
            v = stack.pop()
            op = stack.pop()
            if op == '+':
                res += v
            else:
                res -= v

        res += stack.pop()
        return res

```
