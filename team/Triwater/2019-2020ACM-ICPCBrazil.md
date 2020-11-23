---
title: 2019-2020 ACM-ICPC Brazil Subregional Programming Contest 
description: 
published: true
date: 2020-11-23T04:21:33.007Z
tags: 
editor: markdown
dateCreated: 2020-11-23T04:21:33.007Z
---

## 2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)

#### 比赛情况



我们一共过了4道题，DGIJ

本场贡献：[et3_tsy](https://codeforces.com/profile/et3_tsy) ：D，J题提供关键思路，尝试写B，case过了，赛后发现就差简单排序，<del>差点过了</del>

​			[1427314831a](https://codeforces.com/profile/1427314831a)：过了D，G

​		    [Ryker0923](https://codeforces.com/profile/Ryker0923) ：过了I， 实现J，D提供关键思路

罚时：[1427314831a](https://codeforces.com/profile/1427314831a)产生了所有罚时，不过D的罚时比想象中少（特例确实多）

#### 比赛总结

---

[et3_tsy](https://codeforces.com/profile/et3_tsy) 

本场我认为考验了队伍构造特例的能力

比如说D题，我们看出了当一个字符出现了n-2次，另一个出现了2次，一定不行的情况，但是 $aabb$ 刚好就是一个特例

比如说B题，有一组样例（由 [wmxwmx](https://codeforces.ml/profile/wmxwmx) 提供）

```cpp
2 100 100
101 1 1 1
99 1 1 1
```

就比较坑爹了，导致几只队伍一直在WA5

---

[1427314831a](https://codeforces.com/profile/1427314831a)









---

[Ryker0923](https://codeforces.com/profile/Ryker0923) 









---



### 部分题解



#### B.DP

这道题目的意思就是说，一个人，他要升两个级别，等级1需要经验是S1，他需要升等级2的经验是S2。接下来给定N个探索的任务，然后每一个探索任务给定4个值ti，xi，ri和yi。ti，xi表示在等级1时该任务的时间花费和经验，ri，yi表示在等级2时该任务的时间花费和经验。 

它需要完成探索来升级，升级第一级的经验如果溢出，它会累积到第2个等级，每一个任务做完之后他就不能重复再做了，然后现在就要求他要满足这个升级之后最短的时间是多少。

这是经典的背包问题，但是这个题目的核心关键点就在于它第二轮与第一轮的物品状态不同了

那么我应该考虑的是，对于每一个物品，我拿它的时候当前状态是什么。

我们记 $st[fir][sec]$ 为当前状态，$fir$是指当前状态第一级经验值的多少，$sec$是指第二级经验值的多少

依据题目范围，$fir$ 至多为$s1+s2$,$sec$ 至多为 $500$

如果我现在是等级1，把它当等级2拿了，那么本方案就相当于是将拿的先后顺序交换了一下，依然合法。

但是如果我当前已经达到等级2了，就不能再拿等级1的东西，否则非法。

那么我们就可以产生一个很简单的状态转移过程了，线性枚举各个物品即可

但是

```
2 100 100
101 1 1 1
99 1 1 1
```

这组数据会WA

因为扫完101之后，我们没有办法再去要99了，因为我们上面也说它非法了。

这里的问题在于，在取101之前，99这一状态的缺失，两个物体是存在先后关系的

我们正确的处理应该是对其进行一个简单的排序，使较小的靠前，就不会缺少一些前继状态了

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define INF 0x3f3f3f3f3f3f3f3fLL
ll read()
{
    ll s=0,m=0;char ch=getchar();
    while(!isdigit(ch)){if(ch=='-')m=1;ch=getchar();}
    while(isdigit(ch))s=(s<<3)+(s<<1)+(ch^48),ch=getchar();
    return m?-s:s;
}
ll min(ll a,ll b)
{
    return a<b?a:b;
}
//ll
ll st[1005][505]; //st[fir][sec]
ll tmp[1005][505];
int x[1005],t[1005],y[1005],r[1005];
int n,s1,s2;
int main()
{
    ll ans=INF;
    n=read(),s1=read(),s2=read();
    memset(st,INF,sizeof(st));
    memset(tmp,INF,sizeof(tmp));
    st[0][0]=0;
    for(int i=1;i<=n;i++)
    {
        x[i]=read(),t[i]=read(),y[i]=read(),r[i]=read();
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            if(x[i]>x[j])
            {
                swap(x[i],x[j]);
                swap(t[i],t[j]);
                swap(y[i],y[j]);
                swap(r[i],r[j]);
            }
        }
    }
    for(int k=1;k<=n;k++)
    {
        for(int i=0;i<=(s1+s2);i++)
        {
            for(int j=0;j<=500;j++)
            {
                tmp[i][j]=min(tmp[i][j],st[i][j]);
                int fir=i+x[k];
                int sec=j;
                if(i<s1)
                {
                    if(fir>=s1&&fir+sec>=s1+s2)ans=min(ans,st[i][j]+t[k]);
                    if(fir<=s1+s2&&sec<=500)tmp[fir][sec]=min(tmp[fir][sec],st[i][j]+t[k]);
                }
                fir=i;
                sec=j+y[k];
                if(fir>=s1&&fir+sec>=s1+s2)ans=min(ans,st[i][j]+r[k]);
                if(fir<=s1+s2&&sec<=500)tmp[fir][sec]=min(tmp[fir][sec],st[i][j]+r[k]);
            }
        }
        for(int i=0;i<=(s1+s2);i++)
        {
            for(int j=0;j<=500;j++)
            {
                st[i][j]=tmp[i][j];tmp[i][j]=INF;
            }

            st[0][0]=0;
        }

    }
    if(ans==INF)cout<<-1<<endl;
    else cout<<ans<<endl;

    return 0;
}

```







#### D.

#### I.

#### J.图论

题意就是说给定一个无向完全图，图的点数是奇数，然后给定每一条边的权值。你现在要进行一个把它拆成若干个环的操作，每一个环上你现在要统计相邻两条边权值最大值的和，然后使得这些环的和加起来，求最小是多少。

其实我们要想到一个问题，既然图的点数是奇数，又是无向完全图，那么对于任意点而言，它必定一入一出，那么这条入边与出边在环上一定相邻，这也就产生了一个MAX值，那么我们对于每个点而言，只要把它的边两两配对，使得单点产生的MAX值最小即可。

而什么时候产生的MAX值最小嘞？对于单点，自小到大排序即可，每隔二取大者即可。代码奇短，很考察思维。

```cpp
#include<bits/stdc++.h>
using namespace std;
int ma[1010][1010];
void add(int x,int y,int z)
{
	ma[x][y]=ma[y][x]=z;
}
int n;
int f[1010];
long long sum;
int main()
{
	cin>>n;
	int x,y,z;
	for(int i=1;i<=n*(n-1)/2;i++)
	{
		scanf("%d%d%d",&x,&y,&z);
		add(x,y,z);
	}
	for(int i=1;i<=n;i++)
	ma[i][i]=0x3f3f3f3f;
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		f[j]=ma[i][j];
		sort(f+1,f+n+1);
		for(int j=2;j<=n;j+=2)
		sum+=1ll*f[j];
	}
	cout<<sum;
	return 0;
}
```







