### 普通平衡树 FHQ_Treap：
```cpp
namespace FHQ_Treap
{
	int lson[MAX],rson[MAX];
	int rnd[MAX],val[MAX],sum[MAX],siz[MAX];
	int root,tot;
	int stk[MAX],top;
	inline int randoom(int l=0,int r=INF)
	{
		static mt19937_64 sd(20070707);
		static uniform_int_distribution<int> range(l,r);
		return range(sd);
	}
	inline void pushup(int i)
	{
		siz[i]=siz[lson[i]]+siz[rson[i]]+1;
		sum[i]=sum[lson[i]]+sum[rson[i]]+val[i];
	}
	inline int newnode(int k)
	{
		int now=top?stk[top--]:++tot;
		rnd[now]=randoom(),val[now]=sum[now]=k,siz[now]=1,lson[now]=rson[now]=0;
		return now;
	}
	void split(int now,int k,int &l,int &r)
	{
		if(!now) return l=r=0,void();
		if(val[now]<=k) l=now,split(rson[now],k,rson[now],r);
		else r=now,split(lson[now],k,l,lson[now]);
		pushup(now);
	}
	int merge(int l,int r)
	{
		if(!l||!r) return l|r;
		if(rnd[l]<rnd[r]) return rson[l]=merge(rson[l],r),pushup(l),l;
		else return lson[r]=merge(l,lson[r]),pushup(r),r;
	}
	inline void ins(int k)
	{
		int x=0,y=0;
		split(root,k,x,y);
		root=merge(merge(x,newnode(k)),y);
	}
	inline void del(int k)
	{
		int x=0,y=0,z=0;
		split(root,k,x,z);
		split(x,k-1,x,y);
		stk[++top]=y,y=merge(lson[y],rson[y]);
		root=merge(merge(x,y),z);
	}
	inline int lis(int k)
	{
		int x=0,y=0;
		split(root,k-1,x,y);
		int ans=siz[x]+1;
		root=merge(x,y);
		return ans;
	}
	int kthand(int now,int k)
	{
		if(siz[lson[now]]+1==k) return val[now];
		else if(siz[lson[now]]>=k) return kthand(lson[now],k);
		else return kthand(rson[now],k-siz[lson[now]]-1);
	}
	inline int pre(int k)
	{
		int x=0,y=0;
		split(root,k-1,x,y);
		int ans=kthand(x,siz[x]);
		root=merge(x,y);
		return ans;
	}
	inline int suf(int k)
	{
		int x=0,y=0;
		split(root,k,x,y);
		int ans=kthand(y,1);
		root=merge(x,y);
		return ans;
	}
}
```

### 文艺平衡树 FHQ_Treap：
```cpp
namespace FHQ_Treap
{
	int lson[MAX],rson[MAX];
	int rnd[MAX],val[MAX],siz[MAX],tag[MAX];
	int root,tot;
	int stk[MAX],top;
	inline int randoom(int l=0,int r=INF)
	{
		static mt19937_64 sd(20070707);
		static uniform_int_distribution<int> range(l,r);
		return range(sd);
	}
	inline void pushup(int i)
	{
		siz[i]=siz[lson[i]]+siz[rson[i]]+1;
	}
	inline void pushdown(int i)
	{
		if(tag[i]) Swp(lson[i],rson[i]),tag[lson[i]]^=1,tag[rson[i]]^=1,tag[i]=0;
	}
	void split(int now,int k,int &l,int &r)
	{
		if(!now) return l=r=0,void();
		pushdown(now);
		if(lson[siz[now]]+1<=k) l=now,split(rson[now],k-siz[lson[now]]-1,rson[now],r);
		else r=now,split(lson[now],k,l,lson[now]);
		pushup(now);
	}
	int merge(int l,int r)
	{
		if(!l||!r) return l|r;
		if(rnd[l]<rnd[r]) return pushdown(l),rson[l]=merge(rson[l],r),pushup(l),l;
		else return pushdown(r),lson[r]=merge(l,lson[r]),pushup(r),r;
	}
	inline void reverse(int L,int R)
	{
		int l=0,r=0,mid=0;
		split(root,R,l,r);
		split(l,L-1,l,mid);
		tag[mid]^=1;
		root=merge(merge(l,mid),r);
	}
}
```