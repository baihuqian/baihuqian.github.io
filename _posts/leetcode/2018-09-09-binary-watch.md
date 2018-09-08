---
layout: "post"
title: "Leetcode 401: Binary Watch"
date: "2018-09-09 13:23"
tags:
  - Leetcode
---

# Question
A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).

Each LED represents a zero or one, with the least significant bit on the right.

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

Example:

```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

Note:
* The order of output does not matter.
* The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
* The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

# Solution
```python
class Solution:
    def readBinaryWatch(self, num):
        """
        :type num: int
        :rtype: List[str]
        """
        if num == 0:
            return ["0:00"]
        comb = []
        self._backtrack(comb, [], 0, num)
        res = []
        for c in comb:
            hour = 8 * c[0] + 4 * c[1] + 2 * c[2] + c[3]
            if hour <= 11:
                minute = 32 * c[4] + 16 * c[5] + 8 * c[6] + 4 * c[7] + 2 * c[8] + c[9]
                if minute <= 9:
                    res.append("{}:0{}".format(hour, minute))
                elif minute <= 59:
                    res.append("{}:{}".format(hour, minute))

        return res

    def _backtrack(self, res, partial, idx, remaining):
        if remaining == 0:
            if idx == 10:
                res.append(partial)
            else:
                res.append(partial + [0] * (10 - idx))
        elif 10 - idx == remaining:
            res.append(partial + [1] * remaining)
        elif 10 - idx > remaining:
            self._backtrack(res, partial + [0], idx + 1, remaining)
            self._backtrack(res, partial + [1], idx + 1, remaining - 1)
```
