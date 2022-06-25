---
title: "Leetcode 隨筆 - 665. Non-decreasing Array"
summary: ""
tags: ["leetcode", "array"]
categories: []
date: 2022-06-25T13:44:48+08:00
lastmod: 2022-06-25T13:44:48+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Leetcode 隨筆 - 665
    identifier: leetcode-665
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Non-decreasing Array](https://leetcode.com/problems/non-decreasing-array/)

## 題目大綱
給你一個陣列 `nums`，請你檢查是否可以在 "最多修改一個元素的值" 的限制下，把這個陣列變成一個非遞減陣列 (原來就已經是非遞減陣列也可以算通過檢查)

非遞減定義：
`nums[i] <= nums[i+1], where 0 <= i <= nums.size()-2`

## 解題思路
首先需要把陣列從頭開始掃描一遍，遇到 `nums[i+1] < nums[i]` 時停下來檢查是否可以修改元素值來完成題目要求，如果可以改，那就紀錄本次修改。所以我們可以用一個 counter 來記錄修改次數，而若修改次數大於 1 次就可以提前結束不用再往後檢查了。

接下來重點就是：要怎麼確認可以修改(改成非遞減)？
這裡可以分成幾個 case 來討論：

#### Case 1. 陣列的開頭 or 結尾發生遞減情況
也就是 `nums[0] > nums[1]` or `nums[nums.size()-1] > nums[nums.size()-2]`，這時候一定可以修改，因為只要把陣列頭的值往下修或把陣列尾的值往上調就可以了，所以直接更新 counter (`counter++`) 即可。

#### Case 2. 陣列中間發生遞減情況
這時候發生了 `nums[i+1] < nums[i]`，為了判斷是否能夠只改一個值就能改成非遞減陣列，我們必須額外再抓出 `nums[i-1]` & `nums[i+2]` 這兩個元素來幫助我們判斷。

首先第一項檢查
> `nums[i+2]` 是否大於等於 `nums[i-1]` ?

因為若這個條件不符合，代表除了 `nums[i]` 跟 `nums[i+1]` 之外，在 `nums[i+2]` 這個位置又讓陣列發生了遞減情況，所以至少要改兩個地方，這樣絕對達不到題目要求，因此我們可以提早結束判斷、回傳失敗結果。

通過第一項檢查後，第二件我們要確認的是
> `nums[i]` 跟 `nums[i+1]` 的值是否**至少有一個**落在 [`nums[i-1]`, `nums[i+2]`] 這個區間？

若這個條件不符合，代表 `nums[i]` & `nums[i+1]` 都一定要被修改 (至少需要把它們兩個都放進這個區間內) 才可能達到非遞減，這樣就不符合題目要求了！

若通過檢查，代表我們只要把 `nums[i]` 的值往下調整，或是把 `nums[i+1]` 的值往上調整，就可以達到非遞減 (最簡單的情形就是 `nums[i] == nums[i+1]`)，只要選擇修改一個元素即可。因此，通過檢查後，我們可以更新 counter，然後繼續往後檢查陣列。

最後，掃完陣列，我們就可以透過判斷counter 是否小於 2 來確認是否能完成題目要求，當然，若在過程中發現 counter 已經等於 2 了那我們就可以提前回傳結果。

## 程式碼

在實作上，因為 counter >= 2 時就代表檢查失敗，所以在檢查過程中若發現失敗情形就可以直接把 counter 改成 2 讓迴圈提早結束。

```c++
class Solution {
public:
    bool inline inRange(int n, int low, int high) {
        return (low <= n && n <= high);
    }

    bool checkPossibility(vector<int>& nums) {
        int modified_cnt = 0;
        int len = nums.size();

        for(int i = 0; i < (len-1) && (modified_cnt < 2); i++) {
            if (nums[i+1] < nums[i]) {
                // first or last element -> can modify
                if ((i == 0) || (i+1 == len-1)) {
                    modified_cnt++;
                } else {
                    if ((nums[i+2] - nums[i-1]) < 0) {
                        modified_cnt = 2; // impossible to modify one element
                        break;
                    }

                    if (!inRange(nums[i], nums[i-1], nums[i+2]) && !inRange(nums[i+1], nums[i-1], nums[i+2])) {
                        modified_cnt = 2;
                        break;
                    } else {
                        modified_cnt++;
                    }
                }
            }
        }

        return modified_cnt < 2;
    }
};
```
