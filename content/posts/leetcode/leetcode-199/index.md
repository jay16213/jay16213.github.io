---
title: "Leetcode 隨筆 - 199. Binary Tree Right Side View"
summary: ""
tags: ["leetcode", "array"]
categories: []
date: 2022-07-11T21:48:41+08:00
lastmod: 2022-07-11T21:48:41+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Leetcode 隨筆 - 199
    identifier: leetcode-199
    parent: leetcode
    weight: 10
---

題目難度：<kbd>Medium</kbd><br/>
> 題目連結: [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## 題目大綱
給你一個 binary tree, 請你找出：從右邊看這棵樹，能看到的 node 有哪些。

## 解題思路
這題翻譯過後就是，請你找出此 binary tree 的每一層的最右邊的 node，所以我們只需要使用 level order traverse 的方式把 tree 掃過一遍抓出每層最右邊的 node 就行了！

## 程式碼

實作採用 C++ STL 的 queue, 遍歷時加上每個 node 的 level 資訊，當每掃到新的 level 時就會把該 node 加進 ans 裡面並更新 level 資訊；再來因為我們需要最右邊的 node, 所以 traverse 順序改成先掃右邊的 subtree 再掃左邊 subtree 的方式比較方便實作。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        queue<pair<TreeNode*, int>> q;
        pair<TreeNode*, int> node;
        vector<int> ans;
        int cur_level = -1;

        if (root == NULL)
            return vector<int>();

        q.push(pair<TreeNode*, int>(root, 0));

        while (!q.empty()) {
            node = q.front();
            q.pop();

            if (node.second != cur_level) {
                ans.push_back(node.first->val);
                cur_level = node.second;
            }

            if (node.first->right != NULL) {
                q.push(pair<TreeNode*, int>(node.first->right, node.second + 1));
            }
            if (node.first->left != NULL) {
                q.push(pair<TreeNode*, int>(node.first->left, node.second + 1));
            }
        }

        return ans;
    }
};
```
