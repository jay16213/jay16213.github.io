---
title: "Leetcode 隨筆 - 62. Unique Paths"
summary: ""
tags: ["leetcode", "dynamic-programming", "combinatorics"]
categories: []
date: 2022-11-01T22:46:23+08:00
lastmod: 2022-11-01T22:46:23+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Leetcode 隨筆 - 62
    identifier: leetcode-62
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Unique Paths](https://leetcode.com/problems/unique-paths/)

## 題目大綱
給你一個 `m x n` 的矩陣地圖, 請你找出從座標 `(0, 0)` 走到 `(m-1, n-1)` 一共有多少種走法。題目有限制每一次只能往下或往右走一步。

## 解題思路
### 解法一 - DP
設計一個 2 維陣列 `dp[m][n]`, 其中 `dp[i][j]` 對應到座標 `(i, j)`，代表了從 `(0, 0)` 走到 `(i, j)` 一共有多少種走法。

接著從題目的限制我們可以知道，要走到 `(i, j)` 的話一定要先走到 `(i-1, j)` 或是 `(i, j-1)`，然後再往下或往右走一步來到達 `(i, j)`。

也就是說，**走到 `(i, j)` 的走法數，會等於走到 `(i-1, j)` 的走法數加上走到 `(i, j-1)` 的走法數**

有了這層關係，我們就可以得出 dp 的公式如下：
```c++
dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
```

而一開始要初始化 `dp[i][0] = 1 (0 <= i <= m)` 以及 `dp[0][j] = 1 (0 <= j <= n)`，代表從起點開始，一路往右或往下都只有一種走法

### 解法二 - 數學解
由題目可知，一個 `m x n` 的地圖從 `(0, 0)` 要走到終點 `(m-1, n-1)`，不論過程怎麼走，總共都需要往下走 `m-1` 步及往右走 `n-1` 步。而這就是經典的高中排列組合的題目，故我們只需要算出 `m-1` 個下 ( &#8595; ) 跟 `n-1` 個右 ( &#8594; ) 一共有多少種組合即可求出答案。

因此答案就會是組合 `C((m-1) + (n-1), n-1)`

例如範例測資 `m = 3, n = 7`，則答案就是 `C(10, 7)` (C 10 取 7) = 28

## 程式碼
### 解法一
```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = 1;

        for (int i = 0; i < m; i++)
            dp[i][0] = 1;

        for (int i = 0; i < n; i++)
            dp[0][i] = 1;

        for (int i = 1; i < m; i++)
        {
            for (int j = 1; j < n; j++)
            {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }
};
```

### 解法二
這邊因為我發現如果直接將階層算完最後再除的話會遇到 overflow 的問題，故這邊我的寫法是先除掉一個階層的分母，然後邊算邊除來減小數字，所以程式看起來會比較 tricky 一點

```c++
class Solution {
public:
    int comb(int n, int k) {
        int k2 = n - k;
        int a = 1;
        int b = 1;

        int i = n, j = 2;
        while ((i > k) && (j <= k2))
        {
            a *= i;
            b *= j;

            if (a % b == 0)
            {
                a /= b;
                b = 1;
            }

            i--;
            j++;
        }

        for (; i > k; i--)
        {
            a *= i;
        }

        for (; j <= k2; j++)
        {
            b *= j;
        }

        return a / b;
    }

    int uniquePaths(int m, int n) {
        m--;
        n--;

        return comb(m+n, m > n ? m : n);
    }
};
```
