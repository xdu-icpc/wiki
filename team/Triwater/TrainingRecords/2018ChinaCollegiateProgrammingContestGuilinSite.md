---
title: 2018 China Collegiate Programming Contest - Guilin Site 
description: 
published: true
date: 2021-01-02T07:14:48.481Z
tags: 
editor: markdown
dateCreated: 2021-01-02T07:14:48.481Z
---

## 2018 China Collegiate Programming Contest - Guilin Site 

#### 比赛情况

我们一共过了道题，，

本场贡献：[et3_tsy](https://codeforces.com/profile/et3_tsy) ：D、H、J

​			[1427314831a](https://codeforces.com/profile/1427314831a)：G

​		    [Ryker0923](https://codeforces.com/profile/Ryker0923) ：提供了H的对拍，和有.问题的代码

罚时：一共18发，其中17由 [et3_tsy](https://codeforces.com/profile/et3_tsy) 贡献

#### 比赛总结

---

[et3_tsy](https://codeforces.com/profile/et3_tsy) 

我啪一下就有想法了，啪一下样例就过了，啪一下WA2，~~这好吗？这不好~~

这场我感觉思路还是清晰的，正解想法出来的很快，但是实现就实现了个蛇皮

D题WA1发，是因为cnt2写成cnt3还是啥来着

J题写了蛇皮，跟队友说这从右往左扫，然后就写了个`for(int i=0;i<n-1;i++)   `，就很离谱

H题就更离谱了，初始化骗了自己骗了队友，就是忘了写else啥

```cpp
for(int i=len-1;i>=0;i--)
{
    if(s1[i]!=s2[i])dif[i]=1;
    dif[i]+=dif[i+1];
}
```

然后我们队数据生成就一直一组数据，对拍半天，反而暴力跑出问题了

更离谱的是，只有我们队一直在T5，而不是WA5

反正，这间接证明了~~我日常口胡，压榨工具人队友码代码的事实~~

下次实现不要着急，减少不应该的错误，否则得不偿失（~~好像CCPC绵阳的老毛病我又犯了~~）

---

[1427314831a](https://codeforces.com/profile/1427314831a)









---

[Ryker0923](https://codeforces.com/profile/Ryker0923) 









---



### 部分题解

#### DHJ 思维

就是思维题

#### A.结论题

##### 题意

给定两个序列，分别要将这两个序列里面的正整数给它插到一个新的序列里面，然后统计这个序列的贡献，使得这个序列的贡献最小，这个序列的贡献怎么统计呢？就是每个数的大小乘以这个数在这个序列的位置，求各数的贡献之和。并且原序列的每两个数在新序列里面的相对位置不能变。

##### 题解

首先这道题的贪心策略肯定是越把大的放前越好，麻烦就在于相对位置不能变。

这道题的核心结论就是对原来的两个序列，按平均值大小进行分块，使得每个块之间出现非递增关系，然后在两个序列间，按照块平均值越大的，越先贪心。

为什么这样嘞？我们先考虑块大小为1的情况，即块中只有一个数

```
假设当前序列分别为
ab
k
```

假设a<b,那么我们要么选择kab摆放，要么选择abk摆放，一定不会是akb

如果我们选择abk摆放，那么一定有k+2a+3b>a+2b+3k  => a+b>2k =>b>k

所以显然abk会优于akb

那么，这就有个结论，只要是后者更大，我们直接选择取后面的元素。我们进一步可以将之推广，按照块的平均值来处理问题。

```cpp
#include<bits/stdc++.h>
using namespace std;
#define maxn 1050000
#define ll long long
#define int ll
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0'&&ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
struct block
{
    int cnt,val;
    void operator+=(block B)
    {
        cnt+=B.cnt;
        val+=B.val;
    }
    bool operator<(block B)
    {
        return 1ll*val*B.cnt<1ll*B.val*cnt;
    }
};

int n,m,cnt1,cnt2,cur1,cur2,pos1,pos2,pos;
int a[maxn],b[maxn],c[maxn<<1];
block aa[maxn],bb[maxn];

void solve()
{
    n=read(),m=read();cnt1=cnt2=cur1=cur2=pos1=pos2=pos=0;
    for(int i=0;i<n;i++)
    {
        a[i]=read();
    }
    for(int i=0;i<m;i++)
    {
        b[i]=read();
    }
    for(int i=0;i<n;i++)
    {
        aa[cnt1++]=block{1,a[i]};
        while(cnt1>1&&aa[cnt1-2]<aa[cnt1-1])
        {
            aa[cnt1-2]+=aa[cnt1-1];cnt1--;
        }
    }
    for(int i=0;i<m;i++)
    {
        bb[cnt2++]=block{1,b[i]};
        while(cnt2>1&&bb[cnt2-2]<bb[cnt2-1])
        {
            bb[cnt2-2]+=bb[cnt2-1];cnt2--;
        }
    }
    while(cur1<cnt1&&cur2<cnt2)
    {
        if(bb[cur2]<aa[cur1])
        {
            for(int i=0;i<aa[cur1].cnt;i++)
            {
                c[pos+i]=a[pos1+i];
            }
            pos+=aa[cur1].cnt,pos1+=aa[cur1].cnt,cur1++;
        }
        else
        {
            for(int i=0;i<bb[cur2].cnt;i++)
            {
                c[pos+i]=b[pos2+i];
            }
            pos+=bb[cur2].cnt,pos2+=bb[cur2].cnt,cur2++;
        }
    }
    while(cur1<cnt1)
    {
        for(int i=0;i<aa[cur1].cnt;i++)
        {
            c[pos+i]=a[pos1+i];
        }
        pos+=aa[cur1].cnt,pos1+=aa[cur1].cnt,cur1++;
    }
    while(cur2<cnt2)
    {
        for(int i=0;i<bb[cur2].cnt;i++)
        {
            c[pos+i]=b[pos2+i];
        }
        pos+=bb[cur2].cnt,pos2+=bb[cur2].cnt,cur2++;
    }
    ll ans=0;
    for(int i=0;i<n+m;i++)
    {
        ans+=1ll*(i+1)*c[i];
    }
    cout<<ans<<"\n";
}
signed main()
{
    int _t;
    _t=read();
    for(int i=1;i<=_t;i++)cout<<"Case "<<i<<": ",solve();
    return 0;
}
/*
2
2 2
5 3
4 5
3 3
1 3 5
2 6 4
1
2 2
5 3
4 5
*/

```

