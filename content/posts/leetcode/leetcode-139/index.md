---
title: "Leetcode 隨筆 - 139. Word Break"
summary: ""
tags: ["leetcode", "string", "dynamic-programming"]
categories: []
date: 2022-10-30T00:35:01+08:00
lastmod: 2022-10-30T00:35:01+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Leetcode 隨筆 - 139
    identifier: leetcode-139
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Word Break](https://leetcode.com/problems/word-break/)

## 題目大綱
給定一個字串 `s` 和一個字串陣列 `wordDict`, 請你判斷是否有辦法

## 程式碼
### 解法一
這是我看完 [NeetCode](https://www.youtube.com/watch?v=Sx9NNgInc3A) 影片的前半段的解題思路後想出來的解法
```c++
class Solution {
private:
    vector<int> word_len;
    int s_len;
    vector<int> dp;
public:
    void solve(string& s, int idx, vector<string>& wordDict)
    {
        if (idx >= s_len)
        {
            dp[s_len] = 1;
            return;
        }

        if (dp[idx] != -1)
            return;

        for (int i = 0; i < wordDict.size(); i++)
        {
            if (s.compare(idx, word_len[i], wordDict[i]) == 0)
            {
                solve(s, idx + word_len[i], wordDict);
                if (dp[s_len] == 1)
                    return;
            }
        }

        dp[idx] = 0;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        word_len.resize(wordDict.size(), 0);
        s_len = s.length();
        dp.resize(s_len + 1, -1);
        dp[s_len] = 0;

        for (int i = 0; i < wordDict.size(); i++)
        {
            word_len[i] = wordDict[i].length();
        }

        solve(s, 0, wordDict);
        return dp[s_len];
    }
};
```

### 解法二
NeetCode 提出的最佳解
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<bool> dp(s.length() + 1, false);
        dp[s.length()] = true;

        for (int i = s.length() - 1; i >= 0; i--)
        {
            for (string word : wordDict)
            {
                if (s.compare(i, word.length(), word) == 0)
                {
                    dp[i] = dp[i + word.length()];
                }
                if (dp[i])
                    break;
            }
        }

        return dp[0];
    }
};
```
