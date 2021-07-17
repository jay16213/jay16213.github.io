---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Git Stash Pop Conflict 解決方式"
subtitle: ""
summary: "git stash pop conflict solution"
authors: []
tags: ["Git"]
categories: []
date: 2018-02-10T23:59:38+08:00
lastmod: 2020-06-28T01:43:32+08:00
featured: false
draft: false

# toha params
menu:
  sidebar:
    name: Git Stash Pop Conflict 解決方式
    identifier: git-stash-pop-conflict
    weight: 500
---

相信有在使用 Git 的各位一定或多或少聽過 Git 的一項好用功能 \- **stash**，中文又稱**儲藏**，主要是在手邊有臨時工作時用來暫存檔案用的，詳細使用情境可以點[這裡](https://git-scm.com/book/zh-tw/v1/Git-%E5%B7%A5%E5%85%B7-%E5%84%B2%E8%97%8F-Stashing) 參考。


當 stash pop 遇到 conflict 時，最簡單的方式就是手動改程式碼並 commit 解決。但問題來了，**若在stash裡的是尚未完成的程式碼**，這個解決 conflict 的 commit 是不是就顯得多餘？這篇文章就是要解決這個麻煩的情況。


首先我們照正常解決 conflict 的流程

1. 看有哪些 conflict 的檔案，並把這些檔案的內容改成你要的樣子
2. commit 這些變更\(建議訊息寫 *WIP*，因為之後我們不要這個 commit 了\)


至此，你應該已經解決了你的 conflict，但是 log 上卻多了一個無用的、只是為了解決 conflict 的 commit。這時候下以下指令:
``` bash
$ git reset HEAD^
```

這樣就可以無用的 commit 給去掉，並且成功地解決了 conflict。現在你的 git status 應該會跟你 stash push 前的狀態一樣

### 原理

`git reset HEAD^`僅會還原 commit、並不會把程式碼退到上一個版本。我們就是利用這個特性來達成**解決conflict但不增加一個commit**的目的


##### 參考資料
- [Git stash](https://git-scm.com/book/zh-tw/v1/Git-%E5%B7%A5%E5%85%B7-%E5%84%B2%E8%97%8F-Stashing)
- [Stack overflow 原文](https://stackoverflow.com/questions/7751555/how-to-resolve-git-stash-conflict-without-commit)
