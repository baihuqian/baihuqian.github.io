---
layout: "post"
title: "Leetcode 375: Guess Number Higher or Lower II"
date: "2018-08-21 21:58"
tags:
  - Leetcode
  - Review
---

# Question
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.

However, when you guess a particular number x, and you guess wrong, you pay $x. You win the game when you guess the number I picked.

Example:

```
n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = $21.
```

Given a particular n ≥ 1, find out how much money you need to have to guarantee a win.

# Solution
The problem of finding the minimum cost of reaching the destination number choosing `i` as a pivot can be divided into the subproblem of finding the maximum out of the minimum costs of its left and right segments. For each segment, we can continue the process leading to smaller and smaller subproblems. This leads us to the conclusion that we can use DP for this problem.

We need to use a `dp` matrix, where `dp(i, j)` refers to the minimum cost of finding the worst number given only the numbers in the range `(i, j)`. Now, we need to know how to fill in the entries of this `dp`. If we are given a range that contains only a single number `k`, no matter what the number is the cost of finding that number is always 0 since we always hit the number directly without any wrong guess. Thus, firstly, we fill in all the entries of the `dp` which correspond to segments of length 1 i.e. all entries `dp(k, k)` are initialized to 0. Then, in order to find the entries for segments of length 2, we need all the entries for segments of length 1. Thus, in general, to fill in the entries corresponding to segments of length `len`, we need all the entries of length `len-1` and below to be already filled. Thus, we need to fill the entries in the order of their segment lengths. Thus, we fill the entries of `dp` diagonally.

Now, what criteria do we need to fill up the `dp` matrix? For any entry `dp(i, j)`, given the current segment length of interest is `len` i.e. if `len=j-i+1`, we assume as if we are available only with the numbers in the range `(i, j)`. For a particular `pivot`, the worst-case cost is `cost(i, j) = pivot + max(cost(i, pivot - 1), cost(pivot+1, j))`. To guarantee a win, we need to choose the pivot such that the cost is minimized for range `(i, j)`, thus: `dp(i,j) = min(pivot + max(dp(i, pivot + 1, dp(pivot + 1, n)))) for pivot in (i, j)`.

The time complexity is $$O(n^3)$$.

```java
class Solution {
    public int getMoneyAmount(int n) {
        if(n == 1) return 0;
        if(n == 2) return 1;

        int[][] dp = new int[n + 1][n + 1];
        for(int i = 1; i <= n; i++) {
            dp[i][i] = 0; // range(i, i)
        }
        for(int i = 1; i < n; i++) {
            dp[i][i + 1] = i; // range(i, i + 1)
        }
        for(int len = 3; len <= n; len++) {
            for (int i = 1; i <= n - len + 1; i++) {
                int cost = Integer.MAX_VALUE;
                int j = i + len - 1;
                for(int pivot = i; pivot <= j; pivot++) {
                    cost = Math.min(cost, pivot + Math.max((pivot - 1 >= i ? dp[i][pivot - 1] : 0), (pivot + 1 <= j ? dp[pivot + 1][j] : 0)));
                }
                dp[i][j] = cost;
            }
        }
        return dp[1][n];
    }
}
```
