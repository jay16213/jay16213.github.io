---
title: "Leetcode 隨筆 - 152. Maximum Product Subarray"
summary: ""
tags: ["leetcode", "array", "dynamic-programming"]
categories: []
date: 2022-10-29T14:10:08+08:00
lastmod: 2022-10-29T14:10:08+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Leetcode 隨筆 - 152
    identifier: leetcode-152
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## 題目大綱
給你一個整數陣列, 找出擁有最大乘積的子陣列 (contiguous non-empty subarray)

## 程式碼

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int ans = nums[0];
        int max_product = 1;
        int min_product = 1;
        int t1, t2;

        for (int i = 0; i < nums.size(); i++)
        {
            t1 = max_product * nums[i];
            t2 = min_product * nums[i];
            max_product = max(t1, max(t2, nums[i]));
            min_product = min(t1, min(t2, nums[i]));
            ans = max(max_product, ans);
        }
        return ans;
    }
};
```
