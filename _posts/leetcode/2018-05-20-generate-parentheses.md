---
layout: "post"
title: "Leetcode 22: Generate Parentheses"
date: "2018-05-20 22:53"
tags:
  - Leetcode
---

# Question
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

# Solution

Build from previous results using dynamic programming:

```python
class Solution:
    def __init__(self):
        self.history = {}
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        return self.gen(n)

    def gen(self, n):
        if n in self.history:
            return self.history[n]
        if n == 1:
            self.history[n] = ["()"]
        else:
            curr_set = set()

            # Unique cases for n
            for p in self.gen(n-1):
                curr_set.add("(" + p + ")")

            # Split parentheses between left and right
            for left in range(1, n):
                right = n - left
                left_res = self.gen(left)
                right_res = self.gen(right)
                for l in left_res:
                    for r in right_res:
                        curr_set.add(l + r)
            self.history[n] = list(curr_set)

        return self.history[n]

```

# Solution 2: Backtracking
We only add parentheses when we know it will remain a valid sequence. We can do this by keeping track of the number of opening and closing brackets we have placed so far.

We can start an opening bracket if we still have one (of n) left to place. And we can start a closing bracket if it would not exceed the number of opening brackets.

```python
class Solution:
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        if n == 0: return ['']
        ans = []
        def backtrack(S = '', left = 0, right = 0):
            if len(S) == 2 * n:
                ans.append(S)
                return
            if left < n:
                backtrack(S+'(', left+1, right)
            if right < left:
                backtrack(S+')', left, right+1)

        backtrack()
        return ans
```

# Solution 3:
