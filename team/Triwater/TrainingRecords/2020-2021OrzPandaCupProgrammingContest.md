---
title: 2020-2021 “Orz Panda” Cup Programming Contest 
description: 
published: true
date: 2020-12-14T08:44:19.897Z
tags: 
editor: markdown
dateCreated: 2020-12-14T08:44:19.897Z
---

## 2020-2021 “Orz Panda” Cup Programming Contest 

#### 比赛情况

我们一共过了道3题

本场贡献：[et3_tsy](https://codeforces.com/profile/et3_tsy) ：过了A，提供了H的关键修改

​			[1427314831a](https://codeforces.com/profile/1427314831a)：尝试写E，可惜没过

​		    [Ryker0923](https://codeforces.com/profile/Ryker0923) ：过了H，I

罚时：H，I各罚了一次

#### 比赛总结

---

[et3_tsy](https://codeforces.com/profile/et3_tsy) 

这场比赛是比较考验基本功的，像CG，需要的知识储备很低，但是场上没有写，说明还是有点问题的。

---

[1427314831a](https://codeforces.com/profile/1427314831a)

多注意一些容易导致死循环的细节。

---

[Ryker0923](https://codeforces.com/profile/Ryker0923) 

注意计算几何精度问题。

---



### 部分题解

#### A.带权并查集+优先队列

其他做法（像单调栈）可能更好

#### I.计算几何

因为除2后只会出现0.5和0.0两种情况，double会出现精度损失，所以注意使用long long来存。

#### H.思维题

#### C.set的使用

这道题就是说给定一个空序列，往里面放、拿东西。有两种维护，第1种维护，就是说往里面插入一个数，使得他往已经有的东西最远，一样远那么取序号小的，第2种维护，就是取出第K次是操作放入的东西。

这道题核心就是开两个set。第1个set去储存，我们当前哪些区段放是最优的。第2个set去储存，我们当前哪一些地方已经放置了东西。

其实这道题处理起来不难，就是考察set使用的基本功，然而我疯狂在T，原因如下：

```
auto tmp=pos.lower_bound(x);
auto tmp=lower_bound(pos.begin(),pos.end(),x);
```

第一行比第二行快很多，因为它是容器自带的方法，下次使用要注意

```cpp
#include<bits/stdc++.h>
using namespace std;
#define maxn 200050
#define int long long
int n,m,trans,x,mid,lf,y;
struct seg
{
    int l,r,dis;
    bool operator<(const seg& sec)const
    {
        if(dis==sec.dis)return l<sec.l;
        return dis>sec.dis;
    }
    seg(int a,int b,int c){l=a;r=b;dis=c;}
    seg(){}
};
int op[maxn];
set<seg>s;
set<int>pos;
set<seg>::iterator it;
set<int>::iterator it2;
seg t,t2;
inline void add(int l,int r)
{
    if(l>r)return;
    if(l==1||r==n)s.insert(seg{l,r,r-l});
    else s.insert(seg{l,r,(r-l)>>1});
}
void solve()
{
    s.clear();
    pos.clear();
    pos.insert(0);
    pos.insert(n+1);
    add(1,n);
    for(int i=1;i<=m;i++)
    {
        scanf("%d",&trans);
        if(trans==1)
        {
            t=*s.begin();
            if(!s.empty())s.erase(s.begin());
            if(t.l==1||t.r==n)
            {
                if(t.l==1)
                {
                    printf("1\n");
                    op[i]=1;
                    pos.insert(1);
                    add(2,t.r);
                }
                else
                {
                    printf("%d\n",n);
                    op[i]=n;
                    pos.insert(n);
                    add(t.l,n-1);
                }
            }
            else
            {
                mid=(0ll+t.l+t.r)>>1;
                printf("%d\n",mid);
                op[i]=mid;
                pos.insert(mid);
                add(t.l,mid-1);
                add(mid+1,t.r);
            }
        }
        else
        {
            scanf("%d",&x);
            x=op[x];
            int l=x,r=x;
            auto tmp=pos.lower_bound(x);
            int pre=*--tmp;++tmp;
            int nxt=*++tmp;
            if(pre+2<=x)
            {
                l=pre+1;
                if(pre!=0)s.erase(seg{pre+1,x-1,(x-pre-2)>>1});
                else s.erase(seg{pre+1,x-1,x-2});
            }
            if(x+2<=nxt)
            {
                r=nxt-1;
                if(nxt!=n+1)s.erase(seg{x+1,nxt-1,(nxt-2-x)>>1});
                else s.erase(seg{x+1,nxt-1,nxt-2-x});
            }
            pos.erase(x);
            add(l,r);
        }
    }
}

signed main()
{
    while(scanf("%d%d",&n,&m)!=EOF)solve();
    return 0;
}


```

#### G.LCA

这题其实不难，就是很烦人

一颗结点数量为N的数，美丽度的定义，对于任意的两个点，取遍所有N个节点，分别取他们作为根，求这两个节点在不同的根下，分别到他们lca的距离乘积的总和

首先要想到真正有贡献的节点，实际上是两点连成路径上的节点以及这些点对应的侧分支。

然后再去进一步推导公式，比较麻烦的是，要继续拆开，把它拆成很多子项去进行计算。利用树上前缀和去高效求值。

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXN 250003
#define ll long long
#define int ll
const ll mod=998244353;
inline int read()
{
    int ans=0;
    char last=' ',ch=getchar();
    while(ch<'0'|ch>'9')last=ch,ch=getchar();
    while(ch>='0' && ch<='9')ans=ans*10+ch-'0',ch=getchar();
    if(last=='-')ans=-ans;
    return ans;
}
struct edge
{
    int r,to;
};
edge e[MAXN];
int head[MAXN],d[MAXN],lg[MAXN],s[MAXN];
ll Si[MAXN],Sfa[MAXN],SfaDfa[MAXN],SiDfa[MAXN],SiDfa2[MAXN],SfaDfa2[MAXN];
int fa[MAXN][22];
int total=0,n,m,root,a,b;
ll q;
void add(int x,int y)
{
    e[++total].r=y;
    e[total].to=head[x];
    head[x]=total;
}
void dfs(int cur,int fapos)
{
    fa[cur][0]=fapos;
    d[cur]=d[fapos]+1;
    for(int i=1;i<=lg[d[cur]];i++)
    {
        fa[cur][i]=fa[fa[cur][i-1]][i-1];
    }
    s[cur]=1;
    for(int i=head[cur];i;i=e[i].to)
    {
        if(e[i].r!=fapos)
        {
            dfs(e[i].r,cur);
            s[cur]+=s[e[i].r];
        }
    }
}
void dfs2(int cur,int fapos)
{
    Si[cur]=s[cur];
    if(cur!=1)
    {
        Si[cur]+=Si[fa[cur][0]];
        Sfa[cur]=s[fa[cur][0]];Sfa[cur]+=Sfa[fa[cur][0]];
        SfaDfa[cur]=s[fa[cur][0]]*d[fa[cur][0]];SfaDfa[cur]+=SfaDfa[fa[cur][0]];
        SiDfa[cur]=s[cur]*d[fa[cur][0]];SiDfa[cur]+=SiDfa[fa[cur][0]];
        SiDfa2[cur]=s[cur]*d[fa[cur][0]]*d[fa[cur][0]]%mod;SiDfa2[cur]+=SiDfa2[fa[cur][0]];
        SfaDfa2[cur]=s[fa[cur][0]]*d[fa[cur][0]]*d[fa[cur][0]]%mod;SfaDfa2[cur]+=SfaDfa2[fa[cur][0]];
    }
    for(int i=head[cur];i;i=e[i].to)
    {
        if(e[i].r!=fapos)
            dfs2(e[i].r,cur);
    }
}
int afa,bfa;
int LCA(int a,int b)
{
    if(d[a]<d[b])swap(a,b);
    if(d[a]!=d[b])
    {
        while(d[a]>d[b]+1)
        a=fa[a][lg[d[a]-d[b]-1]-1];
    }
    if(fa[a][0]==b)
    {
        afa=bfa=a;
        return b;
    }
    if(d[a]!=d[b])a=fa[a][0];
    for(int k=lg[d[a]-1];k>=0;k--)
        if(fa[b][k]!=fa[a][k])a=fa[a][k],b=fa[b][k];
    afa=a,bfa=b;
    return fa[b][0];
}
ll prefix(int a,int b)
{
    ll ans=0;
    ans+=(Sfa[a]-Sfa[b])*d[a]*(q-d[a])%mod;
    ans+=(SfaDfa[a]-SfaDfa[b])*d[a]%mod;
    ans-=(SfaDfa[a]-SfaDfa[b])*(q-d[a])%mod;
    ans-=(SfaDfa2[a]-SfaDfa2[b])%mod;
    ans-=(Si[a]-Si[b])*d[a]*(q-d[a])%mod;
    ans+=(SiDfa[a]-SiDfa[b])*(q-d[a])%mod;
    ans-=(SiDfa[a]-SiDfa[b])*d[a]%mod;
    ans+=(SiDfa2[a]-SiDfa2[b])%mod;
    return (ans+1ll*100*mod)%mod;
}
ll query(int a,int b)
{
    ll ans=0;
    int c=LCA(a,b);
    if(afa==bfa)
    {
        if(a==c)swap(a,b);
        q=d[a]-d[c];
        //cout<<a<<" "<<afa<<endl;
        ans=prefix(a,afa);
    }
    else
    {
        ans+=1ll*(n-s[afa]-s[bfa])*(d[a]-d[c])%mod*(d[b]-d[c])%mod;
        //cout<<ans<<endl;
        //cout<<afa<<" "<<bfa<<endl;
        q=d[a]-d[c]+d[b]-d[c];
        ans+=prefix(a,afa);
        ans+=prefix(b,bfa);
    }
    return (ans+1ll*100*mod)%mod;
}

signed main()
{
    n=read(),m=read();
    root=1;
    for(int i=0;i<n-1;i++)
    {
        a=read(),b=read();
        add(a,b);
        add(b,a);
    }
    for(int i=1;i<=n;i++)
        lg[i]=lg[i-1]+(1<<lg[i-1]==i);
    dfs(root,1);
    dfs2(root,1);
    for(int i=1;i<=m;i++)
    {
        a=read(),b=read();
        if(a==b)cout<<"0\n";
        else cout<<query(a,b)<<"\n";
    }

    /*
    cout<<endl;
    for(int i=1;i<=n;i++)
    {
        cout<<i<<" "<<Si[i]<<endl;
    }
    */
    return 0;
}
/*
5 1
1 2
1 3
3 4
3 5
1 5

4 5
2 5
*/

```

#### E.FFT

按照规律打表后发现了一个循环节
$$
2^{log_2 n}
$$
于是最多的操作数不超过131072次
$$
每个数的大小不超过2^{17}，考虑按位将其拆开，再分别处理
$$
每次异或操作相当于一次不进位的加法，相当于要对每一位做多次前缀和，最后对2取模

每一位的多次前缀和用FFT维护，最终时间复杂度为常数很大的nlogn

现场原本以为是常数太大导致T了，后来发现自己有个while(k%2==0)的循环，当k一开始为0时就会死循环

```cpp
#include<bits/stdc++.h>
using namespace std;
const int mod=998244353;
int g=3,a1[300010],b[300010],rev[300010],C[300010];
int a[100010],n;
long long m;
int ksm(int a,int b)
{
	int res=1;
	while(b)
	{
		if(b&1)res=1ll*res*a%mod;
		a=1ll*a*a%mod;
		b>>=1;
	}
	return res;
}
void ntt(int *a,int n,int f)
{
	for(int i=0;i<n;i++)if(i<rev[i])swap(a[i],a[rev[i]]);
	for(int i=1;i<n;i<<=1)
	{
		int gn=ksm(g,(mod-1)/(i<<1));if(f==-1)gn=ksm(gn,mod-2);
		for(int j=0;j<n;j+=i<<1)
		{
			int gqn=1;
			for(int k=j;k<i+j;k++)
			{
				int x=a[k],y=1ll*gqn*a[k+i]%mod;
				a[k]=1ll*(x+y)%mod;
				a[k+i]=1ll*(x-y+mod)%mod;
				gqn=gqn*1ll*gn%mod;
			}
		}
	}
	if(f==-1)
	{
		int ny=ksm(n,mod-2);
		for(int i=0;i<n;i++)a[i]=a[i]*1ll*ny%mod;
	}
}
inline int read()
{
	int res=0;
	char c=getchar();
	while(c<'0'||c>'9')c=getchar();
	while(c>='0'&&c<='9')
	{
		res=(res<<1)+(res<<3)+c-'0';
		c=getchar();
	}
	return res;
}
int ans[20][300010],sum[300010];
int main()
{
//	freopen("std.in","r",stdin);
	int k=1;
	cin>>n>>m;
	while(n>=k)k<<=1;
	m%=k;
	long long l=1,r=m;
	int bj=0;
	k=m;
	for(int i=1;i<=n;i++)
	a[i]=read();
	if(k==0)
	{
		for(int i=1;i<=n;i++)
		cout<<a[i]<<" ";
		return 0;
	}
	while(k%2==0&&k!=0)
	{
		k>>=1;bj++;
	}
	C[0]=1;C[1]=m;
	C[1]&=1;
	for(int i=2;i<=n;i++)
	{
		k=m+i-1;
		while(k%2==0&&k!=0)
		{
			k>>=1;
			bj++;
		}
		k=i;
		while(k%2==0)
		{
			k>>=1;
			bj--;
		}
		if(bj)C[i]=0;
		else C[i]=1;
	}
	for(int i=0;i<=17;i++)
	for(int j=1;j<=n;j++)
	ans[i][j-1]=((a[j]&(1<<i))!=0);
	int n1=2,bit;
	l=n;
	for(bit=1;(1<<bit)<(l<<1);bit++)n1<<=1;
	for(int i=1;i<n1;i++)rev[i]=(rev[i>>1]>>1)|((i&1)<<(bit-1));
	ntt(C,n1,1);
	for(int i=0;i<=17;i++)
	{
		for(int j=0;j<n1;j++)
		b[j]=ans[i][j];
//      for(int j=0;j<n1;j++)
//		cout<<b[j]<<" ";
//		cout<<"\n";
		ntt(b,n1,1);
//		for(int j=0;j<n;j++)
//		cout<<b[j]<<" ";
//		cout<<"\n";
		for(int j=0;j<n1;j++)
		b[j]=1ll*b[j]*C[j]%mod;
		ntt(b,n1,-1);
//		for(int j=0;j<n1;j++)
//		cout<<b[j]<<" ";
//		cout<<"\n";
		for(int j=0;j<n1;j++)
		ans[i][j]=b[j];
	}
	for(int i=0;i<n;i++)
	{
		for(int j=0;j<=17;j++)
		sum[i]+=(ans[j][i]&1)<<j;
	}
	for(int i=0;i<n-1;i++)
	printf("%d ",sum[i]);
	printf("%d",sum[n-1]);
	return 0;
}
```

#### J.容斥原理计数

首先a,b数组的顺序不会影响答案，先将其按照升序排序。
$$
从左上角开始，取q=min(a_1,b_1)
$$
设最大值等于p的有y行x列，其形成一个L的图形，考虑对L型的计数

由容斥原理其方案数为
$$
\sum_{i=0}^{x}\binom{x}{i}(-1)^i\sum_{j=0}^{y}\binom{y}{j}(-1)^jq^{(mx+ny-xy)-(mi+nj-ij)}(q-1)^{mi+nj-ij}
$$
整理后得
$$
q^{mx+ny-xy}\sum_{i=0}^{x}\binom{x}{i}(-1)^i(\frac{q-1}{q})^{mi}\sum_{i=0}^{y}\binom{y}{j}(-1)^j(\frac{q-1}{q})^{j(n-i)}
$$
因为
$$
（x+y）^n=\sum_{i=0}^{n}\binom{n}{i}x^iy^{n-i}
$$
令y=1则有
$$
（x+1）^n=\sum_{i=0}^{n}\binom{n}{i}x^i
$$
再令x=-x
$$
（-x+1）^n=\sum_{i=0}^{n}\binom{n}{i}(-x)^i=\sum_{i=0}^{n}\binom{n}{i}(-1)^ix^i
$$
因此上式可进一步化简为
$$
q^{mx+ny-xy}\sum_{i=0}^{x}\binom{x}{i}(-1)^i(\frac{q-1}{q})^{mi}(1-(\frac{q-1}{q})^{n-i})^y
$$
将每个L型的数目乘起来就是答案

注意不要用递归计算，会爆栈

```cpp
#include <bits/stdc++.h>
using namespace std; 
const int mod=998244353;
int ksm(int a,int b)
{
	int ans=1;
	while(b)
	{
		if(b&1)ans=1ll*ans*a%mod;
		a=1ll*a*a%mod;
		b>>=1;
	}
	return ans;
}
int a[100010],b[100010],inv1[100010];
long long f(int x1,int y1,int n,int m)
{
	long long daan=1;
	for(;m+n>0;)
	{
	int q=min(a[x1],b[y1]);
	int x2=x1,y2=y1;
	while(a[x2]==q)x2++;
	while(b[y2]==q)y2++;
	long long x=x2-x1,y=y2-y1;
	long long ans=ksm(q,(m*x+n*y-x*y)%(mod-1));
	long long ans1=0;
	long long inv=1ll*(q-1)*ksm(q,mod-2)%mod;
	long long xx=ksm(inv,m),zhi=1;
	ans1+=ksm(1-ksm(inv,n)+mod,y);
	ans1%=mod;
	long long zuhe=1,flag=1;
	for(int i=1;i<=x;i++)
	{
		flag*=-1;
		zuhe=zuhe*inv1[i]%mod;
		zuhe=zuhe*(x-i+1)%mod;
		zhi=zhi*xx%mod;
		if(flag==1)
		ans1+=zuhe*zhi%mod*ksm((1-ksm(inv,n-i)+mod)%mod,y)%mod;
		else 
		ans1-=zuhe*zhi%mod*ksm((1-ksm(inv,n-i)+mod)%mod,y)%mod;
		ans1%=mod;
		ans1=(ans1+mod)%mod;
	}
	daan=daan*ans%mod*ans1%mod;
	x1=x2;y1=y2;n-=x;m-=y;
    }
    return daan;
}
int main()
{
    int n,m;
    scanf("%d%d",&n,&m);
    inv1[1]=1;
    for(int i=2;i<=100000;i++)
    inv1[i]=mod-1ll*(mod/i)*inv1[mod%i]%mod;
    for(int i=1;i<=n;i++)
    scanf("%d",&a[i]);
    for(int j=1;j<=m;j++)
    scanf("%d",&b[j]);
    sort(a+1,a+n+1);
    sort(b+1,b+m+1);
    cout<<f(1,1,n,m)%mod;
	return 0;	
} 
```

#### B.Polya计数 杜教筛 数论分块

对于长度为n的手镯，其方案数为
$$
\frac{1}{n}\sum_{d|n}\phi(d)g(n/d)
$$

$$
g(x)=fib(x)+fib(x-2)+2[{x\&1=0}]
$$

答案即求
$$
\sum_{n=1}^m\sum_{d|n}\phi(d)g(n/d)
$$
先枚举d得
$$
\sum_{d=1}^m\phi(d)\sum_{i=1}^{\lfloor m/d\rfloor}g(i)
$$

$$
\sum_{d=1}^m\phi(d)可以杜教筛来求，后面的可以矩阵快速幂来求
$$



```cpp
#include <bits/stdc++.h>
using namespace std; 
int smu[5000010],mu[5000010],prm[5000010],tot,b[5000010],B=5000010,nop[5000010],mod;
long long phi[5000010],sphi[5000010];
map<int,long long>mp_phi;
inline void SieveMu(int n)
{
	mu[1]=1;
	for(int i=2;i<=n;i++)
	{
		if(!b[i])
		{
			prm[++tot]=i;
			mu[i]=-1;
		}
		for(int j=1;j<=tot&&i*prm[j]<=n;j++)
		{
			b[i*prm[j]]=1;
			if(i%prm[j]==0)
			{
				mu[i*prm[j]]=0;
				break;
			}
			else
			{
				mu[i*prm[j]]=-mu[i];
			}
		}
	}
}
void getprime(int lim)
{
	nop[1]=1,phi[1]=1;
	for(int i=2;i<=lim;i++)
	{
		if(!nop[i])phi[i]=i-1;
		for(int j=1;j<=tot&&i*prm[j]<=lim;j++)
		{
			nop[i*prm[j]]=1;
			if(i%prm[j]==0)
			{
				phi[i*prm[j]]=phi[i]*prm[j];
				break;
			}
			else phi[i*prm[j]]=phi[i]*(prm[j]-1);
		}
	}
}
long long DujiaoPhi(int n)
{
	if(n<=B)return sphi[n];
	if(mp_phi[n])return mp_phi[n];
	long long res=1ll*n*(n+1)/2;
	for(int i=2,j;i<=n;i=j+1)
	{
		j=n/(n/i);
		res-=(j-i+1)*DujiaoPhi(n/i);
	}
	return mp_phi[n]=res;
}
struct tnode{
	long long a[2][2];
};
tnode mul(tnode a,tnode b)
{
	tnode c;
	c.a[0][0]=(1ll*a.a[0][0]*b.a[0][0]+1ll*a.a[0][1]*b.a[1][0])%mod;
	c.a[1][0]=(1ll*a.a[0][0]*b.a[0][1]+1ll*a.a[0][1]*b.a[1][1])%mod;
	c.a[0][1]=(1ll*a.a[1][0]*b.a[0][0]+1ll*a.a[1][1]*b.a[1][0])%mod;
	c.a[1][1]=(1ll*a.a[1][0]*b.a[0][1]+1ll*a.a[1][1]*b.a[1][1])%mod;
	return c;
}
int ksm(tnode a,int b)
{
	tnode ans;
	ans.a[0][0]=1;
	ans.a[1][0]=0;
	ans.a[0][1]=0;
	ans.a[1][1]=1;
	while(b)
	{
		if(b&1)ans=mul(ans,a);
		a=mul(a,a);
		b>>=1;
	}
	return ans.a[0][1]%mod;
}
int fib(int n)
{
	tnode a;
	a.a[0][0]=1;
	a.a[0][1]=1;
	a.a[1][0]=1;
	a.a[1][1]=0;
	if(n==1)return 0;
	if(n==2)return 1;
	if(n==3)return 1;
	if(n==4)return 2;
	return ksm(a,n-1);
}
int main()
{
    SieveMu(B);
    getprime(B);
    int n;
    int r;
    cin>>n>>mod;
    for(int i=1;i<=B;i++)
    sphi[i]=sphi[i-1]+phi[i];
    long long ans=0;
    for(int l=1,r;l<=n;l=r+1)
    {
    	r=n/(n/l);
    	int m=n/l;
    	ans+=(DujiaoPhi(r)-DujiaoPhi(l-1))%mod*(((1ll*fib(m+4)-1+fib(m+2)-1+mod-1)%mod+m/2*2ll+mod)%mod)%mod;
    	ans%=mod;
	}
	cout<<ans<<"\n";
	return 0;	
} 
```

#### D.推式子+逆元

$$
\frac{1}{n}\sum_{v}\frac{s(v)-1}{s(fa(v))}*s(v)
$$

根据每个点修改时对其他点的期望可以推出如下式子，最后通过逆元求出每个点期望的和再除以n即可。

```cpp
#include<bits/stdc++.h>
using namespace std;
int n,x,cnt;
long long ans;
const long long mod=998244353;
int l[100010],fa[100010];
long long s[100010];
struct edge{
	int w,next;
}e[500010];
long long inv(long long a,long long p)
{
	long long ans=1;
	long long b=p-2;
	a%=p;
	while(b>0)
	{
		if(b&1)
		ans=(ans*a)%p;
		a=(a*a)%p;
		b/=2;
	}
	return ans%p;
}
void add(int x,int y)
{
	e[++cnt].w=y;
	e[cnt].next=l[x];
	l[x]=cnt;
}
int dfs(int x)
{
	for(int i=l[x];i;i=e[i].next)
	s[x]+=dfs(e[i].w);
	s[x]++;
	return s[x];
}
int main()
{
	cin>>n;
	for(int i=2;i<=n;i++)
	{
		cin>>x;
		fa[i]=x;
		add(x,i);
	}
	dfs(1);
	for(int i=2;i<=n;i++)
	{
		ans+=1ll*(s[fa[i]]-1-s[i]+mod)%mod*inv(s[fa[i]]-1,mod)%mod*(s[i])%mod;
		ans%=mod;
	}
	ans*=inv(n,mod);
	ans%=mod;
	cout<<ans;
	return 0;
}
```

