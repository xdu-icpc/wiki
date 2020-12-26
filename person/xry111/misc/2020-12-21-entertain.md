---
title: 2020-12-21 欢乐赛
description: IG*A 杯
published: true
date: 2020-12-22T23:58:00.467Z
tags: 
editor: markdown
dateCreated: 2020-12-22T23:53:27.176Z
---

# 2020-12-21 IG\*A 杯 欢乐赛

* 题目：[2019 山东省赛](http://acm.zju.edu.cn/contest-materials/sdp2019/sdp2019_problems.pdf)
* 提交地址：ZOJ 4113 - 4125

## 赛事简介

[flukehn](/person/flukehn) 谦虚地说自己是因为某出题组出题才能打到第四，然后颓废的老年选手 IG\*A 说他打过的某场比赛也是这个组出题。于是 xry111 和 flukehn 就想打一打试试看看能不能打过 IG\*A。

因为 vjudge 拉不了新 ZOJ 的题，只好随手乱做题，也没仔细计时，只数题数

## 比赛/训练记录

### M. Sekiro

假题，让你迭代 $k \leq 10^9$ 次 $x = \lceil x / 2 \rceil$，输出结果。

可以发现迭代 $1000$ 次肯定收敛，然后暴力 $\min(k, 1000)$ 次就行了。

### L. Median

给了一堆偏序关系，让你输出所有可能是中位数的元素。就对于每个元素，算一下能确定比他大和比他小的元素有多少个就行了。

注意如果有矛盾的关系，应该全输出不可能。

本来应该拓扑排序 DP 的，但是数据范围就 $100$，随便 Floyd 一下就行了。

### H. Tokens on the Segments

给一堆平行于 $x$ 轴的线段，你可以在线段上打点，要求打的点 $x$ 坐标两两不同，问最多多少线段上可以打上点。

一开始读错了，读成可以打多少点了，就是上周 CSP 刚写的区间覆盖…… 居然还能过样例，交上去就 WA 了。读对以后就把线段按右端点排序，然后一个一个试，能打点就打在最左边的可行位置。用 `set` 乱维护一下连续的被打过点的 $x$ 区间就行了。

### F. Stones in the Bucket

给一堆桶，第 $i$ 个桶有 $a_i$ 个石子。每次可以扔掉一个石子，或者把一个石子移到另一个桶。问最少操作多少次，可以让每个桶有一样多的石子。

最后肯定要让每个桶都是 $a = \left \lfloor \frac{\sum_i a_i}{n} \right \rfloor$ 个石子，那就把比这个数字少的桶都补齐，多余的石子扔掉就行了，答案就是 $\sum_i [a_i < a] (a - a_i) + \sum_i a_i - na$。

### E. BaoBao Loves Reading

给你个 LRU 缓存，以及一个访问内存的操作序列，让你对于从 $1$ 到 $n$ 的每个缓存大小，输出需要从内存中取元素的次数。

其实就是对于每次访问，求一下上次访问这个元素后，直到这次访问时，中间访问过的不同元素的个数，然后后缀和即可。一开始想了个线段树套 `set` 的假算法，最后发现是 $\mathcal{O}(n^2 \log n)$，比暴力还慢。想了半天发现可以开个线段树，把元素按最后访问的时间插入进去，维护区间和，从左往右扫一遍。

### D. Game on a Graph

就是去年新生赛某个题，我怀疑去年 wjj 他们就是打了这场然后出了个原题。

拿 Python 随便写了一下结果跑了 $950$+ 毫秒，差点超时。

### C. Wandering Robot

给你一个操作序列 (长度 $10^5$，可能是上下左右)，重复这个序列 $10^9$ 次，输出整个移动过程中离出发点最远的 Manhattan 距离。

乱画一通发现肯定是在第一个或者最后一个周期中，暴力就行了。

一开始以为一定是最后一个周期，结果 WA 了一发。

### A. Calandar

签到题，给了你个铁华团专属火星日历让你算某天是星期几。

因为那个日历的奇怪性质，年和月都没用…… 直接 $(a + b - c) \bmod 5$ 就行了。

### B. Flipping Game

给你个 100 位二进制数 $s$，你每次必须给他异或一个 $x$，要求 $x$ 里面有 $m$ 位是 $1$。操作 $k$ 次，问能够得到数 $t$ 的方案数。

DP 一下，用 `f[i][j]` 表示 `i` 次操作后，有 `j` 位跟原来不同的方案数。最后是 `f[k][p]` 除以 $\binom{n}{p}$，$p$ 是 $s$ 和 $t$ 不同的位数。

一开始觉得 `j` 设成当前状态 $1$ 的个数也是等价的，结果最后发现不知道该除以啥，于是自闭了 $114514$ 分钟，提出了要跑 $1919810$ 年的假算法。

### K. Happy Equation

给你 $a$ 和 $p$，求

$$a^x \equiv x^a \pmod{2^p}$$

的解 ($1 \leq x \leq 2^p$) 的个数。

首先发现如果 $a$ 是奇数，则模意义下只有一个解 $x = a \bmod 2^p$。使用欧拉定理，证明它确实是解：

$$a^{a \bmod 2^p} \equiv a^{(a \bmod 2^p) \bmod 2^{p-1}} \equiv a^{a \bmod 2^{p-1}} \equiv a^a$$

使用数学归纳法证明，如果命题对于 $p$ 成立，那么对于 $p' = p+1$ 也成立。设 $k \in \{0, 1\}$。考虑

$$a^{2^p k + x \bmod 2^p} \equiv a^{x \bmod 2^p} \equiv a^x \pmod{2^{p+1}}$$
$$(2^p k + x \bmod 2^p)^a = \sum_j \binom{a}{j} (x \bmod 2^p)^{a-j} {（2^p k)}^j \equiv
(x \bmod 2^p)^a + 2^p k a (x \bmod 2^p)^{a-1} \pmod{2^{p+1}}$$

我们希望两个式子模 $2^{p+1}$ 同余，则它们模 $2^p$ 必然也同余。得到

$$a^x \equiv x^a \pmod{2^p}$$

根据归纳猜想，必有

$$x = a \bmod 2^p$$

代回原方程，有左边为

$$a^{2^p k + a \bmod 2^p} \equiv a^a \pmod{2^{p+1}}$$

右边为

$$(2^p k + a \bmod 2^p)^a \equiv (a \bmod 2^p)^a + 2^p ka (a \bmod 2^p)^{a-1}$$

如果 $a < 2^p$，$k = 0$ 时，显然左右两边都是 $a^a$，因此 $a \bmod 2^p = a$ 就是一个解。而 $k = 1$ 时，右边为

$$(a \bmod 2^p)^a + 2^p a (a \bmod 2^p)^{a-1} = (1 + 2^p) a^a$$

显然左右不同余，因此 $2^p + a \bmod 2^p$ 不是解。可见 $x' = a \bmod 2^p = a$ 对于 $p' = p + 1$ 是唯一的解。

如果 $a > 2^p$，有 $a \bmod 2^p = a \bmod 2^{p+1} - 2^p = x' - 2^p$。左边为

$$a^{2^p k + x' - 2^p} \equiv a^{x'}$$

右边为

$$(2^p (k-1) + x')^a \equiv x'^a + 2^p(k-1)a x'^{a-1} \equiv a^a + 2^p(k-1)a^a$$

我们一开始已经证明了 $a^{x'} \equiv x'^{a} \equiv a^a$。可见，$k = 1$ 时，$x'$ 是一个解。而 $k = 0$ 时，如果要同余，则有 $2^p(k-1)a^a \equiv 0$，这是不可能的，因为 $a$ 和 $k-1$ 都是奇数。可见 $x' = a \bmod 2^{p+1}$ 对于 $p' = p + 1$ 是唯一的解。

这个结论其实挺难推的，现场过了一堆人感觉全是打表找规律的。

我们解决了 $a$ 为奇数的情况，而 $a$ 为偶数就很简单了，因为这时只要 $x \geq p$ 的话左边就是 $0$，算一下有多少 $x \geq p$ 使得右边为 $0$，再暴力处理 $x < p$ 就行了。