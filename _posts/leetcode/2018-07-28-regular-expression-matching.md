---
layout: post
title: 'Leetcode 10: Regular Expression Matching'
date: '2018-07-28 20:14'
tags:
  - Leetcode
  - DP
  - Hard
  - Review
---

# Question
Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the entire input string (not partial).

Note:

* `s` could be empty and contains only lowercase letters `a-z`.
* `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.
Example 1:

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

Example 2:

```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

Example 3:

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

Example 4:

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

Example 5:

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

# Solution
As the problem has an optimal substructure, it is natural to cache intermediate results. We ask the question `dp(i, j)`: does `s[i:]` and `p[j:]` match?

Without Kleene stars (the `*` wildcard character for regular expressions), the algorithm is simple. It answers whether `s[i]` must match `p[j]` and `s[i+1:]` matches `p[j+1:]`. When encountering `*`, there are two options:
1. Ignore it. Match `s[i:]` with `p[j+2:]`.
2. Use it. `s[i]` must match `p[j]` and the pattern `p[j:]` matches `s[i+1:]`.

In terms of time complexity, `dp(i, j)` can be called once for any `i, j`, and each call is $$O(1)$$. Thus the time complexity is $$O(SP)$$. The space complexity is $$O(SP)$$ as well because the result for every `i, j` can be cached.

```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        memo = dict()

        def dp(i, j):
            if (i, j) not in memo:
                if j == len(p):
                    res = i == len(s)
                else:
                    match = i < len(s) and p[j] in {s[i], '.'}
                    if j + 1 < len(p) and p[j + 1] == '*':
                        res = dp(i, j + 2) or (match and dp(i + 1, j))
                    else:
                        res = match and dp(i + 1, j + 1)

                memo[i, j] = res

            return memo[i, j]

        return dp(0, 0)
```
