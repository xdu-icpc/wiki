---
title: 2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)
description: 
published: true
date: 2020-11-16T11:49:20.379Z
tags: 
editor: markdown
dateCreated: 2020-11-16T11:33:14.812Z
---

# 2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)
## 训练记录

Nov/13/2020 12:33(UTC+8)
\#define rdqmpa rqdmap

因为rdqmpa三天课设，这场只有Leachim和IGVA，但是因为Ubuntu装在rdqmpa的设备上。所以就决定双人双机，但是不能同时两个人操作（模拟在一个CPU的情况下开双进程）。

因为开了unofficial，开局有人过了A，于是IGVA冲A。Leachim跟着后面的榜单，看了D和I。I是一个博弈，Leachim最开始基于一个不知道真假的结论想了一个$O(n^3)$的暴力而且以为是$O(n^2)$，冲了一发TLE了。遂让IGVA写A去了。后来Leachim又优化了一下假算法的复杂度变成$O(n^2logn)$，又冲了一发WA了。IGVA发现A是个假题，Leachim于是把D丢给了IGVA。

虽然I签到了很多人，但是Leachim似乎思维已经被之前的算法带歪了，于是决定与IGVA换题。果然换题后的IGVA很快整出了I的结论，而Leachim也顺利的构造出来了D题。

IGVA随后去开了J题，Leachim看了B题和G题。IGVA发现没读懂J，又把J丢给了Leachim读了一遍，然后确认题意之后IGVA开始开J。Leachim觉得G更可做于是开G。很快IGVA发现J是个假题遂AC，去搞B题。

Leachim想G想的略有问题，于是WA了2发之后才搞定，此时IGVA的B题也WA了几发。

Leachim过来问IGVA发什甚么事了？
IGVA说我感觉我的dp很对。
Leachim说你dp能不能让我hack一下？
IGVA说可以。
啪很快啊，Leachim出了一个小数据，一个大数据，还把样例反过来输入了一下。
IGVA的dp都防出去了啊，放出去了，之后当然是很自信的把代码给Leachim看。按照传统Hack，三发不中自然是点到为止。IGVA准备收手，这时Leachim说你dp顺序有问题，突然改了一下样例。
IGVA的dp大意了，没有判断。
当时就WA了，不过没有事，二分钟后IGVA就排了个序AC了。
