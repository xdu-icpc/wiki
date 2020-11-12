---
title: 2019-2020 XX Open Cup, Grand Prix of Korea 
description: 
published: true
date: 2020-11-12T15:16:51.447Z
tags: 
editor: markdown
dateCreated: 2020-11-12T15:16:51.447Z
---

# 2019-2020 XX Open Cup, Grand Prix of Korea  

### 比赛收获

本场贡献：[et3_tsy](https://codeforces.com/profile/et3_tsy)  ：G提供了思路，过了H，H爆了int，贡献WA一发，J有想法，调了1hr后，发现在特例情况复杂度退化，假了

[1427314831a](https://codeforces.com/profile/1427314831a)：过了A

[Ryker0923](https://codeforces.com/profile/Ryker0923) ：过了G，提供了A的思路

一共三道题

#### 总结：本场暴露两个问题：

1）后期乏力

前两个小时过了三题，接下来，FIJ都可以做，但是都没有比较好的想法，说明题量不够，还需要时间沉淀。

2）特例导致复杂度退化

算法在实现之前，一定要和队友多交流，多尝试几种特例，一方面看算法错没错，一方面还要看复杂度会不会退化。像[et3_tsy](https://codeforces.com/profile/et3_tsy) 的J题，在处理的时候，看似是树dp，但是随着层数的增多，常数会膨胀，退化成N方，假了。



### 部分题解

#### A. 模拟

简单模拟即可，注意很多细节上的问题，以及不要重复计数

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[1000][1000],b[1000][1000],n,m; 
int cmp(int x,int y)
{
	if(x==6&&y==6)return 1;
	if(x==6&&y==9)return 1;
	if(x==7&&y==7)return 1;
	if(x==8&&y==8)return 1;
	if(x==9&&y==9)return 1;
	if(x==9&&y==6)return 1;
	return 0;
}
int main()
{
	cin>>n>>m;
	char c=getchar();
	for(int i=1;i<=n;i++)
	{
	for(int j=1;j<=m;j++)
	{
	    a[i][j]=getchar()-'0';
	}
	getchar();
    }
	for(int i=1;i<=n;i++)
	for(int j=1;j<=m;j++)
	{
		b[n-i+1][m-j+1]=a[i][j];
	}
	for(int i=1;i<=n;i++)
	for(int j=1;j<=m;j++)
	{
		if(b[i][j]==6)b[i][j]=9;
		else if(b[i][j]==9)b[i][j]=6;
	}
	int bj=0;
	for(int i=1;i<=n;i++)
	for(int j=1;j<=m;j++)
	{
		if(!cmp(a[i][j],b[i][j]))
		{
			bj=1;break;
		}
	}
	if(!bj)
	{
		if(n%2==1&&m%2==1&&a[n/2+1][m/2+1]!=8)
        bj=1;
	}
	if(bj){
		cout<<-1;return 0;
	}
	int ans=0;
	for(int i=1;i<=n;i++)
	for(int j=1;j<=m;j++)
	{
		if(a[i][j]!=b[i][j])ans++;
		else if(a[i][j]==7)ans++;
	}
	cout<<ans/2;
	return 0;
}
```

#### G. 图论

注意这道题是让我们从可行解里面选取最小字典序的路径，那么换言之，我们就要优先dfs去查询哪个点是能到达目标点的。

值得注意的是，因为在求向图可达性中，可能形成强连通，所以直接进行两次连续的dfs，防止漏掉点没有被标记的可达点

最后是进行对可行点形成的单向路径上进行贪心模拟的过程。至于什么时候输出too long？只要访问了已经访问过的点，就说明它在已有的一个强连通分量里面出不来了，故输出too long即可。

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,s,t,cnt,tot;
int l[200010],rd[2000010];
struct node{
	int x,y,z;
}ed[600010];
bool cmp(node a,node b)
{
	return a.z>b.z;
}
struct tedge{
	int v,next,w;
	bool u;
}e[1000010];
bool b[200010],used[200010];
void add(int x,int y,int z)
{
	e[++cnt].w=y;
	e[cnt].v=z;
	e[cnt].next=l[x];
	l[x]=cnt;
}
bool dfs(int x)
{
	if(used[x])
	return b[x];
	used[x]=1;
	for(int i=l[x];i;i=e[i].next)
	{
//		cout<<x<<" "<<e[i].w<<endl;
		if(b[x]==1)
		dfs(e[i].w);
		else
		b[x]=dfs(e[i].w);
	}
	return b[x];
}
void print()
{
	for(int i=1;i<=tot;i++)
	printf("%d ",rd[i]);
}
void dfs2(int x)
{
	used[x]=1;
	for(int i=l[x];i;i=e[i].next)
	if(b[e[i].w])
	{
		rd[++tot]=e[i].v;
		if(tot>1000000)
		{
			printf("TOO LONG");
			exit(0);
		}
		if(e[i].w==t)
		{
			print();
			exit(0);
		}
		else if(used[e[i].w])
		{
			printf("TOO LONG");
			exit(0);
		}
		else
		dfs2(e[i].w);
	}
}
int main()
{
	cin>>n>>m>>s>>t;
	int x,y,z;
	b[t]=1;
	for(int i=1;i<=m;i++)
	{
		scanf("%d%d%d",&ed[i].x,&ed[i].y,&ed[i].z);
	}
	sort(ed+1,ed+m+1,cmp);
	for(int i=1;i<=m;i++)
	{
//		cout<<ed[i].x<<" "<<ed[i].y<<" "<<ed[i].z<<endl;
		add(ed[i].x,ed[i].y,ed[i].z);
	}
	dfs(s);
	for(int i=1;i<=n;i++)used[i]=0;
//	for(int i=1;i<=n;i++)cout<<b[i]<<endl;
	dfs(s);
	if(!b[s])
	{
		printf("IMPOSSIBLE");
		return 0;
	}
//	for(int i=1;i<=n;i++)cout<<b[i]<<endl;
	for(int i=1;i<=n;i++)used[i]=0;
	dfs2(s);
}
/*
3 3 1 3
1 2 1
2 1 2
1 3 3
*/
```

#### H. 思维

给定两个全排列，全排列A可动，B不能动，求最小交换次数，使得Ai-Bi的绝对值之和最大。

应该先从偶数上思考，即什么时候最优，即把 1到 N/2 分为第一组，把 N/2+1 到 N分为第二组

那么我们只要保证对于任意一对Ai和Bi恰好一个属于第一组，一个属于第二组即可，显然这样最优。

而奇数大同小异，唯有不同的是最中间的那个数，它不能分到第一组和第二组中去。

接下来就是双指针贪心扫描了，分别第二组元素为第一组、第二组的情况，取min即可，注意会爆int

```cpp
#include<bits/stdc++.h>
using namespace std;
#define maxn 300000
#define ll long long
#define INF 0x3f3f3f3f3f3f3f3fLL
int a[maxn],b[maxn];
ll minn(ll x,ll y)
{
	return x<y?x:y;
}
int n;
double mid;
bool l(int val)
{
	return val<mid;
}
bool r(int val)
{
	return val>mid;
}
int main()
{
	cin>>n;
	mid=(0.0+n)/2+0.5;
	for(int i=1;i<=n;i++)cin>>a[i];
	for(int i=1;i<=n;i++)cin>>b[i];
	ll ans=INF;
	ll cur=0;
	int cnt=0;
	int apos=0,bpos=0;
	cur=0;
	while(cnt!=n/2)
	{
		while(!l(b[++bpos]));
		while(!r(a[++apos]));
		cur+=abs(apos-bpos);
		cnt++;
	}
	ans=minn(ans,cur);
	cur=0;cnt=0;apos=0,bpos=0;
	while(cnt!=n/2)
	{
		while(!r(b[++bpos]));
		while(!l(a[++apos]));
		cur+=abs(apos-bpos);
		cnt++;
	}
	ans=minn(ans,cur);
	cout<<ans<<"\n";
	return 0;
}
```

### J.启发式合并

[](https://codeforces.ml/group/2l2uaz0vCx/contest/102391/problem/J)

大致题意（经过转化之后的）

给定n个区段[l, r],  以及对应的权值，注意他们不交叉

现在就是求  k (从1->n分别求) 层，至多叠k层的最大权值和为多少

**思路**

显然，我们可以发现他们不交叉只有两种情况，一种是完全包含，一种是完全不相交

这就有一个树的特征了，建树先排个序，左端第一维，区段长第二维，再线性扫一遍。

我们把树建好后，很显然可以在树上去算dp，但是会被下图的特例卡退化成$O(n^2)$

那么我们应该怎么想这道题嘞？

其实有个很核心的性质

**如果我们在第k层进行贪心选择的时候，k+1层也一定会选上这些**

这一点一定要理解好

那么我们理解了这个之后，只要利用堆，去存储单层最优的区段贡献即可（对于兄弟之间而言，他们不会产生影响，所以取出两边最优的相加，就是父亲的最优）

而这一过程可以利用启发式合并的思想进行优化处理

对于本题， [et3_tsy](https://codeforces.com/profile/et3_tsy)  有一定的收获：

对于启发式合并而言，并不一定要依赖于树剖，它只是一种思想，就是把小的往大的上面去合并。在进行树dp的时候，一定优先考虑退化问题（比如这题可能会退化成如下情况）。


![https://s1.ax1x.com/2020/11/06/Bf3PgK.png](https://s1.ax1x.com/2020/11/06/Bf3PgK.png)



```cpp
#include<bits/stdc++.h>
using namespace std;
#define maxn 252000
#define ll long long
int n,tot;
ll tmp[maxn];
int ma[maxn],in[maxn],fa[maxn];
priority_queue<ll>heap[maxn];
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
void merge_(int x)
{
    int f=fa[x],fh=ma[f],gh=ma[x];
    if(heap[fh].size()<heap[gh].size())
    {
        ma[f]=gh;
        swap(fh,gh);
    }
    int cnt=0;
    while(!heap[gh].empty())
    {
        tmp[cnt++]=heap[gh].top()+heap[fh].top();
        heap[gh].pop(),heap[fh].pop();
    }
    for(int i=0;i<cnt;i++)heap[fh].push(tmp[i]);
}
struct ed
{
	int l,r;
	ll val;
	bool operator<(const ed& sec)const
	{
		if(l!=sec.l)return l<sec.l;
		return r>sec.r;
	}
}e[maxn];
inline bool check(int l,int r,int curl,int curr)
{
	return l<=curl&&r>=curr;
}
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		e[i].l=read(),e[i].r=read(),e[i].val=read();
		e[i].r--;
	}
	sort(e+1,e+n+1);
	int curindex=0;
	for(int i=1;i<=n;i++)
	{
		while(curindex&&!check(e[curindex].l,e[curindex].r,e[i].l,e[i].r))
		{
			curindex=fa[curindex];
		}
		fa[i]=curindex;
		curindex=i;
	}
	queue<int>node;
	for(int i=1;i<=n;i++)
    {
        ma[i]=i;
        in[fa[i]]++;
    }
    for(int i=1;i<=n;i++)
        if(in[i]==0)node.push(i);
    while(!node.empty())
    {
        int x=node.front();
        node.pop();
        if(x==0)break;
        heap[ma[x]].push(e[x].val);
        merge_(x);
        in[fa[x]]--;
        if(in[fa[x]]==0)node.push(fa[x]);
    }
	ll ans=0;
	for(int i=1;i<=n;i++)
	{
		if(!heap[ma[0]].empty())
		{
		    ans+=heap[ma[0]].top();
		    heap[ma[0]].pop();
		}
		cout<<ans<<" ";
	}

	return 0;
}

```

