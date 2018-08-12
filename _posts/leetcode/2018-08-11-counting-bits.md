---
layout: "post"
title: "Leetcode 338: Counting Bits"
date: "2018-08-11 00:43"
tags:
  - Leetcode
  - DP
---

# Question
iven a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example 1:

```
Input: 2
Output: [0,1,1]
```

Example 2:

```
Input: 5
Output: [0,1,1,2,1,2]
```

Follow up:

* It is very easy to come up with a solution with run time `O(n*sizeof(integer))`. But can you do it in linear time `O(n)` /possibly in a single pass?
* Space complexity should be O(n).
* Can you do it like a boss? Do it without using any builtin function like `__builtin_popcount` in c++ or in any other language.

# Solution
Calculate this from 0 to num. For each number, we can find the number of 1s by removing its leading 1 bit.
```java
class Solution {
    public int[] countBits(int num) {
        if(num == 0) {
            return new int[]{0};
        } else if (num == 1) {
            return new int[]{0, 1};
        }
        int[] res = new int[num + 1];
        res[0] = 0;
        res[1] = 1;
        int leadingBit = 2;
        int i = 0;
        while(leadingBit + i <= num) {
            res[leadingBit + i] = res[i] + 1;
            i++;
            if(leadingBit == i) {
                leadingBit *= 2;
                i = 0;
            }
        }
        return res;
    }
}
```
