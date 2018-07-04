---
layout: "post"
title: "Leetcode 186: Reverse Words in a String II"
date: "2018-07-03 21:33"
tags:
  - Leetcode
---

# Question
Given an input string , reverse the string word by word.

Example:

```
Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```

Note:

* A word is defined as a sequence of non-space characters.
* The input string does not contain leading or trailing spaces.
* The words are always separated by a single space.

Follow up: Could you do it in-place without allocating extra space?

# Solution
Reverse the entire string, and then reverse word by word.

```python
class Solution(object):
    def reverseWords(self, str):
        """
        :type str: List[str]
        :rtype: void Do not return anything, modify str in-place instead.
        """
        str.reverse()

        left, right = 0, 0
        while left < len(str):
            space = left + 1
            while space < len(str) and str[space] != ' ':
                space += 1
            right = space - 1
            while right > left:
                str[left], str[right] = str[right], str[left]
                left += 1
                right -= 1

            left = space + 1
```
