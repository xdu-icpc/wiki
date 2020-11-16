---
title: 2019-2020 XX Open Cup, Grand Prix of Korea
description: 
published: true
date: 2020-11-16T05:32:47.650Z
tags: 
editor: markdown
dateCreated: 2020-11-16T04:48:42.188Z
---

# 比赛记录
开局 RainCurtain 看A题，  pllj 看G题， FreeSin 看H题。
RainCurtain 在搞清楚题意后过了A题,用不到十分之一的罚时完成了整场比赛一半的输出。
FreeSin 很快便得出了要H题要满足的排列规律，pllj 打表验证确认无误，但（貌似）在实现上还需要存在一些问题，同时大家一致认为G题可做，于是 pllj 上去写G~~疯狂贡献罚时~~。在 WA 了三次之后打印代码到一旁debug~~自闭~~。由于 pllj 在G题代码实现上过于花里胡哨（企图通过一次dfs得出答案），RainCurtain随后也写了一发G但还是没过。于是FreeSin 和 RainCurtain 开始写H。
pllj 在花了大把时间分析代码实现无果之后随便构造了组数据便把自己的算法hack得死死的（然而这并不能hack掉RainCurtain的程序qaq），在此数据的基础上发现自己写的算法假得不能再假，便开始考虑如何修改代码。大致有了思路后上去写但在代码实现上又出现了一堆问题，~~瞎~~改了一些地方后过了样例和构造的数据，但提交后发现TLE了。随后在 FreeSin 的提醒下发现自己的算法很容易就能被卡到 $n^2$ 的复杂度，加了个vis数组后在离比赛结束还有10分钟的时候过了。
由于 pllj 在G题花费了太多时间，导致队友没有足够的时间写出H题和看新题，所以两题滚粗。