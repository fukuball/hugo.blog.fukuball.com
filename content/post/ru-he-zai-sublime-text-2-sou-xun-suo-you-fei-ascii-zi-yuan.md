---
title: "如何在 Sublime Text 2 搜尋所有非 ASCII 字元"
date: 2014-04-15T08:11:24+08:00
draft: false
tags:
- regex
- sublime text
---

寫程式有時會需要找出原始碼裡所有非 ASCII 字元，拜訪了一下 Google 大神得到了這個答案，筆記一下！

Regular Expression:

    [^\x00-\x7F]
