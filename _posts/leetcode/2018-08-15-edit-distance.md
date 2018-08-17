---
layout: "post"
title: "Leetcode 72: Edit Distance"
date: "2018-08-15 21:07"
tags:
  - Leetcode
  - Review
  - DP
  - Hard
---

# Question
Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.

You have the following 3 operations permitted on a word:

1. Insert a character
1. Delete a character
1. Replace a character

Example 1:

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation:
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

Example 2:

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation:
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

# Solution
This is a classic problem of Dynamic Programming. We define the state `dp[i][j]` to be the minimum number of operations to convert `word1[0..i - 1]` to `word2[0..j - 1]`. The state equations have two cases: the boundary case and the general case. Note that in the above notations, both `i` and `j` take values starting from 1 as we reserve 0 for empty strings.

For the boundary case, that is, to convert a string to an empty string, it is easy to see that the minimum number of operations to convert `word1[0..i - 1]` to `""` requires at least `i` operations (deletions). In fact, the boundary case is simply:

1. `dp[i][0] = i`;
1. `dp[0][j] = j`.

Now let's move on to the general case, that is, convert a non-empty `word1[0..i - 1]` to another non-empty `word2[0..j - 1]`. Well, let's try to break this problem down into smaller problems (sub-problems). Suppose we have already known how to convert `word1[0..i - 2]` to `word2[0..j - 2]`, which is `dp[i - 1][j - 1]`. Now let's consider `word[i - 1]` and `word2[j - 1]`. If they are equal, then no more operation is needed and `dp[i][j] = dp[i - 1][j - 1]`. Well, what if they are not equal?

If they are not equal, we need to consider three cases:

1. Replace `word1[i - 1]` by `word2[j - 1]` (`dp[i][j] = dp[i - 1][j - 1] + 1` (for replacement));
1. Delete `word1[i - 1]` and `word1[0..i - 2] = word2[0..j - 1]` (`dp[i][j] = dp[i - 1][j] + 1` (for deletion));
1. Insert `word2[j - 1]` to `word1[0..i - 1]` and `word1[0..i - 1] + word2[j - 1] = word2[0..j - 1]` (`dp[i][j] = dp[i][j - 1] + 1` (for insertion)).

Make sure you understand the subtle differences between the equations for deletion and insertion. For deletion, we are actually converting `word1[0..i - 2]` to `word2[0..j - 1]`, which `costs dp[i - 1][j]`, and then deleting the `word1[i - 1]`, which costs 1. The case is similar for insertion.

Putting these together, we now have:

1. `dp[i][0] = i`;
1. `dp[0][j] = j`;
1. `dp[i][j] = dp[i - 1][j - 1]`, if `word1[i - 1] = word2[j - 1]`;
1. `dp[i][j] = min(dp[i - 1][j - 1] + 1, dp[i - 1][j] + 1, dp[i][j - 1] + 1)`, otherwise.

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();

        int[][] dp = new int[m + 1][n + 1];

        for(int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for(int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }

        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                if(word1.charAt(i - 1) == word2.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1];
                else
                    dp[i][j] = min(dp[i - 1][j - 1] + 1, dp[i - 1][j] + 1, dp[i][j - 1] + 1);
            }
        }
        return dp[m][n];
    }

    private int min(int a, int b, int c) {
        if(a <= b && a <= c) return a;
        else if (b <= a && b <= c) return b;
        else return c;
    }
}
```

Well, you may have noticed that each time when we update `dp[i][j]`, we only need `dp[i - 1][j - 1]`, `dp[i][j - 1]`, `dp[i - 1][j]`. In fact, we need not maintain the full `m*n` matrix. Instead, maintaining one column is enough. The code can be optimized to $$O(m)$$ or $$O(n)$$ space, depending on whether you maintain a row or a column of the original matrix.

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();

        int[] dp = new int[n + 1];

        for(int j = 0; j <= n; j++) {
            dp[j] = j;
        }

        for(int i = 1; i <= m; i++) {
            int[] newDp = new int[n + 1];
            newDp[0] = i;
            for(int j = 1; j <= n; j++) {
                if(word1.charAt(i - 1) == word2.charAt(j - 1))
                    newDp[j] = dp[j - 1];
                else
                    newDp[j] = min(dp[j - 1] + 1, dp[j] + 1, newDp[j - 1] + 1);
            }
            dp = newDp;
        }
        return dp[n];
    }

    private int min(int a, int b, int c) {
        if(a <= b && a <= c) return a;
        else if (b <= a && b <= c) return b;
        else return c;
    }
}
```

The time completely is $$O(mn)$$.
