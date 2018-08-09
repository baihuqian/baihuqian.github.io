---
layout: "post"
title: "Leetcode 290: Word Pattern"
date: "2018-08-02 21:48"
tags:
  - Leetcode
---

# Question
Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in `pattern` and a non-empty word in `str`.

Example 1:

```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```

Example 2:

```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```

Example 3:

```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```

Example 4:

```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```

Notes:

You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space.

# Solution
```python
class Solution(object):
    def wordPattern(self, pattern, str):
        """
        :type pattern: str
        :type str: str
        :rtype: bool
        """
        str = str.strip().split(' ')
        if len(str) != len(pattern):
            return False

        pattern_dict = dict()
        seen_words = set()

        for i, c in enumerate(pattern):
            if c in pattern_dict:
                if str[i] != pattern_dict[c]:
                    return False
            else:
                if str[i] not in seen_words:
                    pattern_dict[c] = str[i]
                    seen_words.add(str[i])
                else:
                    return False

        return True
```
