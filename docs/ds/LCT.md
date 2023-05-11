### 通用模板：

```cpp
namespace Link_Cat_Tree
{
	int fa[MAX],ch[MAX][2],tag[MAX],siz[MAX],val[MAX];
	inline bool get(int i)      {return ch[fa[i]][1]==i;}
	inline bool noroot(int i)   {return ch[fa[i]][get(i)]==i;}
	inline void pushup(int i)   {siz[i]=siz[ch[i][0]]+siz[ch[i][1]]+1;}
	inline void pushdown(int i) {if(tag[i]) Swp(ch[i][0],ch[i][1]),tag[ch[i][0]]^=1,tag[ch[i][1]]^=1,tag[i]=0;}
	inline void pushall(int i)  {if(noroot(i)) pushall(fa[i]); pushdown(i);}
	inline void rotate(int x)
	{
		int y=fa[x],z=fa[y];
		bool k1=get(x),k2=get(y);
		if(noroot(y)) ch[z][k2]=x; fa[x]=z;
		ch[y][k1]=ch[x][!k1],fa[ch[x][!k1]]=y;
		ch[x][!k1]=y,fa[y]=x;
		pushup(y),pushup(x);
	}
	inline void splay(int x)
	{
		pushall(x);
		while(noroot(x))
		{
			int y=fa[x],z=fa[y];
			if(noroot(y)) (get(x)^get(y))?rotate(x):rotate(y);
			rotate(x);
		}
	}
	inline void access(int x)      {for(int y=0;x;y=x,x=fa[x]) splay(x),ch[x][1]=y,pushup(x);}
	inline void makeroot(int x)    {access(x),splay(x),tag[x]^=1;}
	inline int  findroot(int x)    {access(x),splay(x); while(ch[x][0]) pushdown(x),x=ch[x][0]; return splay(x),x;}
	inline void split(int x,int y) {makeroot(x),access(y),splay(y);}
	inline void link(int x,int y)  {makeroot(x); if(findroot(y)!=x) fa[x]=y;}
	inline void cut (int x,int y)  {makeroot(x); if(findroot(y)==x&&fa[y]==x&&!ch[y][0]) fa[y]=ch[x][1]=0,pushup(x);}
	inline bool check(int x,int y) {return findroot(x)==findroot(y);}
}
```