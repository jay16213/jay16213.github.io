---
title: "LeetCode 隨筆 - 203. Remove Linked List Elements"
summary: ""
tags: ["leetcode"]
categories: []
date: 2021-09-19T01:31:18+08:00
lastmod: 2021-09-19T01:31:18+08:00
draft: false

# toha params
menu:
  sidebar:
    name: LeetCode 隨筆
    identifier: leetcode
    weight: 500
---

題目難度：<kbd>Easy</kbd>

## 題目大綱
給你一個單向的 linked list, 再給你一個數字 `val`, 移除該 linked list 中所有 `Node.val == val` 的 node 並回傳處理完後的 linked list.

## 解題思路
分兩步驟來解：
1. 首先檢查 head node 是否符合條件，若 head 符合條件則一直執行 `head = head->next` ，不斷地把 head 截掉直到 head 是有效的 (`Node.val != val`)
2. 接著一個 node 一個 node 去歷遍整個 list, 每個 node 的操作為：看下一個 node (後續以 next node 代稱) 是否符合條件，若符合的話就把自己的 next 指標接到 next node 的 next 指標 (也就是移除該 next node)，並繼續比較新的 next node (也就是回到第 2 步剛開始的操作)，否則繼續往下 loop 剩餘的 node

> 理論上移除掉的 node 要記得 free 掉不然會 memory leak, 不過這只是練習而已 LeetCode 好像也沒檢查這個，有沒有 free 掉都可以 AC

## 程式碼
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* removeElements(struct ListNode* head, int val){
    struct ListNode* dhead = head;
    struct ListNode* tmp = NULL;

    while (dhead != NULL && dhead->val == val) {
        dhead = dhead->next;
    }

    for (struct ListNode* ptr = dhead; ptr != NULL && ptr->next != NULL;) {
        if (ptr->next->val == val) {
            tmp = ptr->next;
            ptr->next = ptr->next->next;
            free(tmp);
        } else {
            ptr = ptr->next;
        }
    }
    return dhead;
}
```
