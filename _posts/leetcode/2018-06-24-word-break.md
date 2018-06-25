---
layout: post
title: 'Leetcode 139: Word Break'
date: '2018-06-24 15:25'
tags:
  - Leetcode
  - DP
---

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

Example 1:
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

Example 2:
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

Example 3:
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

# Solution
The problem can be divided into sub problem: if the string can be divided into two and each part satisfies the requirement, then the string itself satisfies the requirement.

The DP part of the problem can be like this: the substring up to but not including index `i` satisfies the question. To solve this, we can further divide the substring (up to `i`) into two parts, `s[0:j]` and `s[j:i]`. `dp[j]` provides the answer for `s[0:j]` and we only need to verify `s[j:i]` is in `wordDict` or not. If there exists a `j` that satisfies both requirements, then `dp[i]` is True.

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        wordDict = set(wordDict)
        # a substring [0:i] satisfies the requirement
        dp = [False for _ in range(len(s) + 1)]
        dp[0] = True

        for i in range(1, len(s) + 1):
            for j in range(i):
                if dp[j] and s[j: i] in wordDict:
                    dp[i] = True
                    break

        return dp[-1]
```
