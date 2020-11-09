---
title: RainCurtain
description: 我才是废物
published: true
date: 2020-10-13T14:06:19.603Z
tags: 
editor: undefined
dateCreated: 2020-10-09T12:12:24.897Z
---

# RainCurtain

## fish in the pool
> The world is a dream in rain
 The splashes of water shines don't you see
 Watch out
Don't step on the fish in the pool
I'm a little soaking mouse
I'm wet with a blanket of rain
And I dream of you

# OJ刷题单



## 数学

### 博弈论

#### Muti-Nim博弈

HDU-3032 Nim or not Nim? **Muti-Nim入门题**

HDU-2509 Be the Winner **Muti-Nim博弈 + Anti-Nim博弈**

#### Anti-Nim博弈

> **SJ定理：**
>
> 对于任意一个Anti-SG游戏，如果我们规定当局面中所有的单一游戏的SG值为0时，游戏结束。
>
> 先手必胜当且仅当：（1）游戏的SG函数不为0且游戏中某个单一游戏的SG函数大于1；（2）游戏的SG函数为0且游戏中没有单一游戏的SG函数大于1。



### 数论

#### 莫比乌斯反演

[P3327 [SDOI2015]约数个数和](https://www.luogu.com.cn/problem/P3327) 不错的反演练习题，不需要用到复杂的变换，但是较为繁琐。需要用到结论：
$$
d(ij) = \sum_{x|i}\sum_{y|j}[gcd(x,y)=1]
$$


## 数据结构

#### 线段树

[2018-2019 ICPC, NEERC, Northern Eurasia Finals K - King Kog's Reception](https://codeforces.com/contest/1089/problem/K)  **思维题+线段树** 线段树不错的一道题，**单点修改**。有一定的思维难度。



## 其他

### 思维题

HDU-6237 A Simple Stone Game **思维 + 质因数分解**





## 字符串

### Trie树

[P2580 于是他错误的点名开始了](https://www.luogu.com.cn/problem/P2580)   **Trie树模板题**

[P4551 最长异或路径](https://www.luogu.com.cn/problem/P4551)  **Trie树入门应用**  任选一点为根，建立根到所有节点的异或和的Trie树。假设$T(x,y)$为$x$节点到$y$节点路径上的权值异或和，那么我们有$T(x,y)=T(x,root)\oplus T(y,root)$，这是因为$T(lca,root)$被异或了两次，所以无了。根据Trie树，对于某一个数$a$，方便求其可能的最大异或和，自高位贪心即可。

[Codeforces Round #673 (Div. 2) E - XOR Inverse](https://codeforces.com/contest/1417/problem/E) **Trie的应用 + 逆序对**。详情见[CF673E](#CF673E)。

[TJOI2010\]阅读理解](https://www.luogu.com.cn/problem/P3879)  **Trie树模板题** 对每个单词节点统计行号即可，但是用$vector$会`WA`，不知道为什么（更新：**`WA`**）。用$bitset$标记出现的行号可过。(用$bool$标记会随机`MLE`)

### AC自动机

[P3808【模板】AC自动机（简单版）](https://www.luogu.com.cn/problem/P3808)  **AC自动机模板题**，求模式串出现次数。

[P3796【模板】AC自动机（加强版）](https://www.luogu.com.cn/problem/P3796)  **AC自动机模板题**，求模式串最大出现次数以及出现次数最大的模式串。

[P5357 【模板】AC自动机（二次加强版）](https://www.luogu.com.cn/problem/P5357)  **AC自动机模板题加强版**，名副其实的加强版。求每个模板串的出现次数。最后统计时需要用**拓扑排序**优化。

[P2414 [NOI2011]阿狸的打字机](https://www.luogu.com.cn/problem/P2414)  **<u>AC自动机好题</u>**，十分好的一道题，整合了`AC自动机`，`dfs序`，`区间求和`等方法，并且有一定思维难度。建议在练习一定程度后尝试，可以帮助大大加深对于`fail树`的理解。

#### AC自动机上DP

[P3041 [USACO12JAN]Video Game G ](https://www.luogu.com.cn/problem/P3041) **AC自动机入门题**，AC自动机模板+简单DP。

[P4052 [JSOI2007]文本生成器](https://www.luogu.com.cn/problem/P4052)  **AC自动机+DP**，比较套路。

**fail树的建立，引以为戒。**
（在这里谢过畅爷，畅爷yyds！）

```c++
inline void build(){
        queue<int> q;
        for (int i = 0; i < 26; ++i)
            if (tr[0][i]) q.push(tr[0][i]);
        while (!q.empty()){
            int u = q.front();
            // 建立fail树，可以
            e[fail[u]].push_back(u);
            q.pop();
            for (int i = 0; i < 26; ++i){
                int v = tr[u][i];
                if (v) {
                    fail[v] = tr[fail[u]][i];
//                    e[fail[v]].push_back(v); // 建立fail树，不行
                    q.push(v);
                }else tr[u][i] = tr[fail[u]][i];
            }
        }

//        // 建立fail树，可以
//        for (int i = 1; i <= cnt; ++i){
//            int u = fail[i];
//            e[u].push_back(i);
//        }
}
```





## CodeForces

### [Codeforces Round #673 (Div. 2)](https://codeforces.com/contest/1417)

[C - k-Amazing Numbers](https://codeforces.com/contest/1417/problem/C) **思维题**。对于一个数$i$，考虑其相邻两数的最大间隔$d$，显然，$i$**可能**成为$k-amazing\ number$当且仅当$k\ge d$。对$1-n$中每一个数都求出$d$（无论其有没有出现），然后从小开始更新答案即可。（假设$i$的$d$为$d_i$，$i-1$的$d$为$d_{i-1}$，显然当且仅当$k \in \{j|d_i\leq j \leq d_{i-1}\}$ 时，$k-amazing\ number$ 为 $i$）。

[D - Make Them Equal](https://codeforces.com/contest/1417/problem/D) **思维题**。先把$2-n$所有的数弄到$1$上，再分配下去。可以考虑先把$1$上的数分发给$2$使其凑成$2$的倍数，再把$2$的数全部给$1$，对$3,...,n$以此类推，可以证明这样一定是可行的。

[E - XOR Inverse](https://codeforces.com/contest/1417/problem/E)  <span id="CF673E">** Trie的应用 + 逆序对 **</span> 。 对所有的数建立$trie$，对每一个节点按顺序维护经过这个节点的数的索引。任意考虑一个节点。假设其左右儿子都存在。如果我们对其两条边异或$1$，则相当于将两个子节点的大小顺序反转。（即原来$0$边小于$1$边，现在$1$边小于$0$边）。因为维护了有序的索引值，所以方便快速计算逆序对。