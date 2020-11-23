---
title: 2019-2020 ACM-ICPC Brazil Subregional Programming Contest 
description: 
published: true
date: 2020-11-23T04:23:19.771Z
tags: 
editor: markdown
dateCreated: 2020-11-23T04:21:33.007Z
---

## 2019-2020 ACM-ICPC Brazil Subregional Programming Contest

#### 比赛情况

我们一共过了7道题，ABDGHLM，如果失误再少点，JK可做

本场贡献：[et3_tsy](https://codeforces.com/profile/et3_tsy) ：6min读完B，完成签到。四小时过了D。

​			[1427314831a](https://codeforces.com/profile/1427314831a)：1小时37过了中档题L。

​		    [Ryker0923](https://codeforces.com/profile/Ryker0923) ：16min完成H。56min过了M。三小时过了A题。四小时过了G。

一共七道题

罚时：[et3_tsy](https://codeforces.com/profile/et3_tsy) AD加起来差不多十几发， [Ryker0923](https://codeforces.com/profile/Ryker0923)  GM加起来差不多十几发

#### 比赛总结

---

[et3_tsy](https://codeforces.com/profile/et3_tsy) 

这场同时出现在现场的队伍比较多，而且势均力敌。不知道队友怎么感受的，但反正我是感觉有点压力，或者说，受到其他队伍影响比较大（会比较紧张，打乱了自己的节奏），所以以后心理抗压这方面要注意，以及不要过分关注榜单情况。

首先是队友M二分那边翻车了，不知道犯了一个什么神奇的问题，我也没有问，去想A了。我A过于自信，没有读样例三，就感觉是并查集，和队友确定了算法和题意后直接码。疯狂WA，后来才发现样例三有点问题，队友[Ryker0923](https://codeforces.com/profile/Ryker0923)改了挺久的过了。我得到[1427314831a](https://codeforces.com/profile/1427314831a)给我的D题题意后，一下子就想到了正解，等A改完后码。可能是我注意力不集中，树剖写的过程中忘记取了一个地方的max了，自造的小数据一点没问题，一直在WA。

这场比赛后，我读懂了J后，20min左右写了J，A掉，其实我想如果我们前面没有犯各种奇奇怪怪的错误，我们是有可能能做出9道题的（队友的K似乎也找到规律了）

我感觉我这场的A，犯了一个很大的问题，就是确认样例，样例给的多，再耗时间，再麻烦，**一定**要全部手动求解，以防**有些题目本身没有讲清楚的地方在样例中体现**

以及D题，对于数据结构题而言，当WA了两次（即改过一次后），还是WA的话，优先选择debug，而不是去构造数据，去输出一下看看每步应该存的东西对不对。如果某个数组到某处的值与理想不同，应该看看是不是什么函数写了忘记调用（如常忘的init()），以及一些取max，min的，还有全局局部变量的问题，以及对象要不要加引用去绑定（比如一些图论中的边可能要被标记，这个时候迭代器不只是一个clone,要能修改原对象）

---

[1427314831a](https://codeforces.com/profile/1427314831a)









---

[Ryker0923](https://codeforces.com/profile/Ryker0923) 









---



### 部分题解

#### BH.签到

#### A. 并查集

这题题意有点没讲清楚，反反复复在猜题意（实际上如果读通样例3就不会犯这样的错误）

```cpp
#include<bits/stdc++.h>
using namespace std;
int fa[1010];
int m,n,k;
long long r[1010][3];
bool u[1010][4],used[4];
int find(int k)
{
    if(fa[k]==k)return k;
    return fa[k]=find(fa[k]);
}
bool ins(int x,int y)
{
	long long rd=(r[x][2]+r[y][2])*(r[x][2]+r[y][2]);
	long long dis=(r[x][0]-r[y][0])*(r[x][0]-r[y][0])+(r[x][1]-r[y][1])*(r[x][1]-r[y][1]);
	if(rd>=dis)
	return 1;
	else
	return 0;
}
int main()
{
	cin>>m>>n>>k;
	for(int i=1;i<=k;i++)
	{
		cin>>r[i][0]>>r[i][1]>>r[i][2];
		fa[i]=i;
	}
	for(int i=1;i<=k;i++)
	for(int j=i+1;j<=k;j++)
	if(ins(i,j))
	{
		int x=find(i);
		int y=find(j);
		fa[x]=y;
	}
	for(int i=1;i<=k;i++)
	{
		if(r[i][0]-r[i][2]<=0)
		u[fa[i]][0]=1;
		if(r[i][0]+r[i][2]>=m)
		u[fa[i]][1]=1;
		if(r[i][1]-r[i][2]<=0)
		u[fa[i]][2]=1;
		if(r[i][1]+r[i][2]>=n)
		u[fa[i]][3]=1;
		if(u[fa[i]][0]==1&&u[fa[i]][2]==1)
		used[0]=1;
		if(u[fa[i]][0]==1&&u[fa[i]][3]==1)
		used[1]=1;
		if(u[fa[i]][1]==1&&u[fa[i]][2]==1)
		used[2]=1;
		if(u[fa[i]][1]==1&&u[fa[i]][3]==1)
		used[3]=1;
	}
//	for(int i=1;i<=k;i++)
//	cout<<u[i][0]<<" "<<u[i][1]<<" "<<u[i][2]<<" "<<u[i][3]<<" "<<endl;
	for(int i=1;i<=k;i++)
	{
		int sum=0;
//		for(int j=0;j<=3;j++)
//		sum+=u[i][j];
//		cout<<sum<<endl;
		if((u[i][0]==1&&u[i][1]==1)||(u[i][2]==1&&u[i][3]==1))
		{
			cout<<"N";
			return 0;
		}
//		if(sum>=2)
//		for(int j=0;j<=3;j++)
//		used[j]=used[j]?1:u[i][j];	
	}
//	for(int i=0;i<=3;i++)
//	cout<<used[i]<<" ";
//	cout<<endl;
//	if(used[0]==1&&used[1]==1)
//	cout<<"N";
//	else if(used[0]==1&&used[2]==1)
//	cout<<"N";
//	else if(used[1]==1&&used[3]==1)
//	cout<<"N";
//	else if(used[2]==1&&used[3]==1)
//	cout<<"N";
	if(used[0]==1||used[3]==1)
	cout<<"N";
	else
	cout<<"S";
	return 0;
}
```



#### D.长链剖分+堆

首先我们进行长链剖分，使得每步贪心最优，利用堆存储每个轻孩子，在堆中按照长度优先进行存储，暴力跑出k条即可。

```cpp
#include<bits/stdc++.h>
#include<ctime>
#define maxn 1000050
using namespace std;
struct e
{
    int depth;
    int index;
    e(int a,int b)
    {
        depth=a;
        index=b;
    }
    bool operator< (const e&sec)const
    {
        return depth<sec.depth;
    }
};

priority_queue<e>q;

int been[maxn];
int ans=0;

struct treenode
{
    int fa,hson,sz,depth,top,ds;
    vector<int>nxt;    
}t[maxn];  
int n,p,cur;
int dfs1(int pos)
{
    int ret=0;
    int ldis=0;
    t[pos].hson=-1;
    t[pos].sz=1;
    for(int k:t[pos].nxt)
    {
        if(!t[k].depth)
        {
            
            t[k].depth=t[pos].depth+1;
            t[k].fa=pos;
            int dis=dfs1(k);
            ret=max(dis,ret);
            if(t[pos].hson==-1||ldis<dis)
            {
                t[pos].hson=k;
                ldis=dis;
            }
        }
    }
    ret++;
    t[pos].ds=ret;
    return ret;
}

int main()
{
    cin>>n>>p;
    for(int i=2;i<=n;i++)
    {
        cin>>cur;
        t[cur].nxt.push_back(i);
        t[i].nxt.push_back(cur);
    }

    t[1].depth=1;
    dfs1(1);

    

    q.push(e(t[1].depth,1));
    while(q.size()&&p--)
    {
        int x=q.top().index;q.pop();
        int fa=x;
        ans++;
        been[x]=1;
        //cout<<x<<endl;
        while(t[x].hson!=-1)
        {
            //cout<<t[x].hson<<endl;
            for(int k:t[x].nxt)
            {
                if(k!=t[x].hson&&!been[k]&&fa!=k)q.push(e(t[k].ds,k));
            }
            fa=x;
            been[t[x].hson]=1;
            ans++;

            x=t[x].hson;
            
            //cout<<x<<endl;
        }
    }
    cout<<ans<<endl;
	return 0;
}
```

#### J.模拟

模拟题意即可，有可能开局天胡，注意细节。

```cpp
#include<iostream>
#include<cstring>
#include<cstdio>
#include<string>
using namespace std;
#define INF 0x3f3f3f3f
#define max(a,b) (a>b ? a:b)
#define min(a,b) (a<b ? a:b)
#define swap(a,b) (a^=b^=a^=b)
#define maxn 105
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
string s="A23456789DQJK",tt;
char a;
int n,k;
int ttt;
struct e
{
    int save[15];
    int out()
    {
        int curnum=10,curval=-1;
        for(int i=0;i<15;i++)
        {
            if(save[i]<curnum&&save[i])
            {
                curval=i;
                curnum=save[i];
            }
        }
        save[curval]--;
        return curval;
    }
    bool check()
    {
        bool mt=0;
        int cnt=0;
        for(int i=0;i<15;i++)
        {
            cnt+=save[i];
            if(save[i]==4)mt=1,ttt=i;
        }
        return cnt==4&&mt;
    }

}t[15];

int main()
{
    bool go=1;
    cin>>n>>k;
    for(int i=1;i<=n;i++)
    {
        cin>>tt;
        for(int j=0;j<4;j++)
        {
            t[i].save[s.find(tt[j])]++;
        }
        if(k!=i&&t[i].check())go=0;
    }
    int cur=k;
    bool ok=0;
    while(go)
    {
        int tmp=-1;
        if(cur==k)
        {
            if(ok==0)
            {
                ok=1;
                tmp=t[cur].out();
            }
            else
            {
                k++;
                if(k==n+1)k=1;
                ok=0;
                if(t[cur].check())go=0;
            }
        }
        else
        {
            tmp=t[cur].out();
            if(t[cur].check())go=0;
        }
        //cout<<tmp<<endl;

        cur++;
        if(cur==n+1)cur=1;
        if(tmp!=-1)t[cur].save[tmp]++;
        if(k!=cur&&t[cur].check())go=0;
    }


    int curans=0,miin=1000;
    for(int i=1;i<=n;i++)
    {
        if(k!=i&&t[i].check()&&miin>ttt)
        {
            miin=ttt;
            curans=i;
        }
    }
    cout<<curans<<endl;


    return 0;
}
/*
    2 1
    33J3
    JJJ3

    2 2
    A2A2
    22AA

    4 2
    774Q
    JJQ7
    44Q7
    4QJJ

    3 1
    JQAA
    JJJA
    QQQA
*/

```

#### I:Floyd

类似于洛谷的灾后重建，离线之后从温度最大和温度最小两个方向

按顺序加点跑两次floyd。

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m,cnt,cnt1,cnt2,q;
int ma1[510][510],ma2[510][510];
int tp[1010],l[1010];
struct query{
	int x,y,k,xl,ans;
}q1[100010],q2[100010];
void add(int x,int y,int z)
{
	ma1[x][y]=z;
	ma1[y][x]=z;
	ma2[x][y]=z;
	ma2[y][x]=z;
}
void floyd1(int k)
{
	for(int i=1;i<=n;i++)
	for(int j=1;j<=n;j++)
	ma1[i][j]=ma1[j][i]=min(ma1[i][j],ma1[i][k]+ma1[k][j]);
}
void floyd2(int k)
{
	for(int i=1;i<=n;i++)
	for(int j=1;j<=n;j++)
	ma2[i][j]=ma2[j][i]=min(ma2[i][j],ma2[i][k]+ma2[k][j]);
}
bool cmp1(query a,query b)
{
	return a.k<b.k;
}
bool cmp2(query a,query b)
{
	return a.xl<b.xl;
}
void lsh()
{
	for(int i=1;i<=n;i++)
	l[i]=tp[i];
	sort(l+1,l+n+1);
	cnt=unique(l+1,l+n+1)-l-1;
	for(int i=1;i<=n;i++)
	tp[i]=lower_bound(l+1,l+cnt+1,tp[i])-l;
}
int main()
{
	cin>>n>>m;
	int x,y,z,t;
	for(int i=1;i<=n;i++)
	scanf("%d",&tp[i]);
	lsh();
//	for(int i=1;i<=n;i++)
//	cout<<tp[i]<<" ";
//	cout<<"\n";
	for(int i=1;i<=n;i++)
	for(int j=1;j<=n;j++)
	if(i!=j)
	ma1[i][j]=ma2[i][j]=0x3f3f3f3f;
	for(int i=1;i<=m;i++)
	scanf("%d%d%d",&x,&y,&z),add(x,y,z);
	cin>>q;
	for(int i=1;i<=q;i++)
	{
		scanf("%d%d%d%d",&x,&y,&z,&t);
		if(t==0)
		q1[++cnt1].x=x,q1[cnt1].y=y,q1[cnt1].k=z,q1[cnt1].xl=i;
		else
		q2[++cnt2].x=x,q2[cnt2].y=y,q2[cnt2].k=z,q2[cnt2].xl=i;
	}
	sort(q1+1,q1+cnt1+1,cmp1);
	sort(q2+1,q2+cnt2+1,cmp1);
	int ltp=0,htp=cnt+1;
	for(int i=1;i<=cnt1;i++)
	{
		while(q1[i].k>ltp)
		{
			ltp++;
			for(int j=1;j<=n;j++)
			if(tp[j]==ltp)
			{
//				cout<<tp[j]<<" ";
//				cout<<"1:"<<" "<<i<<" "<<j<<endl;
				floyd1(j);
			}
		}
		q1[i].ans=ma1[q1[i].x][q1[i].y];
	}
	for(int i=1;i<=cnt2;i++)
	{
		while(cnt-q2[i].k+1<htp)
		{
			htp--;
			for(int j=1;j<=n;j++)
			if(tp[j]==htp)
			{
//				cout<<tp[j]<<" ";
//				cout<<"2:"<<" "<<i<<" "<<j<<endl;
				floyd2(j);
			}
			
		}
		q2[i].ans=ma2[q2[i].x][q2[i].y];
	}
	sort(q1+1,q1+cnt1+1,cmp2);
	sort(q2+1,q2+cnt2+1,cmp2);
	int t1=1,t2=1;
	for(int i=1;i<=q;i++)
	{
		if(q1[t1].xl==i)
		{
			printf("%d\n",q1[t1].ans==0x3f3f3f3f?-1:q1[t1].ans);
			t1++;
		}
		else
		{
			printf("%d\n",q2[t2].ans==0x3f3f3f3f?-1:q2[t2].ans);
			t2++;
		}
	}
	return 0;
}
```

#### G.二分图带权匹配 KM(或者费用流)

利用对数将乘法转为加法，之后就是KM板子题

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
using namespace std;
typedef long long ll;
const ll Maxn=505;
const ll inf=1e18;
ll n,m,map[Maxn][Maxn],matched[Maxn];
ll slack[Maxn],pre[Maxn],ex[Maxn],ey[Maxn];//ex,ey顶标
bool visx[Maxn],visy[Maxn];
void match(ll u)
{	
    ll x,y=0,yy=0,delta;
    memset(pre,0,sizeof(pre));
    for(ll i=1;i<=n;i++)slack[i]=inf;
    matched[y]=u;
    while(1)
    {	
        x=matched[y];delta=inf;visy[y]=1;
        for(ll i=1;i<=n;i++)
        {	
            if(visy[i])continue;
            if(slack[i]>ex[x]+ey[i]-map[x][i])
            {	
                slack[i]=ex[x]+ey[i]-map[x][i];
                pre[i]=y;
            }
            if(slack[i]<delta){delta=slack[i];yy=i;}
        }
        for(ll i=0;i<=n;i++)
        {	
            if(visy[i])ex[matched[i]]-=delta,ey[i]+=delta;
            else slack[i]-=delta;
        }
        y=yy;
        if(matched[y]==-1)break;
    }
    while(y){matched[y]=matched[pre[y]];y=pre[y];}
}
ll KM()
{	
    memset(matched,-1,sizeof(matched));
    memset(ex,0,sizeof(ex));
    memset(ey,0,sizeof(ey));
    for(ll i=1;i<=n;i++)
    {	
        memset(visy,0,sizeof(visy));
        match(i);
    }
    ll res=0;
    for(ll i=1;i<=n;i++)
        if(matched[i]!=-1)res+=map[matched[i]][i];
    return res;
}
int main()
{	
    scanf("%lld",&n);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
        	cin>>map[i][j];
        	map[i][j]=log(map[i][j])*10000;
		}   
    KM();
    for(int i=1;i<=n;i++)
    {
    	if(i!=1)
    	printf(" ");
		printf("%lld",matched[i]);
	}
//    printf("\n");
    return 0;
}
```

#### L.



```cpp

```





#### M.



```cpp

```

