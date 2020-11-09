---
title: 2020 IEEExtreme
description: 
published: true
date: 2020-10-29T09:32:06.119Z
tags: 
editor: undefined
dateCreated: 2020-10-11T17:30:48.123Z
---

# 2020 IEEExtreme

* 参赛队：BigBigWang (队名致敬西电第一数据结构王 [wang9897](/person/wang9897))
* 队员：[qkoqhh](/person/qkoqhh)，[flukehn](/person/flukehn)，xry111

## 比赛记录

[fc]: /person/flukehn
[qko]: /person/qkoqhh
[wang9897]: /person/wang9897

* [fc] 一开场秒了 Mosiac Decoration I
* xry 一开始把 IEEEXplore Indexing 读错了，下载了个 Go 编译器以后发现并没有用，最后发现这题就是个题意很假的模拟……
* [fc] 读 Linearly Separable Samples 没读到 “直线必须通过原点” 这一条件，然后要用叫什么投影 SAT 的阴间算法…… 其实有了这个条件好像随便极角排序扫一遍就行了，但是感觉写起来很麻烦……
* [fc] 不想写这个题，这时放出了 Identifiying Infected，就是给个无向图，多次查询 $u$ 到 $v$ 的所有路径都会经过的点数。xry 说了题意以后 [fc] 就写了个假算法，最后发现就是圆方树裸题…… 然后圆方树又写假了……
* [qko] Linearly Separable Samples 交了两发假算法，结果又 WA 又 T
* xry 写 Identifiying Infected 自闭了两小时过了 (参考的 [wang9897] 的代码过于难以理解)，同时 [qko] 把 Linearly Spearable Samples 写到了 $50$ 分
* Craft Wooden Tables 是初中数学题，就是数据范围会导致中间结果爆 `long long`，xry 懒得想了于是直接 Python 强行过
* [qko] 写了 Making a Tangram 一直 WA，于是 xry 写了个 spj 把 $n \geq 5$ 全验证了一遍，最后发现 $n = 4$ 居然有解……
* xry 读错了 Rotational Lights 的题意，导致 [qko] 自闭…… 最后 xry 瞎 yy 了一个算法不知道为啥就过了
* [fc] 写 Magical Stones I 一直 WA，到吃晚饭时候终于发现错哪了。结果等 [fc] 回宿舍休息以后 xry 突然发现他忘了改这个题，只好重写了一遍，然后居然错得一样又改了半天 qaq
* Defense Walls 太难了，[fc] 写了个暴搜，样例没过，交上去却得了 50 分 orz
* Parkour [qko] 说是曼哈顿最小生成树，写了半天莫名其妙 WA 了，只得了 70 分
* Non-Overlapping Palindroms [fc] $n^2$ 暴力过 $10^5$，tql
* xry 写 Furin Back 直接把题目给的 Brainf\*\*k 翻译成 C，跑得贼慢，交上去会 T 一半的数据。xry 优化了半天也没优化出更多的分……
* Molecules 被 [qko] 直接秒了 orz
* Mosaic Decoration II 被 [fc] 直接秒了 orz
* The Last of Us 一看题面里的图就觉得不可做，直接放弃
* Game of Life 2020 就是个模拟 (而且好像是原题)，xry 交错语言结果 CE，然后换语言提交居然被提示 1 小时内不能提交重复代码，只好加了没用的注释 qaq
* Poker Game 是模拟 + DP，xry 写了半天过不了样例，最后发现这题居然规定 `A` 的字典序大于 `K`……
* Magical Stones II 看上去不太可做，[qko] 交随机过了 25 分
* Rescue Mission 就是个新生赛难度的题，随便写写就过了
* xry 一开始觉得 Restaurant Reopening 是平面图最大割，后来发现好像不对，最后 [fc] 暴力 38 分
* Coin Collector 是简单背包题，随便写写就过了
* Mosaic Decoration III 应该是个水题，但是我们两个人写了半天都 73 分，估计读错题了
* Coupon Codes 是暴力假题，随便写写就过了
* ARM Constant Multiplication 是 CCSP 风格的题，随便骗了几十分
* Social Distancing in Class 是力学题，xry 口胡了个假结论骗了点分

## 比赛结果

世界 rk 14，中国 rk 1，但是感觉打得并不好，因为[某电](https://www.uestc.edu.cn/)比我们多过一个 Magical Stones II。如果是 ICPC 赛制不能骗分，我们会被吊起来打。

另外如果 xry 能够再强一点，半夜多得一些分，就不需要 [fc] 早起写暴力，这样就不会影响 [fc] 的比赛状态导致 CCPC 场上爆炸。xry 太菜了 qaq。

## 补题

困死了，睡起来再说 zzz

* [x] Mosaic Decoration III (sb 题，但是口胡的一个假结论错了)
* [x] Linearly Separable Samples (极角排序，双指针)
* [ ] Defense Walls (不知道)
* [ ] Parkour (曼哈顿最小生成树)
* [ ] Furin Back (不知道)
* [ ] The Last of Us (题都没看)
* [ ] Magical Stones II (不知道)
* [ ] Restaurant Reopening (怀疑是平面图最大割)
* [ ] Social Distancing in Class (力学题)
* [ ] Molecules (思维题，虽然 qko 秒了，但是我不会)

### Mosaic Decoration III

啥都不说了，以后画图不要随手画出一些很巧的东西…… 另外暴力对拍很重要 qaq……

### Linearly Separable Samples

极角排序以后环上双指针扫描。

注意：

* 如果没有规定点都在一个象限，那么极角排序必须先比较象限再判断叉积，否则 `sort` 会跑飞掉 (不是偏序)。
* 千万不要偷懒，一定要把极角相同的 `p[i]` 判掉，否则可能出现绕一圈咬住自己的情况，很难处理。
* 恰好在直线上的点实在不行可以特判 (hash 一下斜率)，4 指针我实在不想写……

## 吐槽

比赛的时候每次交了题都刷不出结果，得到 my submission 里面看，后来发现是浏览器的 bug…… 

[WebKitGTK 2.30.2 Release Note:](https://webkitgtk.org/2020/10/23/webkitgtk2.30.2-released.html)

* ... ...
* Fix WebSocket requests with same-site cookies.
* ... ...