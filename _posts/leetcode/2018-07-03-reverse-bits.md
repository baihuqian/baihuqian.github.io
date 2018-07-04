---
layout: "post"
title: "Leetcode 190: Reverse Bits"
date: "2018-07-03 20:39"
tags:
  - Leetcode
---

# Question
Reverse bits of a given 32 bits unsigned integer.

Example:

```
Input: 43261596
Output: 964176192
Explanation: 43261596 represented in binary as 00000010100101000001111010011100,
             return 964176192 represented in binary as 00111001011110000010100101000000.
```

Follow up:

If this function is called many times, how would you optimize it?

# Solution
```python
class Solution:
    # @param n, an integer
    # @return an integer
    def reverseBits(self, n):
        bit = 31
        res = 0
        while n > 0:
            if n % 2 == 1:
                res += 2 ** bit
            n //= 2
            bit -= 1
        return res
```

Follow up: break into bytes, and cache the result for each byte.
