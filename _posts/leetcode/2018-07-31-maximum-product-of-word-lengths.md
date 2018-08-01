---
layout: "post"
title: "Leetcode 318: Maximum Product of Word Lengths"
date: "2018-07-31 20:50"
tags:
  - Leetcode
---

# Question
Given a string array words, find the maximum value of `length(word[i]) * length(word[j])` where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

Example 1:

```
Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16
Explanation: The two words can be "abcw", "xtfn".
```

Example 2:

```
Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4
Explanation: The two words can be "ab", "cd".
```

Example 3:

```
Input: ["a","aa","aaa","aaaa"]
Output: 0
Explanation: No such pair of words.
```

# Solution
Because each word has no more than 26 different characters, we can use an `int` to represent the set of characters. Then if bit-wise and `&` of such numbers of two words is 0, then there are no common characters in them.

```python
class Solution(object):
    def maxProduct(self, words):
        """
        :type words: List[str]
        :rtype: int
        """
        if len(words) <= 1:
            return 0

        charSets = [0] * len(words)

        for i in range(len(words)):
            for c in words[i]:
                charSets[i] |= 1 << (ord(c) - ord('a'))

        length = 0
        for i in range(len(words) - 1):
            for j in range(i + 1, len(words)):
                    if charSets[i] & charSets[j] == 0 and length < len(words[i]) * len(words[j]):
                        length = len(words[i]) * len(words[j])

        return length
```
