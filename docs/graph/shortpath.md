### dijktra 单源最短路：
```cpp
namespace dijkstra
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX];
	priority_queue<pii> q;
	inline void dijkstra()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis);
		dis[s]=0,q.push(mp(0,s));
		while(!q.empty())
		{
			int now=q.top().second; q.pop();
			if(vis[now]) continue; vis[now]=1;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
					dis[to]=dis[now]+e[i].val,
					q.push(mp(-dis[to],to));
			}
		}
		return;
	}
}
```

### SPFA 单源最短路：
```cpp
namespace SPFA
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX];
	queue<int> q;
	inline void SPFA()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis);
		q.push(s),dis[s]=0,vis[s]=1;
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			vis[now]=0;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
				{
					dis[to]=dis[now]+e[i].val;
					if(!vis[to]) vis[to]=1,q.push(to);
				}
			}
		}
		return;
	}
}
```

### SPFA 判负环：
```cpp
namespace Check_Loop
{
	struct edge{int nex,to,val;}e[MAX<<1];
	int head[MAX],cnt;
	inline void add(int x,int y,int z)
	{
		e[++cnt].nex=head[x],head[x]=cnt,e[cnt].to=y,e[cnt].val=z;
		return;
	}
	
	int dis[MAX],vis[MAX],con[MAX];
	queue<int> q;
	inline bool SPFA()
	{
		memset(dis,0x3f,sizeof dis),memset(vis,0,sizeof vis),memset(con,0,sizeof con);
		q.push(s),dis[s]=0,vis[s]=1;
		while(!q.empty())
		{
			int now=q.front(); q.pop();
			vis[now]=0;
			for(int i=head[now];i;i=e[i].nex)
			{
				int to=e[i].to;
				if(dis[to]>dis[now]+e[i].val)
				{
					dis[to]=dis[now]+e[i].val,con[to]=con[now]+1;
					if(con[now]>=n) return 1;
					if(!vis[to]) vis[to]=1,q.push(to);
				}
			}
		}
		return 0;
	}
}
```

