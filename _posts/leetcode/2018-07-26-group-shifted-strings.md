---
layout: "post"
title: "Leetcode 249: Group Shifted Strings"
date: "2018-07-26 20:10"
tags:
  - Leetcode
---

# Question

Given a string, we can "shift" each of its letter to its successive letter, for example: `"abc" -> "bcd"`. We can keep "shifting" which forms the sequence:
```
"abc" -> "bcd" -> ... -> "xyz"
```

Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

Example:

```
Input: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"],
Output:
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
```

# Solution
Convert each string to its shifted counterpart which starts with `'a'`. Strings with the same counterpart should be grouped together

```python
class Solution(object):
    def groupStrings(self, strings):
        """
        :type strings: List[str]
        :rtype: List[List[str]]
        """
        if len(strings) == 0:
            return []

        str_map = dict()
        for s in strings:
            delta = ord(s[0]) - ord('a')
            k = "".join([chr(ord(c) - delta) if c >= s[0] else chr(26 + ord(c) - delta) for c in s])
            if k not in str_map:
                str_map[k] = list()
            str_map[k].append(s)

        return str_map.values()
```
