---
layout: "post"
title: "Leetcode 97: Interleaving String"
date: "2018-08-16 22:38"
tags:
  - Leetcode
  - Hard
  - DP
  - Review
---

# Question
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

Example 1:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```

Example 2:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

# DP Solution
The DP array answers the following question: for a pair `(i, j)`, can we produce `s3[:i + j -1]` by interleaving `s1[:i]` and `s2[:j]`.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1.length() + s2.length() != s3.length())
            return false;

        boolean[][] dp = new boolean[s1.length() + 1][s2.length() + 1];

        for(int i = 0; i <= s1.length(); i++) {
            for(int j = 0; j <= s2.length(); j++) {
                if(i == 0 && j == 0)
                    dp[i][j] = true;
                else if(i == 0) {
                    dp[i][j] = dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
                } else if(j == 0) {
                    dp[i][j] = dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i - 1);
                } else {
                    dp[i][j] = (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1)) || (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1));
                }
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
```

Because only `i-1` is used to compute `i`, we can reduce the memory foot print by changing DP array to 1D.
```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1.length() + s2.length() != s3.length())
            return false;

        boolean[] dp = new boolean[s2.length() + 1];

        for(int i = 0; i <= s1.length(); i++) {
            for(int j = 0; j <= s2.length(); j++) {
                if(i == 0 && j == 0)
                    dp[j] = true;
                else if(i == 0) {
                    dp[j] = dp[j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
                } else if(j == 0) {
                    dp[j] = dp[j] && s1.charAt(i - 1) == s3.charAt(i - 1);
                } else {
                    dp[j] = (dp[j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1)) || (dp[j] && s1.charAt(i - 1) == s3.charAt(i + j - 1));
                }
            }
        }
        return dp[s2.length()];
    }
}
```

# Backtracking Solution
Time complexity is $$O(2^{m+n})$$.

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if(s1.length() + s2.length() != s3.length())
            return false;
        int[] count = new int[128];
        for(int i = 0; i < s1.length(); i++)
            count[s1.charAt(i)]++;
        for(int i = 0; i < s2.length(); i++)
            count[s2.charAt(i)]++;
        for(int i = 0; i < s3.length(); i++)
            if(--count[s3.charAt(i)] < 0)
                return false;

        return backtracking(s1, 0, s2, 0, s3, 0);
    }

    private boolean backtracking(String s1, int idx1, String s2, int idx2, String s3, int idx3) {
        if(s3.length() == idx3)
            return true;
        else {
            if(idx1 < s1.length() && s1.charAt(idx1) == s3.charAt(idx3)) {
                if(backtracking(s1, idx1 + 1, s2, idx2, s3, idx3 + 1))
                    return true;
            }
            if(idx2 < s2.length() && s2.charAt(idx2) == s3.charAt(idx3)) {
                if(backtracking(s1, idx1, s2, idx2 + 1, s3, idx3 + 1))
                    return true;
            }
            return false;
        }
    }
}
```
