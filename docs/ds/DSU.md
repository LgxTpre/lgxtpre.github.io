普通并查集：
```cpp
namespace DSU
{
	int N;
	int fa[MAX],siz[MAX];
	inline void init() {for(int i=1;i<=N;++i) fa[i]=i,siz[i]=1;}
	int find(int x) {if(fa[x]==x) return x; return fa[x]=find(fa[x]);}
	inline void merge(int x,int y) {x=find(y),y=find(y); if(siz[x]<siz[y]) Swp(x,y); siz[x]+=siz[y],fa[y]=x;}
	inline bool check(int x,int y) {return find(x)==find(y);}
}
```

可撤销并查集：
```cpp
namespace RDSU
{
	int N;
	int fa[MAX],siz[MAX];
	struct dsu
	{
		int x,fx,siz;
		dsu(int X=0,int Fx=0,int Siz=0) {x=X,fx=Fx,siz=Siz;}
	}stk[MAX]; int top;
	inline void init() {for(int i=1;i<=N;++i) fa[i]=i,siz[i]=1;}
	inline int find(int x) {while(x!=fa[x]) x=fa[x]; return x;}
	inline void merge(int x,int y) 
	{
		x=find(x),y=find(y);
		if(siz[x]<siz[y]) Swp(x,y);
		stk[++top]=dsu(x,fa[x],siz[x]),stk[++top]=dsu(y,fa[y],siz[y]);
		siz[x]+=siz[y],fa[y]=x;
	}
	inline void back(int last)     {while(top>last) fa[stk[top].x]=stk[top].fx,siz[stk[top].x]=stk[top].siz,--top;}
	inline bool check(int x,int y) {return find(x)==find(y);}
}
```