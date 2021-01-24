---
title: H题讨论归档
description: 
published: true
date: 2021-01-24T09:34:32.261Z
tags: 
editor: markdown
dateCreated: 2021-01-24T09:32:11.912Z
---

# H题讨论归档

## 讨论1
如果用WT01-B的做法的话，我们可以搞出来复杂度为O(m+logL)的dp做法。
按L的二进制从小到大dp，当构造出第一个大于m的字符串P之后，转移中会有四个KMP增量。如此一来预处理复杂度为KMP的O(m)，线性dp就好。

by 王孟晞