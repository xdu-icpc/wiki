---
title: 2014 ICPC 上海邀请赛
description: 
published: true
date: 2020-10-09T05:22:19.782Z
tags: 
editor: undefined
dateCreated: 2020-10-09T05:19:17.842Z
---

# 2014 ICPC 上海区域赛

* 队伍：[GoodGame](/team/goodgame)
* 主办学校：上海大学

照例先放个[题目传送门](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=648)。

这是我第一次打区域赛，也是第一次在上海大学打比赛 (后来发现每次去上海大学就打破铜)。

这场命题方是 Google，题目极其毒瘤。

J 题是签到题，但我们队却不会，于是 [fpcsong](fpcsong) 开了个 I 题。这是个贪心，需要某数据结构加速一下，然后他居然说自己发明了个可以二分查找的链表。我赶紧制止了他，然后拿出 `std::multiset` 的文档现场教他怎么用。

然后他写挂了，最后 debug 半天发现打反了变量名。

在 debug 的间隙我开了 B (离线 + 线段树处理二维问题)，写得很长才过。

最后 J 题自闭了，全场都过的题我们没过。后来发现这个题并不需要在纸上进行详细推导，就打开文本编辑器按感觉一边写一边改就行，我在火车站等车没事干就写了一个，回去改 1 行就过了。不会利用机时一直是我的一大弱点 QAQ。

2 题 Cu 滚粗。

后来补了个 D 题 (树套树模板题)。