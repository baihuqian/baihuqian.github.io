---
layout: "post"
title: "Leetcode 140: Word Break II"
date: "2018-06-24 15:36"
tags:
  - Leetcode
  - DP
  - Backtracking
---

# Question
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

Note:

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

Example 1:
```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```

Example 2:
```
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```

Example 3:
```
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

# Solution 1: Backtracking
Worst case time complexity is $$O(n^n)$$, so it is TLE.

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: List[str]
        """
        res = list()
        self.backtracking(s, wordDict, len(s), res, [], 0)
        return res

    def backtracking(self, s, wordDict, length, res, partial, index):
        if index == length:
            res.append(' '.join(partial))
        else:
            for end in range(index + 1, length + 1):
                if s[index: end] in wordDict:
                    self.backtracking(s, wordDict, length, res, partial + [s[index: end]], end)
```
# Solution 2: DP
$$O(n^3)$$ complexity. It is prone to TLE if the string is long but not breakable.

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: List[str]
        """
        if len(s) == 0 or len(wordDict) == 0:
            return []
        if not self.wordBreak1(s, wordDict):
            return []

        dp = list()
        dp.append([])
        wordDict = set(wordDict)
        max_len = max([len(word) for word in wordDict])

        for i in range(1, len(s) + 1):
            res = list()
            for j in range(i - 1, max(-1, i - max_len - 1), -1):
                if s[j:i] in wordDict:
                    if j == 0:
                        res.append(s[j:i])
                    elif len(dp[j]) != 0:
                        for partial in dp[j]:
                            res.append("{} {}".format(partial, s[j:i]))
            dp.append(res)

        return dp[-1]
```
