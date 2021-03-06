---
title: 2020 西电 CCCC 选拔赛
description: 
published: true
date: 2020-10-14T12:13:21.833Z
tags: 
editor: undefined
dateCreated: 2020-10-11T17:22:29.219Z
---

# 2020 西电 CCCC 选拔赛

## 评测系统垃圾

以后千万不要用[奇怪的 XDOJ](/xdoj/strange) 了。

### 技术复盘

* 开场后很快出现提交积压情况，原因是服务器性能差 (初代至强 E3，机械硬盘)，以及由于配置问题未能打开更多的评测机。
* 提交积压后，由于 RabbitMQ 会将超过生存期的未处理消息直接丢弃，导致一些提交被放弃评测。这迫使选手重复提交相同代码，进一步增大了评测压力。
* 服务器出现停止响应的情况后，为了尽快恢复比赛的正常秩序，技术团队选择了强行重启服务。这导致 RabbitMQ 中积压的提交被直接丢弃，更进一步导致选手重复提交。
* 比赛无法正常进行，不得不参照 NOIP 赛制，在比赛结束时关闭评测，并分区进行提交。
* 比赛结束时积压了约 300 发提交，结束后立刻开始评测。但是在评测过程中据 [cdcq](/person/cdcq) 报，发现了 $\mathcal{O}(1)$ 的程序超时的情况。另外，如果程序在部分数据点出现超时，另一些数据点则通过，评测系统会返回“答案错”。这似乎就是 2015 年校赛出现过的问题。
* 最后不得不换系统进行重测。

### 教训

* 没有测试过的系统都不可靠。
* 在 OJ 中使用消息队列机制纯属过度工程，制造的麻烦比解决的问题更多。
* Windows 系统本质上不适合作为评测机或比赛服务器。
* 如果可以使用固态硬盘，应尽量使用。机械硬盘会严重拖慢评测。
* 技术团队应密切关注比赛状况，因为轻微故障最后可能导致一连串雪崩事件 (cascaded events)，导致无法控制的结局。

### 建议

* 建议 [XDOJ](/xdoj/strange) 开发团队立刻开展技术归零和管理归零工作
* 建议程序设计竞赛基地技术团队编写或改造更可靠的支持 IOI 赛制的评测系统，在明年 CCCC 选拔赛中淘汰 [XDOJ](/xdoj/strange)

## 重测

### 重测环境技术说明

* CPU：Intel Core i7-1065G7
* OS：Linux
* 内存限制为 1 GB
* 输出限制为 8 MB
* 栈限制为 8MB (Linux 默认)
* 忽略行末空格和文末回车
* 重测只会返回 CE、AC、TL、WA，不会返回 ML 或者 OL (会判为 RE)
* 编译器：GCC-10.1.0
* 编译选项 (与 CCCC 官方一致)：`-DONLINE_JUDGE -fno-tree-ch -O2 -Wall -std=c++14 -pipe -lm`
  - 如果编译失败，则自动改为 `-std=c++11` 重试；如果仍失败，则自动改为 `-std=c++03` 重试。
* 评测系统：[OPOJ](https://github.com/xdu-icpc/opoj) (其实就是 xry111 写的一堆脚本……)

### 重测时部分题目的更改

* 所有题目的输入输出数据均进行正规化处理，删除行末空格和文末空行，以及 `\r`
* L2-03 加入 SPJ (输出与答案的绝对误差不超过 $2 \times 10^{-3}$ 则判为正确)