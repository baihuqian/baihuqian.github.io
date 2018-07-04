---
layout: "post"
title: "Leetcode 205: Isomorphic Strings"
date: "2018-07-04 20:57"
tags:
  - Leetcode
---

# Question
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

Example 1:

```
Input: s = "egg", t = "add"
Output: true
```

Example 2:

```
Input: s = "foo", t = "bar"
Output: false
```

Example 3:
```
Input: s = "paper", t = "title"
Output: true
```

Note:

You may assume both s and t have the same length.

# Solution
```python
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False

        checked = [0] * len(s)
        for i in range(len(s)):
            if checked[i] == 0:
                checked[i] = 1
                for j in range(i + 1, len(s)):
                    if s[j] == s[i]:
                        if t[j] == t[i]:
                            checked[j] = 1
                        else:
                            return False
                    elif t[j] == t[i]:
                        return False

        for c in checked:
            if c == 0:
                return False

        return True

```

# Other Solutions
The basic idea is that occurrences of same character should be the same.

```python
def isIsomorphic1(self, s, t):
    d1, d2 = {}, {}
    for i, val in enumerate(s):
        d1[val] = d1.get(val, []) + [i]
    for i, val in enumerate(t):
        d2[val] = d2.get(val, []) + [i]
    return sorted(d1.values()) == sorted(d2.values())

def isIsomorphic2(self, s, t):
    d1, d2 = [[] for _ in xrange(256)], [[] for _ in xrange(256)]
    for i, val in enumerate(s):
        d1[ord(val)].append(i)
    for i, val in enumerate(t):
        d2[ord(val)].append(i)
    return sorted(d1) == sorted(d2)

def isIsomorphic3(self, s, t):
    return len(set(zip(s, t))) == len(set(s)) == len(set(t))

def isIsomorphic4(self, s, t):
    return [s.find(i) for i in s] == [t.find(j) for j in t]

def isIsomorphic5(self, s, t):
    return list(map(s.find, s)) == list(map(t.find, t))

def isIsomorphic(self, s, t):
    d1, d2 = [0 for _ in xrange(256)], [0 for _ in xrange(256)]
    for i in xrange(len(s)):
        if d1[ord(s[i])] != d2[ord(t[i])]:
            return False
        d1[ord(s[i])] = i+1
        d2[ord(t[i])] = i+1
    return True
```
