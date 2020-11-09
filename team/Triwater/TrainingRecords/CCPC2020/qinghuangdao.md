---
title: 2020CCPC秦皇岛训练总结
description: 2020CCPC秦皇岛训练总结
published: true
date: 2020-11-04T10:53:26.405Z
tags: 
editor: undefined
dateCreated: 2020-10-27T13:24:18.222Z
---

## CCPC秦皇岛训练总结

### 比赛分析

我们队这场才过了3题，有点拉垮。

**A**签到。[Ryker0923](https://codeforces.com/profile/Ryker0923) 过的

**G**是 [1427314831a ](https://codeforces.com/profile/1427314831a)打模拟的之前自己做过，结果模拟赛写WA了，他自己看了一下他之前A的代码，改了细节，过了。

**F** [et3_tsy](https://codeforces.com/profile/et3_tsy)  看懂了题意，就有了思路，直接敲完。犯了各种奇奇怪怪的错误，贡献了一堆罚时。其中有一个是滥用memset，当T比较小，覆盖的n较大，暴力memset无所谓，但是n偏小，T较大，出题人一定会卡。

我们在E题上被卡傻了，[Ryker0923](https://codeforces.com/profile/Ryker0923) 的算法看起来很对，[1427314831a ](https://codeforces.com/profile/1427314831a) 写了个假数据生成，对了半个小时的假拍，回头补题才发现问题。

如果，E早就过了，我们可能可以过K，[Ryker0923](https://codeforces.com/profile/Ryker0923) 的**K题思路是对的，没有上机实践。**

比赛时rand一定一定一定一定要加种子，场上小于一小时过的多的题一定是类板子和思维题不能想复杂，少看榜看眼哪题能写就行了，看多了心态容易爆炸。

---

### 各题分析

#### E 尺取法

题意：*n*个学生，及格率*p*，每个学生有可能获得两个分数：最高分$a_i$和最低分$b_i$及格线为*n*个学生中的最高分*p*，问最好的情况下有多少的人能及格，$n<=10^{6}$

题解：经典的尺取法

本题的矛盾点在于，如果我们选取的数尽可能的大，那么对应的及格线也会被抬高。我们应该考虑的是如何考虑求得所有可行解，然后求max

---

**尺取法的基本模型：**

1）研究多个对象，每个对象有多个可行取值

2）每个对象取值的取大取小均影响答案

3）求最大最小值

**尺取法的基本处理思路**：

1）排序，排序对象维护两个值，本身的val，对应原来对象的下标

2）建立初态区间，使得这个区间能完整地使每个对象至少覆盖一次，记录其出现的次数vis

3）移动左右指针，每移动一次右指针，左指针尽可能向右移动，直到左指针再移动会不满足至少被覆盖一次的条件为止。

分析：

显然双指针的复杂度是线性的。而它左右区间以及vis的维护能够保证每个对象至少被访问了一次（如果vis了两次+，不影响）。而我们的排序以及维护区间尽可能的短能够保证最优性。

这种扫描方式能在不遗漏的枚举所有可行解。

---

很显然，本题就可以使用尺取法。

每个人存一个最大值，一个最小值，以及他们的id，然后排序。

这里的左指针值得注意，与常规的尺取法有点不同，他移动的依据是右指针对应的键值乘以p，然后统计区间中有多少个vis为零即可。

```cpp
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>
using namespace std;
#define INF 0x3f3f3f3f
#define max(a,b) (a>b ? a:b)
#define min(a,b) (a<b ? a:b)
#define swap(a,b) (a^=b^=a^=b)
#define maxn 3050000
#define minn -105
#define ll long long int
#define ull unsigned long long int
#define uint unsigned int
#define int long long
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
int _cse=0;
struct A
{
    int val;
    int id;
    void s(int a,int b){val=a;id=b;}
}q[maxn<<1];
int vis[maxn];
bool cmp(A a,A b)
{
    return a.val<b.val;
}
void solve()
{
    int n,p,a,b;
    cin>>n>>p;
    for(int i=0;i<n;i++)
    {
        a=read(),b=read();
        q[(i<<1)].s(a,i+1);
        q[(i<<1)+1].s(b,i+1);
        vis[i+1]=0;
    }
    sort(q,q+(n<<1),cmp);

    int ans=1,now=0,head=0,tail=-1;
    while(now!=n)
    {
        tail++;
        if(vis[q[tail].id]==0)now++;
        vis[q[tail].id]++;
    }
    vis[q[tail].id]--;
    now--;
    tail--;
    while(tail<(n<<1)-1)
    {
        tail++;
        if(vis[q[tail].id]==0)now++;
        vis[q[tail].id]++;
        while(1ll*q[head].val*100<1ll*q[tail].val*p)
        {
            if(vis[q[head].id]==1)now--;
            vis[q[head].id]--;head++;
        }
        ans=max(now,ans);
    }
    cout<<"Case #"<<(_cse+1)<<": "<<ans<<"\n";
}

signed main()
{
    int _t;
    _t=read();
    for(;_cse<_t;_cse++)solve();
    return 0;
}

```

刚好打完的周日**Codeforces Round #679 C** 中 再次 运用了**尺取法**

题意其实很简单，有六个值$A_i,0<=i<=5$, 现在给定你n个数，你需要对所有给定的$B_j$，从6六个A中选一个数出来，表达成任意一个A+$Cj$，n个C中，求最小的 $maxC-minC$

注意C要非负

思路：把所有可以表达的C以及他的id记录一下，然后进行尺取，保证区间能所有的id均出现过。

```cpp
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>
using namespace std;
#define INF 0x3f3f3f3f
#define max(a,b) (a>b ? a:b)
#define min(a,b) (a<b ? a:b)
#define swap(a,b) (a^=b^=a^=b)
#define maxn 1050000
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
struct line
{
    int val;
    int id;
}t[maxn*6];


bool cmp(line a,line b)
{
    return a.val<b.val;
}
int a[6];
int num[maxn];
int vis[maxn];
int n;
int tot=0;
int main()
{
    for(int i=0;i<6;i++)a[i]=read();
    sort(a,a+6);
    n=read();
    for(int i=0;i<n;i++)
    {
        num[i]=read();
        for(int j=0;j<6;j++)
        {
            if(a[j]<=num[i])
            {
                t[tot].val=num[i]-a[j];
                t[tot].id=i;
                tot++;
            }
            else break;
        }
    }
    sort(t,t+tot,cmp);
    int ans=INF;
    int all=0,tail=-1,head=0;
    while(all!=n)
    {
        tail++;
        if(vis[t[tail].id]==0)all++;
        vis[t[tail].id]++;
    }
    vis[t[tail].id]--;
    all--;
    tail--;
    while(tail<tot-1)
    {
        tail++;
        vis[t[tail].id]++;
        while(vis[t[head].id]>1)
        {
            vis[t[head].id]--;
            head++;
        }
        //cout<<tail<<" "<<head<<" "<<t[tail].val<<" "<<t[head].val<<endl;
        ans=min(ans,t[tail].val-t[head].val);
    }


    cout<<ans<<endl;
    return 0;
}

```



### F dfs 贪心

求各个联通块中点数边数。

如果边数大于点数，则累加边数减点数。

```cpp
#include<bits/stdc++.h>
#include<cstring>
using namespace std;
#define maxn 1000400
#define ll long long
int n,m,a,b;
struct E
{
	int to;
	bool vis;
	E(int a)
	{
		to=a;
		vis=0;
	}
	E(){}
};
vector<E>ls;
vector<int>node[maxn];
bool been[maxn];
int ans;
void init()
{
	ans=0;
	ls.clear();
	for(int i=0;i<=n;i++)node[i].clear(),been[i]=0;
}
int dfs(int pos,int& cntm)
{
	if(been[pos])return 0;
	int tot=1;
	been[pos]=1;
	for(int k:node[pos])
	{
		E& cur=ls[k];
		if(cur.vis)continue;
		cur.vis=1;ls[k^1].vis=1;cntm++;
		tot+=dfs(cur.to,cntm);
	}
	return tot;
}
void solve()
{
	init();
	cin>>n>>m;
	for(int i=0;i<m;i++)
	{
		scanf("%d%d",&a,&b);
		node[a].push_back(ls.size());
		ls.emplace_back(b);
		node[b].push_back(ls.size());
		ls.emplace_back(a);
	}
	for(int i=1;i<=n;i++)
	{
		int cntn=0,cntm=0;
		cntn=dfs(i,cntm);
		if(cntn<cntm)ans+=cntm-cntn;
		//cout<<cntm<<" "<<cntn<<endl;	
	}
	cout<<ans<<"\n";
}

int main()
{
	int _t;
	cin>>_t;
	for(int i=1;i<=_t;i++)
	{
		cout<<"Case #"<<i<<": ";
		solve();
	}
	return 0;
}
```

### I exgcd

两个任意方向的向量可以合成为一个在x方向上的向量+一个任意方向的向量，假设已有向量（x，y）和向量（d,0）,此时若加入一个新的向量（a,b）,对（x,y）和（a,b）消去y方向的分量得一个新的向量（d',0）（其中d'=(bx-ay)/gcd(y,b)）d'与d可够成在x轴上的最小分量gcd(d',d).当y与b合成在y方向上得最小分量gcd(y,b)时，可用exgcd求出y和b之前的系数，代入x与a中，求出新的x,若新的x>gcd(d',d)时，可取x%=gcd(d',d);每次维护更新x,y,d判断每次当前给定的点能否被访问到即可。注意x,y,d分别为0时的情况，容易出现/0、%0的情况。

```cpp
#include<bits/stdc++.h>
using namespace std;
long long exgcd(long long a,long long b,long long &x,long long &y)
{
	if(b==0)
	{
		x=1;y=0;return a;
	}
	long long ans=exgcd(b,a%b,x,y);
	long long tmp=x;
	x=y;
	y=tmp-a/b*y;
	return ans;
}
int main()
{
//	freopen("C:/Users/Lenovo/Desktop/std.in","r",stdin);
	int t;
	cin>>t;int cnt=0;long long a,b,a1,b1;
	while(t--)
	{
		cnt++;
		long long ans=0;
		int n;
		cin>>n;
		long long x=0,y=0,c=0,bj,k;
		while(n--)
		{			
			scanf("%lld",&bj);
			if(bj==1)
			{
			    scanf("%lld%lld",&a,&b);
			    if(a==0&&b==0)continue;
			    if(x==0&&y==0&&c==0)
			    {
			    	x=a;y=b;continue;
				}
			    if(a==0)
			    {
			    	c=exgcd(abs(a*y-b*x)/exgcd(b,y,a1,b1),c,a1,b1);
			    	y=exgcd(b,y,a1,b1);
			    	x=a1*a+b1*x;
			    	if(c)x=(x%c+c)%c;
			    	continue;
				}
				if(b==0)
				{
					c=exgcd(c,a,a1,b1);
					if(y==0)
					{
						c=exgcd(c,x,a1,b1);
						if(c)x=(x%c+c)%c;
					}
					continue;
				}
				if(!c)
				{
					if(b==0)
					{
						c=exgcd(a,c,a1,b1);
						continue;
					}
					if(a*y-b*x!=0)
					{
						c=abs(a*y-b*x)/exgcd(b,y,a1,b1);
						y=exgcd(b,y,a1,b1);
						x=a1*a+b1*x;
						if(c)x=(x%c+c)%c;
				    }
				    else
				    {
				    	y=exgcd(b,y,a1,b1);
					    x=a1*a+b1*x;
						if(c)x=(x%c+c)%c;
					}
				}
				else
				{
					if(b==0)
					{
						c=exgcd(a,c,a1,b1);
						continue;
					}
					if(a*y-b*x!=0)
					{
						c=exgcd(c,abs(a*y-b*x)/exgcd(b,y,a1,b1),a1,b1);
						y=exgcd(b,y,a1,b1);
						x=a1*a+b1*x;
						if(c)x=(x%c+c)%c;
				    }
				    else
				    {
				    	x=exgcd(x,a,a1,b1);
				    	y=exgcd(y,b,a1,b1);
					}
				}
		    }
		    else
		    {
		    	scanf("%lld%lld%lld",&a,&b,&k);
		        if(y==0)
		        {
		        	 if(x==0)
		        	 {
		        	 	if(c==0)
		        	 	{
		        	 		if(a==0&&b==0)ans+=k;
						}
		        	 	else
		        	 	{
		        	 		if(b==0&&(a%c==0))ans+=k;
						}
					 }
		        	 else
		        	 {
		        	 	if(c==0)
		        	 	{
		        	 		if(b==0&&(a%x==0))ans+=k;
						}
		        	 	else
		        	 	{
		        	 		if(b==0&&(a%c%x==0))ans+=k;
						}
					 }
				}
				else
				{
					if(x==0)
					{
						if(c==0)
						{
							if(b%y==0&&a==0)ans+=k;
						}
		        	 	else
		        	 	{
		        	 		if(b%y==0&&a%c==0)ans+=k;
						}
					}
		        	else
		        	{
		        	    if(c==0)
		        	    {
		        	    	if(b%y==0&&b*x==a*y)ans+=k;
						}
		        	 	else
		        	 	{
		        	 		if(b%y==0&&(b/y*x-a)%c==0)ans+=k;
						}
					}
				}
			}
		}
		printf("Case #%d: %lld\n",cnt,ans);
	} 
	return 0;
}
```

### K Kingdom's Power

有无数个军队可以从树根1出发，每花费一个点可以让军队走一步，问遍历完整个树的最小花费。

做法：树形DP加贪心，用vector和pair存儿子节点和深度，这样使用sort对儿子节点排序更方便。对儿子节点深度从小到大排序，依次从同一个子树深度小的叶子向深度大的叶子遍历，判断距离和直接从根到第二个叶子的距离大小，选取最小的那一个值，之后以此类推，从第二小向第三小走......最后计算总和即可（long long)。

时间复杂度：找深度和dp都是O(n),sort(nlogn(n较小))。

```cpp
#include<bits/stdc++.h>
using namespace std;
int val[1000010];
int fa[1000010],tot;
vector<pair<int,int> > son[1000010];
inline int read()
{
    int s=0,m=0;char ch=getchar();
    while(!isdigit(ch)){if(ch=='-')m=1;ch=getchar();}
    while( isdigit(ch))s=(s<<3)+(s<<1)+(ch^48),ch=getchar();
    return m?-s:s;
}
int getdeep(int x)
{
	if(son[x].empty())
	return 1;
	for(int i=0;i<son[x].size();i++)
	son[x][i].first=getdeep(son[x][i].second);
	sort(son[x].begin(),son[x].end());
	return son[x].back().first+1;
}
int dfs(int x,int d,int v)
{
	val[x]=v;
	if(son[x].empty())
	return 1;
	int t=v;
	for(int i=0;i<son[x].size();i++)
	t=min(d,dfs(son[x][i].second,d+1,t+1));
	return t+1;
}
int main()
{
    int t,n;
    cin>>t;
    while(t--)
    {
    	tot++;
    	cin>>n;
    	for(int i=1;i<=n;i++)
    	{
    		son[i].clear();
    		val[i]=0;
		}

    	for(int i=2;i<=n;i++)
    	{
    		fa[i]=read();
    		son[fa[i]].push_back({0,i});
		}
		getdeep(1);
		dfs(1,0,0);
		long long ans=0;
		for(int i=1;i<=n;i++)
		if(son[i].empty())
		{
			ans+=val[i];
		}
		
		printf("Case #%d: %lld\n",tot,ans);
	}
    return 0;
}

```

### B.线段树

这道题也不难，就是很难写，而且很耗时间，有大模拟的味道。就是给定一个矩形，矩形上有干湿两种地面。两种操作，一是翻转干湿，二是查询经过特定点，围成的最大围墙包括的面积是多少，围墙只能建在干地上。

这题平心而论，真的不难，但是很考研码量功底，在5hrs的时间中，如果码力不够是不可能将本题写完的（我赛后补题前前后后快花了差不多4小时）

核心思想就是暴力枚举所有可行的情况，记录每个点对应的上下左右最远可以达到多少，查询的时候往四个方向上建线段树（因为高度是一定的，但是对应每个高度的情况是不同的，所以要暴力枚举）

这题细节很多，x与y，i与j，n与m，都很容易弄混，以及几个边界，什么时候是有效的，以及一些降低常数的细节也要仔细。

```cpp
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 1305
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
int _t,n,m,q,xx,yy,trans;
string s;
bool a[maxn][maxn];
int up[maxn][maxn],down[maxn][maxn],lft[maxn][maxn],rgt[maxn][maxn];
int va[maxn];

struct segment
{
    int val;
}seg[maxn<<2];
void seg_built(int pos,int l,int r)
{
    if(l==r)
    {
        seg[pos].val=va[l];
        return;
    }
    int mid=(l+r)>>1;
    seg_built(pos<<1,l,mid);
    seg_built(pos<<1|1,mid+1,r);
    seg[pos].val=max(seg[pos<<1].val,seg[pos<<1|1].val);
}
int query_l(int pos,int l,int r,int cl,int cr,int mn)
{
    if(l>cr||r<cl||cl>cr)return INF;
    if(l==r)
    {
        return l;
    }
    int curans=INF;
    if(seg[pos<<1].val>=mn)curans=query_l(pos<<1,l,(l+r)>>1,cl,cr,mn);
    if(curans==INF&&seg[pos<<1|1].val>=mn)curans=query_l(pos<<1|1,((l+r)>>1)+1,r,cl,cr,mn);
    return curans;
}
int query_r(int pos,int l,int r,int cl,int cr,int mn)
{
    if(l>cr||r<cl||cl>cr)return INF;
    if(l==r)
    {
        return l;
    }
    int curans=INF;
    if(seg[pos<<1|1].val>=mn)curans=query_r(pos<<1|1,((l+r)>>1)+1,r,cl,cr,mn);
    if(curans==INF&&seg[pos<<1].val>=mn)curans=query_r(pos<<1,l,(l+r)>>1,cl,cr,mn);
    return curans;
}

void init()
{
    for(int i=1;i<=m;i++)down[n+1][i]=0;
    for(int i=1;i<=n;i++)rgt[i][m+1]=0;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
            if(a[i][j])up[i][j]=up[i-1][j]+1;
            else up[i][j]=0;
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
            if(a[n-i+1][j])down[n-i+1][j]=down[n-i+2][j]+1;
            else down[n-i+1][j]=0;
    }
    for(int j=1;j<=m;j++)
    {
        for(int i=1;i<=n;i++)
            if(a[i][j])lft[i][j]=lft[i][j-1]+1;
            else lft[i][j]=0;
    }
    for(int j=1;j<=m;j++)
    {
        for(int i=1;i<=n;i++)
            if(a[i][m-j+1])rgt[i][m-j+1]=rgt[i][m-j+2]+1;
            else rgt[i][m-j+1]=0;
    }
    //cout<<"debug: pos13:    "<<rgt[1][3]<<endl;
}
void renew(int x,int y)
{
    a[x][y]^=1;

    int cur=up[x-1][y]+1;
    if(!a[x][y])cur=-cur;
    up[x][y]+=cur;
    for(int i=x+1;i<=n;i++)
    {
        if(up[i][y])up[i][y]+=cur;
        else break;
    }

    cur=down[x+1][y]+1;
    if(!a[x][y])cur=-cur;
    down[x][y]+=cur;
    for(int i=x-1;i;i--)
    {
        if(down[i][y])down[i][y]+=cur;
        else break;
    }

    cur=lft[x][y-1]+1;
    if(!a[x][y])cur=-cur;
    lft[x][y]+=cur;
    for(int i=y+1;i<=m;i++)
    {
        if(lft[x][i])lft[x][i]+=cur;
        else break;
    }

    cur=rgt[x][y+1]+1;
    if(!a[x][y])cur=-cur;
    rgt[x][y]+=cur;
    for(int i=y-1;i;i--)
    {
        if(rgt[x][i])rgt[x][i]+=cur;
        else break;
    }
}
int query_max(int x,int y)
{
    if(!a[x][y])return 0;
    int maxans=max(up[x][y]+down[x][y]-1,lft[x][y]+rgt[x][y]-1);
    //cout<<maxans<<" "<<lft[x][y]<<" "<<rgt[x][y]<<endl;
    int l=y-lft[x][y]+1,r=y+rgt[x][y]-1; //chosen as the downward edge
    for(int i=l;i<=r;i++)va[i]=up[x][i];
    seg_built(1,l,r);
    for(int h=2;h<=seg[1].val;h++)
    {
        if(!a[x-h+1][y])continue;
        int curl=max(l,y-lft[x-h+1][y]+1),curr=min(r,y+rgt[x-h+1][y]-1);
        if(curl>curr)continue;
        int qr=query_r(1,l,r,curl,curr,h),ql=query_l(1,l,r,curl,curr,h);
        if(qr!=INF&&ql!=INF&&ql<=y&&qr>=y)maxans=max(maxans,h*(qr-ql+1));
    }

    l=y-lft[x][y]+1,r=y+rgt[x][y]-1; //chosen as the upward edge
    for(int i=l;i<=r;i++)va[i]=down[x][i];
    seg_built(1,l,r);
    for(int h=2;h<=seg[1].val;h++)
    {
        if(!a[x+h-1][y])continue;
        int curl=max(l,y-lft[x+h-1][y]+1),curr=min(r,y+rgt[x+h-1][y]-1);
        if(curl>curr)continue;
        int qr=query_r(1,l,r,curl,curr,h),ql=query_l(1,l,r,curl,curr,h);
        if(qr!=INF&&ql!=INF&&ql<=y&&qr>=y)maxans=max(maxans,h*(qr-ql+1));
    }

    l=x-up[x][y]+1,r=x+down[x][y]-1; //chosen as the left edge
    for(int i=l;i<=r;i++)va[i]=rgt[i][y];
    seg_built(1,l,r);
    for(int h=2;h<=seg[1].val;h++)
    {
        if(!a[x][y+h-1])continue;
        int curl=max(l,x-up[x][y+h-1]+1),curr=min(r,x+down[x][y+h-1]-1);
        if(curl>curr)continue;
        int qr=query_r(1,l,r,curl,curr,h),ql=query_l(1,l,r,curl,curr,h);
        if(qr!=INF&&ql!=INF&&ql<=x&&qr>=x)maxans=max(maxans,h*(qr-ql+1));
    }

    l=x-up[x][y]+1,r=x+down[x][y]-1; //chosen as the right edge
    for(int i=l;i<=r;i++)va[i]=lft[i][y];
    seg_built(1,l,r);
    for(int h=2;h<=seg[1].val;h++)
    {
        if(!a[x][y-h+1])continue;
        int curl=max(l,x-up[x][y-h+1]+1),curr=min(r,x+down[x][y-h+1]-1);
        if(curl>curr)continue;
        int qr=query_r(1,l,r,curl,curr,h),ql=query_l(1,l,r,curl,curr,h);
        if(qr!=INF&&ql!=INF&&ql<=x&&qr>=x)maxans=max(maxans,h*(qr-ql+1));
    }

    return maxans;
}

void solve()
{
    n=read(),m=read(),q=read();
    for(int i=1;i<=n;i++)
    {
        cin>>s;
        for(int j=1;j<=m;j++)
        {
            if(s[j-1]=='#')a[i][j]=1;
            else a[i][j]=0;
        }
    }
    init();
    for(int i=1;i<=q;i++)
    {
        trans=read(),xx=read(),yy=read();
        if(trans==1)renew(xx,yy);
        else cout<<query_max(xx,yy)<<"\n";
    }
}

int main()
{
    _t=read();
    for(int i=1;i<=_t;i++)
    {
        cout<<"Case #"<<i<<":\n";
        solve();
    }
    return 0;
}


```

