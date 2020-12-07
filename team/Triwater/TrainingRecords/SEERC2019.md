---
title: 2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)
description: 
published: true
date: 2020-12-07T11:29:08.730Z
tags: 
editor: markdown
dateCreated: 2020-12-07T11:29:08.730Z
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

以及博弈题卡掉一些假算法需要一些针对性比较强的数据

---

[1427314831a](https://codeforces.com/profile/1427314831a)

注意线段树开四倍空间

使用*之前一定要注意相乘的两个数会不会爆int



---

[Ryker0923](https://codeforces.com/profile/Ryker0923) 

需要注意到一些特殊条件

图论题给出了n为奇数的条件，一开始没有注意到

注意到之后五分钟就A了

同时要注意猜结论，证明要花费很多时间

猜结论很可能就过了

---



### 部分题解



#### B.背包DP

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







#### D.分类讨论

```cpp
#include<bits/stdc++.h>
using namespace std;
string s; 
int a[1000];
int main()
{
	cin>>s;
	int n=s.size();
	if(n==2)
	{
		if(s[0]==s[1])cout<<"NO";
		else 
		{
			cout<<"YES\n";
			cout<<s;
		}
		return 0;
	}
	for(int i=0;i<s.size();i++)
	a[s[i]-'a']++;
	int bj=-1,maxn=0;
	for(int i=0;i<=25;i++)
	{
		if(a[i]>maxn)
		{
			bj=i,maxn=a[i];
		}
	}
	if(n==4)
	{
		if(a[bj]>=3)cout<<"NO";
		else
		{
			cout<<"YES\n";
			for(int i=0;i<=25;i++)
			{
				while(a[i])
				{
					cout<<char(i+'a');a[i]--;
				}
			}
		}
		return 0;
	}
	if(a[bj]>=n-1)
	{
		cout<<"NO\n";
		return 0;
	}
	if(a[bj]==n-2)
	{
		for(int i=0;i<=25;i++)
		{
			if(a[i]==2&&i!=bj)
		    {
		    	cout<<"NO";
		    	return 0;
			}
		}
		cout<<"YES\n";
		for(int i=1;i<=n/2;i++)
		cout<<char(bj+'a');
		int bj1;
		for(int i=0;i<=25;i++)
		if(a[i]==1)bj1=i;
		for(int i=0;i<=25;i++)
		{
			if(a[i]==1)
			{
				cout<<char('a'+i);
			break;}
		}
		for(int i=1;i<=n/2-2;i++)
		cout<<char('a'+bj);
		cout<<char('a'+bj1);
		return 0;
	}
	if(a[bj]>n/2)
	{
		cout<<"YES\n";
		for(int i=1;i<=n/2;i++)
		cout<<char('a'+bj);
		for(int i=0;i<=25;i++)
		{
			if(a[i]!=0&&i!=bj)
			{
				cout<<char('a'+i);
				a[i]--;
				break;
			}
		}
		for(int i=1;i<=a[bj]-n/2;i++)
		cout<<char('a'+bj);
		for(int i=0;i<=25;i++)
		{
			while(a[i]&&i!=bj)
			{
				cout<<char('a'+i);a[i]--;
			}
		}
		return 0;
	}
	if(a[bj]<=n/2)
	{
		cout<<"YES\n";
		for(int i=0;i<=25;i++)
		{
			while(a[i])
			{
				cout<<char('a'+i);a[i]--;
			}
		}
	}
	return 0;
}
/*
4
2 14 7 14
5 10 9 22
*/
```

#### I.猜结论

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,a[10010],b[10010],c[10010];
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
	cin>>a[i];
	for(int i=1;i<=n;i++)
	cin>>b[i];
	for(int i=1;i<=n;i++)
	c[i]=0x3f3f3f3f;
	sort(a+1,a+n+1);
	sort(b+1,b+n+1);
	for(int i=1;i<=n;i++)
	for(int j=1;j<=n;j++)
	c[i]=min(c[i],abs(a[i]-b[j]));
	int maxn=0;
	for(int i=1;i<=n;i++)
	maxn=max(maxn,c[i]);
	cout<<maxn;
	return 0;
 }
```



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

#### F.博弈

树上博弈

题意：给定一个有根树，根为1，两人每轮操作，第一次操作选择树上一个点，放置棋子，接下来每次操作，选择没被棋子放置过的且是当前棋子所在格子的孩子或者祖宗的点，把棋子放置到该位置。

 **思路**：

**首先先提供一种错误思路**(这是[et3_tsy](https://codeforces.com/profile/et3_tsy)的假思路)：

 我们假设对于一个树而言，考虑它所有子树的必胜必败态。

1）如果所有子树均是必败态，那么先手可以选择父节点，那么对于后手而言，他一定会进入的其中一个子树并且没有办法再到其他子树中，故对于先手而言，这是必胜态。

2）如果所有子树里至少有两个必胜态，先手可以选择其中一个必胜的子树，那么后手无论如何一定会走到父节点，那么先手再选另一个必胜子树。





对应代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define max(a,b) (a>b ? a:b)
#define min(a,b) (a<b ? a:b)
#define swap(a,b) (a^=b^=a^=b)
#define maxn 105000
#define minn -105
#define ll long long int
#define ull unsigned long long int
#define uint unsigned int
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
int n;
vector<int>node[maxn];

bool dfs(int cur,int fa)
{
    int cntgood=0,cntbad=0,cnt=0;
    bool ok=0;
    for(int k:node[cur])
    {
        if(fa==k)continue;
        cnt++;
        if(dfs(k,cur))cntgood++;
        else cntbad++;

    }
    if(cntbad==cnt)ok=1;
    if(cntgood>=2)ok=1;
    if(cnt==0)ok=1;
    //if(cur==2)cout<<ok<<endl;
    return ok;
}

int main()
{
    cin>>n;
    for(int i=0;i<n-1;i++)
    {
        int a,b;
        cin>>a>>b;
        node[a].push_back(b);
        node[b].push_back(a);
    }
    if(dfs(1,1))cout<<"Alice\n";
    else cout<<"Bob\n";
    return 0;
}

```



以上代码看似很对，但是下面这组就是错的

感谢隔壁大神提供一组样例（又由 [wmxwmx](https://codeforces.ml/profile/wmxwmx) 提供）

```cpp
7
1 2
2 3
3 4
4 5
4 6
4 7
```

很显然在博弈逻辑里面，这种子树递进的逻辑是错误的。也就是说，博弈过程会反复横跳，考虑层级关系在本题不适用。

正解思路：

我们应该考虑将树上的点转化成图，如果两点相互可达则建立一条无向边。

那么这就是一般图最大匹配问题了。

如果最大匹配是完美匹配，那么一定能够使得两两配对，则后手一定存在一种应对方案，后手必胜否则先手必胜。

那么一般图在这道题中会T，其实只要在树上进行Dp即可，具体实现可看代码。

```cpp
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define max(a,b) (a>b ? a:b)
#define min(a,b) (a<b ? a:b)
#define swap(a,b) (a^=b^=a^=b)
#define maxn 105000
#define minn -105
#define ll long long int
#define ull unsigned long long int
#define uint unsigned int
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
int n;
vector<int>node[maxn];

int dfs(int cur,int fa)
{
    int lft=0;
    for(int k:node[cur])
    {
        if(fa==k)continue;
        lft+=dfs(k,cur);
    }
    if(lft)lft--;
    else lft++;
    return lft;
}

int main()
{
    cin>>n;
    for(int i=0;i<n-1;i++)
    {
        int a,b;
        cin>>a>>b;
        node[a].push_back(b);
        node[b].push_back(a);
    }
    if(dfs(1,1))cout<<"Alice\n";
    else cout<<"Bob\n";
    return 0;
}

```

#### E.双指针

有n个人去旅游，可以选择开车或者骑摩的，一辆车带m个人（含司机），要求司机年龄大于lc，租金pc，

一辆摩的只能带一个人（司机自己），要求司机年龄大于lm，租金pm

每次花t块钱，把一个人的年龄续给另一个人。

要求1.年龄不能小于1，2.对每个人最多用d次（包括给的人和接的人）

现在告诉你所有人的年龄ai，问你最少花费把所有人都带走，如果做不到输出-1

 **思路**：

通过双指针来模拟开车和骑车人的数量，其中一项确定后另一项就也确定了

在模拟的同时记录最小值

如果不能满足情况就输出-1

```cpp
#include<bits/stdc++.h>
using namespace std;
long long n,k,lm,lc,pm,pc,t,d;
long long nd,hv,co;
long long a[100010]; 
int main()
{
	cin>>n>>k>>lc>>pc>>lm>>pm>>t>>d;
	for(int i=1;i<=n;i++)
	cin>>a[i];
	sort(a+1,a+n+1);
	long long ans=0x3f3f3f3f3f3f3f3f;
	int cnt=0;
	for(int i=1;i<=n;i++)
	{
        hv+=max(0ll,min(1ll*d,a[i]-lm));
        nd+=max(0ll,min(1ll*d,lm-a[i]));
        if(a[i]+d<lm)
		cnt++;
    }
    co=n*pm+nd*t;
    if(!cnt&&hv>=nd)
    ans=co;
    int l=1,r=n;
    int tot=0;
//    cout<<"1:"<<nd<<" "<<hv<<endl;
    while(r-l+1>=k)
    {
//		cout<<"1:"<<nd<<" "<<hv<<endl;
    	tot++;
    	for(int i=0;i<k-1;i++)
    	{
    		nd-=max(0ll,min(1ll*d,lm-a[i+l]));
    		hv+=min(1ll*d,min(1ll*lm,a[i+l])-1);
    		if(a[i+l]+d<lm)
    		cnt--;
		}
		nd+=max(0ll,lc-max(1ll*lm,a[r]));
		hv-=max(0ll,min(1ll*d,a[r]-lm));
		hv+=max(0ll,min(1ll*d,a[r]-lc));
		if(a[r]+d<lc)
		break;
		co=pm*(n-tot*k)+tot*pc+nd*t;
		if(!cnt&&hv>=nd)
    	ans=min(co,ans);
    	l+=k-1;
    	r--;
//    	cout<<"2:"<<nd<<" "<<hv<<endl;
	}
	if(a[r]+d>=lc)
	{
//		cout<<"1:"<<nd<<" "<<hv<<endl;
		tot++;
		for(int i=0;i<r-l;i++)
		{
			nd-=max(0ll,min(1ll*d,lm-a[i+l]));
			hv+=min(1ll*d,min(1ll*lm,a[i+l])-1);
			if(a[i+l]+d<lm)
			cnt--;
		}
		nd+=max(0ll,lc-max(1ll*lm,a[r]));
		hv-=max(0ll,min(1ll*d,a[r]-lm));
		hv+=max(0ll,min(1ll*d,a[r]-lc));
		co=tot*pc+nd*t;
		if(!cnt&&hv>=nd)
    	ans=min(co,ans);
//    	cout<<"2:"<<nd<<" "<<hv<<endl;
	}
	if(ans==0x3f3f3f3f3f3f3f3f)
	cout<<-1;
	else
	cout<<ans;
	return 0;
}
```

#### A.线段树维护单点修改，区间合并

有n个数字排列成一个环，每次可选择一个数字和它相邻的两个数字，将这个数字变为三个中最大的或最小的。

问对所有的i∈[1,m]。最小的将所有的数字变为i的操作数，若不存在这样的方案数，输出-1.

1<=n,m<=2e5

思路：

对于一个选定的i，我们计所有大于i的数为1，小于i的数为-1，等于i的记为0，则

[0,1,1]直接取最小

[0,1,-1]先取最小，再取最大

[0,-1,-1]直接取最大

[0,-1,1]先取最大，再取最小

可以看到，-1与1的连续导致操作数增加

-1 1时增加一次

-1 1 -1时可先改中间，总操作数增加一次

-1 1 -1 1时增加两次

-1 1 -1 1 -1时可改2 4位置的，总操作数增加两次

由此可见一个长度为m的-1 1串会增加总操作数m/2次

开二倍数组使之成为一个环

维护从一个0开始长度为n的区间中-1 1串的贡献加上所有非零数的个数即可

对每个区间记录从左开始的-1 1长度与从右开始的

合并两个区间时若从左开始的长度等于区间长度，则将右边的左长度也加入合成区间的左长度，否则只继承左边的左长度，右边也做相同的处理

计算贡献时若左边的右端点与右边的左端点异号，则需更新在此处的贡献为(左边的右长度+右边的左长度)/2

```
#include<bits/stdc++.h>
#define int long long
using namespace std;
struct tnode{
	int to,next;
}e[800080];
struct tree{
	int l,r,ans,l1,r1;
}t[1600080];//开始时习惯性的看题目2e5的范围开了8e5的数组，忘了自己为了让数组成环已经扩大到了4e5的范围，线段树应该开到4e5的四倍
int head[800080],tot,a[800080],hash1[800020];
void add(int l,int r)
{
	e[++tot].to=r;
	e[tot].next=head[l];
	head[l]=tot;
}
void build(int i,int l,int r)
{
	if(l==r)
	{
		t[i].l=1;
		t[i].r=1;
		t[i].l1=l;
		t[i].r1=r;
		t[i].ans=0;
		return;
	}
	int mid=(l+r)>>1;
	build(i<<1,l,mid);
	build(i<<1|1,mid+1,r);
	t[i].l=1;
	t[i].r=1;
	t[i].l1=l;
	t[i].r1=r;
	t[i].ans=0;
}
int now;
void update(int i,int zhi)
{
	if(t[i].l1>zhi||t[i].r1<zhi)return;
	if(t[i].l1==t[i].r1)
	{
		if(a[t[i].l1]==now)
		{
			t[i].l=0;
			t[i].r=0;
			t[i].ans=0;
		}
		else
		{
			t[i].l=1;
			t[i].r=1;
			t[i].ans=0;
		}
		return;
	}
	update(i<<1,zhi);
	update(i<<1|1,zhi);
	if(t[i<<1].l==t[i<<1].r1-t[i<<1].l1+1&&(a[t[i<<1].r1]-now)*(a[t[i<<1|1].l1]-now)<0)
	{
		t[i].l=t[i<<1].l+t[i<<1|1].l;
	}
	else t[i].l=t[i<<1].l;
	if(t[i<<1|1].r==t[i<<1|1].r1-t[i<<1|1].l1+1&&(a[t[i<<1|1].l1]-now)*(a[t[i<<1].r1]-now)<0)
	{
		t[i].r=t[i<<1].r+t[i<<1|1].r;
	}
	else t[i].r=t[i<<1|1].r;
	t[i].ans=t[i<<1].ans+t[i<<1|1].ans;
	if((a[t[i<<1].r1]-now)*(a[t[i<<1|1].l1]-now)<0)//开始时想偷懒直接用乘积来判断两个区间的端点异号，没注意到会爆int
	t[i].ans+=(t[i<<1].r+t[i<<1|1].l)/2-t[i<<1].r/2-t[i<<1|1].l/2;
}
int sum(int i,int l,int r)
{
	if(t[i].l1>r||t[i].r1<l)return 0;
	if(t[i].l1>=l&&t[i].r1<=r)return t[i].ans;
	int mid=(t[i].l1+t[i].r1)>>1;
	int res=0;
	res+=sum(i<<1,l,r);
	res+=sum(i<<1|1,l,r);
	int l1,r1;
	if(l<=mid&&mid+1<=r)
	{
		l1=min(t[i<<1].r,mid-max(l,t[i].l1)+1);
		r1=min(t[i<<1|1].l,min(r,t[i].r1)-mid);
		if((a[t[i<<1].r1]-now)*(a[t[i<<1|1].l1]-now)<0)
		{
		res+=(l1+r1)/2-l1/2-r1/2;
//		cout<<"error"<<min(r,t[i].r)-mid<<" "<<r1<<"\n";
	    }
	}
	return res;
}
void print(int n)
{
	for(int i=1;i<=2*n;i++)
	cout<<t[i].l1<<" "<<t[i].r1<<" "<<t[i].ans<<" "<<t[i].l<<" "<<t[i].r<<"\n";
}
signed main()
{
//	freopen("std.in","r",stdin);
//	freopen("std.out","w",stdout);
	int n,m;
	cin>>n>>m;
	for(int i=1;i<=n;i++)
	{
		scanf("%d",&a[i]);
		a[i+n]=a[i];
		add(a[i],i);
		hash1[a[i]]++;
	}
	build(1,1,n<<1);
	for(int i=1;i<=m;i++)
	{
		now=i;
		for(int j=head[i-1];j;j=e[j].next)
			{
				update(1,e[j].to);
				update(1,e[j].to+n);
			}
		if(hash1[i]==0)
		{
			printf("-1 ");
		}
		else
		{
			for(int j=head[i];j;j=e[j].next)
			{
				update(1,e[j].to);
				update(1,e[j].to+n);
			}
//			printf("%d %d\n",i,sum(1,e[head[i]].to,e[head[i]].to+n-1));
			printf("%d ",n-hash1[i]+sum(1,e[head[i]].to,e[head[i]].to+n-1));
		}
//		cout<<i<<"\n";
//		print(n<<2);
//		cout<<"\n";
//        cout<<i<<" "<<e[head[i]].to<<" "<<e[head[i]].to+n-1<<"\n";
//        cout<<sum(1,1,5)<<"\n";
	}
    return 0;
}
```



