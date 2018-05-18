---
layout: "post"
title: "Leetcode 6: ZigZag Conversion"
date: "2018-05-17 22:11"
tags: Leetcode
---

# Question
The string "`PAYPALISHIRING`" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "`PAHNAPLSIIGYIR`"

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

Example 1:
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

Example 2:
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```

# Solution
$$O(n)$$ time and space complexity solution by rearranging characters into the final string.

Each row's character starts at $$r$$th character in the string. The gap between two columns are $$2*(N_r-1)$$. Between two columns there will be one or two gaps. If there's only one gap, the gap size is $$2*(N_r-1)$$. If there are two gaps, the first gap is of size $$2*(N_r - r - 1)$$ and the second gap is of size $$2*r$$, where $$N_r$$ is total number of rows and $$r$$ is the 0-based row number.

```python
class Solution:
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        if numRows == 1:
            return s

        gap = 2 * (numRows - 1)
        out = list()
        for row in range(0, numRows):
            firstGap = gap - 2 * row
            secondGap = 2 * row
            i = row
            round = 0
            while i < len(s):
                out.append(s[i])
                if firstGap == 0 or secondGap == 0:
                    i += max(firstGap, secondGap)
                elif round % 2 == 0:
                    i += firstGap
                else:
                    i += secondGap
                round += 1

        return ''.join(out)
```
