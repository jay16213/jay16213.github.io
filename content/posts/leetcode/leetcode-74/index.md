---
title: "LeetCode 隨筆 - 74. Search a 2D Matrix"
summary: ""
tags: ["leetcode", "array", "binary-search"]
categories: []
date: 2022-06-18T23:38:19+08:00
lastmod: 2022-06-18T23:38:19+08:00
draft: false

# toha params
menu:
  sidebar:
    name: LeetCode 隨筆 - 74
    identifier: leetcode-74
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## 題目大綱
給你一個 `m x n` 的整數二維陣列和一個 value `target`，要你判斷 `target` 是否在這個陣列中。
題目保證這個二維陣列：
1. 每個 row 的數字的順序已經由小到大排序過了
2. 每個 row 的第一個數字一定比上一個 row 的最後一個數字還要大

## 解題思路
由題目可知，將這個二維陣列按照 row 展開成一維陣列後就是一個排序過的 array，所以我們就可以直接用 **二分搜 (Binary Search)** 來找 `target`。
需要注意的是傳入的是二維陣列，所以在寫 binary search 的時候需要轉換一維 & 二維之間的坐標系

一維 & 二維坐標系轉換範例：
假設我們要轉換一維陣列的 index `i` 到一個 `m x n` (`m` rows * `n` columns) 的二維陣列的 index `(row, col)`
```c++
row = i / n;
col = i % n;
```

## 程式碼
binary search 可以使用迴圈或遞迴的方式來實作，以下提供兩種版本供讀者參考

### 迴圈版
```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();

        int left = 0, right = m*n - 1;
        int i, j, mid;

        while (left <= right) {
            mid = (left + right) / 2;
            i = mid / n;
            j = mid % n;
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return false;
    }
};
```

### 遞迴版
```c++
class Solution {
public:
    int m, n;
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        m = matrix.size();
        n = matrix[0].size();
        return binarySearch(matrix, target, 0, m*n - 1);
    }

    bool binarySearch(vector<vector<int>>& matrix, int target, int begin, int end) {
        int mid;
        int i, j;

        if (begin > end)
            return false;

        mid = (begin + end) / 2;
        i = mid / n;
        j = mid % n;

        if (matrix[i][j] == target) {
            return true;
        } else if (matrix[i][j] > target) {
            return binarySearch(matrix, target, begin, mid-1);
        } else {
            return binarySearch(matrix, target, mid+1, end);
        }
    }
};
```
