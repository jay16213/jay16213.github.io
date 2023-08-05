---
title: "Binary Search 中的 lower bound & upper bound"
summary: ""
tags: ['algorithm', 'binary-search']
categories: []
date: 2023-07-24T21:56:23+08:00
lastmod: 2023-07-24T21:56:23+08:00
draft: false

# toha params
menu:
  sidebar:
    name: Binary Search 中的 lower bound & upper bound
    identifier: binary-search
    parent: algorithm-note
    weight: 10
---

## 前言
c++ 的 STL 有提供兩個跟 binary seach 有關的 function: `lower_bound` 和 `upper_bound`, 每次在寫 leetcode 相關的題目時都會一直忘記這兩個 function 的定義，故決定紀錄一下兩者的定義及用法

#### Include STL
`lower_bound` 和 `upper_bound` 被定義在 `<algorithm>` 裡面
```c++
#include <algorithm>
```

### Lower Bound
找出數列中第一個 **大於或等於** target 的位置, 換句話說就是找到 `>= target` 的最小值的位置

```c++
// vector<int> v;
// assume v is a sorted array
auto it = lower_bound(v.begin(), v.end(), target);
int idx = it - v.begin();
```

### Upper Bound
找出數列中第一個 **大於** target 的位置, 換句話說就是找到 `> target` 的最小值的位置

```c++
// vector<int> v;
// assume v is a sorted array
auto it = upper_bound(v.begin(), v.end(), target) - v.begin();
int idx = it - v.begin();
```

<br>

`lower_bound` 和 `upper_bound` 都會回傳 對應型態的 iterator, 如果傳入的 sequence 找不到目標值，就會回傳 `v.end()`

### Custom Compare function
有時候已排序的陣列可能是自訂的資料結構，這時候可以透過自定義的 compare function 來使用 `lower_bound` 和 `upper_bound`
```c++
// define custom compare function
bool my_compare(const my_struct_t &lhs, const my_struct_t &rhs)
{
    // do something
    return result;
}

// example usage

// vector<my_struct_t> v;
// my_struct_t target;

auto it = lower_bound(v.begin(), v.end(), target, my_compare);
// auto it = upper_bound(v.begin(), v.end(), target, my_compare);
```
